<properties
   pageTitle="Konfigurace připojení brány VPN typu Point-to-Site ke službě Azure Virtual Network pomocí klasického portálu | Microsoft Azure"
   description="Připojte se bezpečně ke službě Azure Virtual Network vytvořením připojení brány VPN typu Point-to-Site."
   services="vpn-gateway"
   documentationCenter="na"
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
   ms.date="09/27/2016"
   ms.author="cherylmc"/>


# Konfigurace připojení typu Point-to-Site k virtuální síti prostřednictvím portálu Classic

> [AZURE.SELECTOR]
- [Resource Manager – PowerShell](vpn-gateway-howto-point-to-site-rm-ps.md)
- [Classic – klasický portál](vpn-gateway-point-to-site-create.md)

Konfigurace Point-to-Site (P2S) umožňuje vytvořit zabezpečené připojení jednotlivých klientských počítačů k virtuální sítí. Připojení P2S je užitečné, když se chcete ke své virtuální síti připojit ze vzdáleného umístění, například z domova nebo z místa konání konference, nebo když máte jen několik klientů, kteří se potřebují připojovat k virtuální síti.

Tento článek vás provede vytvořením virtuální sítě s připojením Point-to-Site v **klasickém modelu nasazení** s použitím **klasického portálu**.

Připojení typu Point-to-Site k fungování nevyžadují zařízení VPN ani veřejnou IP adresu. Připojení VPN je vytvořeno spuštěním připojení na klientském počítači. Další informace o připojeních typu Point-to-Site najdete v tématech [Služba VPN Gateway – nejčastější dotazy](vpn-gateway-vpn-faq.md#point-to-site-connections) a [Plánování a navrhování](vpn-gateway-plan-design.md).


### Modely a metody nasazení pro přípojení P2S

[AZURE.INCLUDE [vpn-gateway-table-point-to-site](../../includes/vpn-gateway-table-point-to-site-include.md)] 

**O modelech nasazení Azure**

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)] 

![Diagram Point-to-Site](./media/vpn-gateway-point-to-site-create/p2sclassic.png "point-to-site")

## Informace o vytvoření připojení typu Point-to-Site
 
Následující kroky vás provedou postupem vytvoření bezpečného připojení typu Point-to-Site k virtuální síti. 

Konfigurace pro připojení typu Point-to-Site je rozdělena do čtyř oddílů. Pořadí, ve kterém nakonfigurujete každý z těchto oddílů, je důležité. Nevynechávejte kroky ani nepřeskakujte o několik kroků dopředu.

- **Oddíl 1** Vytvoření virtuální sítě a brány VPN.
- **Oddíl 2** Vytvoření certifikátů, které použijete pro ověření, a jejich nahrání.
- **Oddíl 3** Export a instalace klientských certifikátů.
- **Oddíl 4** Konfigurace klienta VPN.

## <a name="vnetvpn"></a>Oddíl 1 – Vytvoření virtuální sítě a brány VPN

### Část 1: Vytvoření virtuální sítě

1. Přihlaste se do [portálu Azure Classic](https://manage.windowsazure.com/). Tyto kroky používají portál Classic, nikoli Azure Portal. V současné době nelze připojení P2S vytvořit pomocí webu Azure Portal.

2. V levém dolním rohu obrazovky klikněte na **Nový**. V podokně navigace klikněte na **Network Services**, a poté klikněte na **Virtual Network**. Kliknutím na **Vytvořit vlastní** spustíte průvodce konfigurací.

3. Na stránce **Podrobnosti virtuální sítě** zadejte následující informace, a poté klikněte na šipku Další vpravo dole.
    - **Název**: Zadejte název virtuální sítě. Například „VNet1“. To je název, na který se budete odkazovat při nasazování virtuálních počítačů a této virtuální sítě.
    - **Umístění**: Umístění přímo souvisí s fyzickým umístěním (oblastí), kde chcete mít umístěné prostředky (virtuální počítače). Například pokud chcete, aby virtuální počítače, které do této virtuální sítě nasadíte, byly fyzicky umístěné ve východní USA, vyberte toto umístění. Oblast přidruženou k virtuální síti nebude možné po jejím vytvoření změnit.

4. Na stránce **Servery DNS a připojení VPN** zadejte následující informace, a poté klikněte na šipku Další vpravo dole.
    - **Servery DNS**: Zadejte název serveru DNS a IP adresu, nebo vyberte dříve zaregistrovaný server DNS z místní nabídky. Toto nastavení neslouží k vytvoření serveru DNS. Umožňuje určit server DNS, který chcete použít pro překlad názvů pro tuto virtuální síť. Chcete-li používat výchozí službu překladu IP adres Azure, ponechte tuto sekci prázdnou.
    - **Konfigurování sítě Point-to-Site VPN**: Zaškrtněte toto políčko.

5. Na stránce **Připojení typu Point-to-Site** zadejte rozsah IP adres, ze kterého klienti sítě VPN obdrží po připojení IP adresu. Existuje několik pravidel ohledně rozsahů adres, které lze zadat. Je velmi důležité ověřit, že se rozsah, který zadáte, nepřekrývá s žádným z rozsahů ve vaší místní síti.

6. Zadejte následující informace, a poté klikněte na šipku Další.
 - **Adresní prostor**: Zahrnuje počáteční IP adresu a CIDR (počet adres).
 - **Přidání adresního prostoru**: Adresní prostor přidejte pouze pokud to vyžaduje návrh vaší sítě.

7. Na stránce **Adresní prostory virtuální sítě** zadejte rozsah adres, které chcete použít pro vaši virtuální síť. Toto jsou dynamické IP adresy (vyhrazené IP adresy), které budou přiřazené pro virtuální počítače a další instance rolí, které nasadíte do této virtuální sítě.<br><br>Je obzvlášť důležité vybrat rozsah, který se nepřekrývá s žádným z rozsahů, které se používají ve vaší místní síti. Budete se muset domluvit se správcem vaší sítě, který možná bude muset vyčlenit z adresního prostoru místní sítě rozsah IP adres, které budete moci použít pro virtuální síť.

8. Zadejte následující informace, a poté kliknutím na značku zaškrtnutí zahájíte vytváření virtuální sítě.
 - **Adresní prostor**: Přidejte rozsah interních IP adres, které chcete používat pro tuto virtuální síť, včetně počáteční IP adresy a počtu. Je důležité vybrat rozsah, který se nepřekrývá s žádným z rozsahů, které se používají ve vaší místní síti. 
 - **Přidání podsítě**: Dodatečné podsítě nejsou vyžadovány, ale možná budete chtít vytvořit samostatnou podsíť pro virtuální počítače, které budou mít statické vyhrazené IP adresy. Nebo možná budete chtít mít virtuální počítače v podsíti oddělené od dalších instancí rolí.
 - **Přidání podsítě brány**: Podsíť brány je vyžadována pro síť Point-to-Site VPN. Klikněte pro přidání podsítě brány. Podsíť brány se používá pouze pro bránu virtuální sítě.

9. Po vytvoření virtuální sítě uvidíte na portálu Azure Classic na stránce sítí v části **Stav** stav **Vytvořená**. Po vytvoření virtuální sítě můžete vytvořit bránu dynamického směrování.

### Část 2: Vytvoření brány dynamického směrování

Typ brány musí být nakonfigurován jako dynamický. Brány statického směrování nebudou s touto funkcí fungovat.

1. V klasickém portálu Azure na stránce **Sítě** klikněte na virtuální síť, kterou jste právě vytvořili, a přejděte na stránku **Řídicí panel**.

2. Klikněte na tlačítko **Vytvořit bránu** umístěné v dolní části stránky **Řídicí panel**. Zobrazí se zpráva s dotazem **Opravdu chcete vytvořit bránu pro virtuální síť „VNet1“**. Kliknutím na **Ano** zahájíte proces vytváření brány. Vytvoření brány může trvat přibližně 15 minut.

## <a name="generate"></a>Oddíl 2 – Generování a nahrávání certifikátů

Certifikáty slouží k ověřování klientů VPN pro sítě Point-to-Site VPN. Můžete použít kořenový certifikát vygenerovaný podnikovým certifikačním řešením, stejně jako certifikát podepsaný svým držitelem. Do Azure můžete nahrát až 20 kořenových certifikátů. Po nahrání souboru .cer Azure informace obsažené v něm slouží k ověřování klientů, které mají nainstalovaný certifikát klienta. Certifikát klienta je nutné vygenerovat ze stejného certifikátu, který představuje soubor .cer.

V této části provedete následující:

- Získejte soubor .cer pro kořenový certifikát. Může jím být buď certifikát podepsaný svým držitelem nebo můžete použít podnikový certifikační systém.
- Nahrajte soubor .cer do Azure.
- Vygenerujte klientské certifikáty.

### <a name="root"></a>Část 1: Získání souboru .cer pro kořenový certifikát

Pokud používáte podnikový certifikační systém, získejte soubor .cer pro kořenový certifikát, který chcete použít. V [části 3](#createclientcert) vygenerujete certifikáty klientů z kořenového certifikátu.

Pokud nepoužíváte podnikové certifikační řešení, budete muset vygenerovat kořenový certifikát podepsaný svým držitelem. Kroky pro Windows 10 najdete v tématu [Práce s kořenovými certifikáty podepsanými svým držitelem pro konfigurace Point-to-Site](vpn-gateway-certificates-point-to-site.md). Článek vás provede postupem použití nástroje makecert k vygenerování certifikátu podepsaného svým držitelem a jeho následným exportem do souboru .cer.

### <a name="upload"></a>Část 2: Nahrání souboru .cer s kořenovým certifikátem do portálu Azure Classic

Přidejte do Azure důvěryhodný certifikát. Pokud do Azure přidáte soubor X.509 zakódovaný ve formátu Base64 (.cer), oznamujete tím Azure, že má důvěřovat kořenovému certifikátu, který tento soubor představuje.

1. V portálu Azure Classic na stránce **Certifikáty** pro vaši virtuální síť klikněte na **Nahrát kořenový certifikát**.

2. Na stránce **Nahrání certifikátu** vyhledejte soubor .cer kořenového certifikátu, a poté klikněte na značku zaškrtnutí.

### <a name="createclientcert"></a>Část 3: Vygenerování klientského certifikátu

Dále vygenerujte certifikáty klientů. Můžete buď vygenerovat jedinečný certifikát pro každého klienta, který se připojí, nebo můžete použít stejný certifikát na více klientů. Výhodou generování jedinečných certifikátů pro klienty je možnost v případě potřeby jednotlivý certifikát odvolat. V opačném případě, pokud všichni používají stejný certifikát klienta a zjistíte, že pro jednoho klienta je třeba tento certifikát odvolat, bude nutné vygenerovat a nainstalovat nové certifikáty pro všechny klienty, u nichž je odvolaný certifikát taktéž používán k ověření.

- Pokud používáte podnikové certifikační řešení, vygenerujte certifikát klienta se společným názvem ve formátu „jmeno@vasedomena.com", spíše než formátu NetBIOS „Doména\uživatelské jméno“. 

- Používáte-li certifikát podepsaný svým držitelem, naleznete informace o vygenerování certifikátu klienta v tématu [Práce s kořenovými certifikáty podepsanými svými držiteli pro účely konfigurace připojení Point-to-Site](vpn-gateway-certificates-point-to-site.md).

## <a name="installclientcert"></a>Oddíl 3 – Export a instalace klientského certifikátu

Nainstalujte certifikát klienta v každém počítači, který se má připojovat k virtuální síti. Klientský certifikát je vyžadován pro ověřování. Instalaci klientského certifikátu lze buď automatizovat, nebo ji lze provádět ručně. Následující kroky vás provedou procesem exportu a ruční instalace klientského certifikátu.

1. Chcete-li exportovat klientský certifikát, použijte nástroj *certmgr.msc*. Klikněte pravým tlačítkem na klientský certifikát, který chcete exportovat, klikněte na **všechny úlohy**, a poté klikněte na **exportovat**.
2. Exportujte klientský certifikát s privátním klíčem. Jedná se o soubor *.pfx*. Ujistěte se, že jste si poznamenali nebo si pamatujete heslo (klíč), které jste pro tento certifikát nastavili.
3. Zkopírujte soubor *.pfx* na klientský počítač. Na klientském počítači spusťte instalaci dvojím kliknutím na soubor *.pfx*. Až budete vyzváni, zadejte heslo. Neměňte umístění instalace.

## <a name="vpnclientconfig"></a>Oddíl 4 – Konfigurace klienta VPN

Chcete-li se připojit k virtuální síti, je potřeba nakonfigurovat také klienta VPN. Pro připojení klient vyžaduje klientský certifikát a správnou konfiguraci klienta VPN. Klienta VPN nakonfigurujete pomocí následujících kroků ve správném pořadí.

### Část 1: Vytvoření konfiguračního balíčku klienta VPN

1. Na portálu Azure Classic na stránce **Řídicí panel** u vaší virtuální sítě přejděte do rychlé nabídky , přejděte do nabídky rychlého přehledu v pravém horním rohu. Seznam podporovaných klientských operačních systémů naleznete v části [Připojení Point-to-Site](vpn-gateway-vpn-faq.md#point-to-site-connections) v tématu Služba VPN Gateway - nejčastější dotazy. Balíček klienta VPN obsahuje informace o konfiguraci pro konfigurování klientského softwaru VPN, který je součástí Windows. Balíček neinstaluje další software. Nastavení jsou specifická pro virtuální síť, ke které se chcete připojit.<br><br>Vyberte balíček ke stažení, který odpovídá operačnímu systému klienta, na kterém bude nainstalován.
 - Pro 32bitové klienty vyberte **Stáhnout 32bitovou verzi balíčku klienta VPN**.
 - Pro 64bitové klienty vyberte **Stáhnout 64bitovou verzi balíčku klienta VPN**.

2. Vytvoření vašeho klientského balíčku bude trvat několik minut. Když je balíček hotový, můžete soubor stáhnout. Soubor *.exe*, který stáhnete, můžete bezpečně uložit na místním počítači.

3. Po vygenerování a stažení balíčku klienta VPN z portálu Azure Classic můžete balíček klienta nainstalovat na klientský počítač, ze kterého se chcete připojit k virtuální síti. Pokud plánujete nainstalovat balíček klienta VPN na více klientských počítačů, ujistěte se, že je na každém z nich nainstalovaný také klientský certifikát.

### Část 2: Instalace konfiguračního balíčku sítě VPN na klienta

1. Zkopírujte konfigurační soubor do počítače, který chcete připojit k virtuální síti a dvakrát klikněte na soubor .exe. 

2. Jakmile je balíček nainstalován, můžete zahájit připojení VPN. Konfigurační balíček není podepsán společností Microsoft. Balíček můžete chtít podepsat pomocí podpisové služby vaší organizace, nebo podepsat sami pomocí nástroje [SignTool]( http://go.microsoft.com/fwlink/p/?LinkId=699327). Balíček lze použít i bez podepsání. Nicméně pokud balíček není podepsaný, zobrazí se během instalace balíčku upozornění.

3. Na klientském počítači přejděte do části **Nastavení sítě** a klikněte na možnost **Síť VPN**. Zobrazí se příslušné připojení. Bude uveden název virtuální sítě, ke kterému se bude počítač připojovat. Název bude vypadat podobně jako tento: 

    ![Klient VPN](./media/vpn-gateway-point-to-site-create/vpn.png "VPN client")


### Část 3: Připojení k Azure

1. Chcete-li se připojit ke své síti VNet, přejděte na klientském počítači na připojení VPN a vyhledejte připojení VPN, které jste vytvořili. Bude mít stejný název jako vaše virtuální síť. Klikněte na **Připojit**. Může se zobrazit místní zpráva týkající se použití certifikátu. Pokud k tomu dojde, klikněte na možnost **Pokračovat**, abyste použili zvýšená oprávnění. 

2. Připojení spustíte kliknutím na tlačítko **Připojit** na stavové stránce **Připojení**. Pokud uvidíte obrazovku **Výběr certifikátu**, ujistěte se, že zobrazený klientský certifikát je ten, který chcete pro připojení použít. Pokud není, vyberte pomocí šipky rozevíracího seznamu správný certifikát, a poté klikněte na **OK**.

    ![Klient VPN 2](./media/vpn-gateway-point-to-site-create/clientconnect.png "VPN client connection")

3. Připojení by nyní mělo být navázáno.

    ![Klient VPN 3](./media/vpn-gateway-point-to-site-create/connected.png "VPN client connection 2")

### Část 4: Ověření připojení VPN

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

Další informace o virtuálních sítích najdete na stránce [Dokumentace k virtuálním sítím](https://azure.microsoft.com/documentation/services/virtual-network/).



<!--HONumber=Sep16_HO4-->


