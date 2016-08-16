<properties
   pageTitle="Konfigurace připojení VPN typu Point-to-Site ke službě Azure Virtual Network pomocí klasického portálu | Microsoft Azure"
   description="Připojte se bezpečně ke službě Azure Virtual Network vytvořením připojení VPN typu Point-to-Site. Pokyny pro virtuální sítě vytvořené pomocí modelu nasazení správou služeb (model nasazení Classic)."
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
   ms.date="03/30/2016"
   ms.author="cherylmc"/>

# Konfigurace připojení VPN typu Point-to-Site k virtuální síti pomocí klasického portálu

> [AZURE.SELECTOR]
- [PowerShell – Resource Manager](vpn-gateway-howto-point-to-site-rm-ps.md)
- [Klasický portál](vpn-gateway-point-to-site-create.md)

Konfigurace Point-to-Site vám umožňuje vytvoření bezpečného připojení k virtuální síti z jednotlivých klientských počítačů. Připojení VPN je vytvořeno spuštěním připojení na klientském počítači. Point-to-Site je vynikající řešení, pokud se chcete připojit k virtuální síti ze vzdáleného umístění, například z domova nebo z konference, nebo pokud máte pouze několik klientů, kteří se potřebují připojit k virtuální síti. 

Připojení typu Point-to-Site k fungování nevyžadují zařízení VPN ani veřejnou IP adresu. Další informace o připojeních typu Point-to-Site najdete v tématech [Služba VPN Gateway – nejčastější dotazy](vpn-gateway-vpn-faq.md#point-to-site-connections) a [Informace o připojeních mezi různými místy](vpn-gateway-cross-premises-options.md).

Tento článek se týká připojení typu Point-to-Site služby VPN Gateway k virtuální síti vytvořené pomocí **modelu nasazení Classic** (správou služeb) a portálu Classic. Až dokončíme článek s kroky pro Azure Portal, odkaz na něj přidáme na tuto stránku.

**Modely nasazení a nástroje pro připojení typu Point-to-Site**

[AZURE.INCLUDE [vpn-gateway-table-point-to-site](../../includes/vpn-gateway-table-point-to-site-include.md)] 


**O modelech nasazení Azure**

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)] 

![Diagram Point-to-Site](./media/vpn-gateway-point-to-site-create/point2site.png "point-to-site")



## Informace o vytvoření připojení typu Point-to-Site
 
Následující kroky vás provedou postupem vytvoření bezpečného připojení typu Point-to-Site k virtuální síti. Přestože konfigurace připojení typu Point-to-Site vyžaduje více kroků, je to skvělý způsob, jak zajistit bezpečné připojení mezi vaším počítačem a virtuální sítí bez nutnosti pořizování a konfigurace zařízení VPN. 

Konfigurace pro připojení typu Point-to-Site je rozdělena do čtyř oddílů. Pořadí, v jakém probíhá konfigurace, je důležité, proto kroky nepřeskakujte.


- **Oddíl 1** vás provede vytvořením virtuální sítě a brány VPN.
- **Oddíl 2** vám pomůže vytvořit a nahrát certifikáty pro ověřování.
- **Oddíl 3** vás provede exportem a instalací klientských certifikátů.
- **Oddíl 4** vás provede konfigurací klienta VPN.

## Oddíl 1 – Vytvoření virtuální sítě a brány VPN


### Část 1: Vytvoření virtuální sítě

1. Přihlaste se do [portálu Azure Classic](https://manage.windowsazure.com/). Tyto kroky používají klasický portál, ne Azure Portal.

2. V levém dolním rohu obrazovky klikněte na **Nový**. V podokně navigace klikněte na **Network Services**, a poté klikněte na **Virtual Network**. Kliknutím na **Vytvořit vlastní** spustíte průvodce konfigurací.

3. Na stránce **Podrobnosti virtuální sítě** zadejte následující informace, a poté klikněte na šipku Další vpravo dole.
    - **Název**: Zadejte název virtuální sítě. Například „VNetEast“. To je název, na který se budete odkazovat při nasazování virtuálních počítačů a instancí PaaS do této virtuální sítě.
    - **Umístění**: Umístění přímo souvisí s fyzickým umístěním (oblastí), kde chcete mít umístěné prostředky (virtuální počítače). Například pokud chcete, aby virtuální počítače, které do této virtuální sítě nasadíte, byly fyzicky umístěné ve východní USA, vyberte toto umístění. Oblast přidruženou k virtuální síti nebude možné po jejím vytvoření změnit.

4. Na stránce **Servery DNS a připojení VPN** zadejte následující informace, a poté klikněte na šipku Další vpravo dole.
    - **Servery DNS**: Zadejte název serveru DNS a IP adresu, nebo vyberte dříve zaregistrovaný server DNS z místní nabídky. Toto nastavení nevytvoří server DNS, ale umožňuje vám určit servery DNS, které chcete používat pro překlad IP adres u této virtuální sítě. Chcete-li používat výchozí službu překladu IP adres Azure, ponechte tuto sekci prázdnou.
    - **Konfigurování sítě Point-to-Site VPN**: Zaškrtněte toto políčko.

5. Na stránce **Připojení typu Point-to-Site** zadejte rozsah IP adres, ze kterého klienti sítě VPN obdrží po připojení IP adresu. Existuje několik pravidel ohledně rozsahů adres, které lze zadat. Je velmi důležité ověřit, že se rozsah, který zadáte, nepřekrývá s žádným z rozsahů ve vaší místní síti.

6. Zadejte následující informace, a poté klikněte na šipku Další.
 - **Adresní prostor**: Zahrnuje počáteční IP adresu a CIDR (počet adres).
 - **Přidání adresního prostoru**: Adresní prostor přidejte pouze pokud to vyžaduje návrh vaší sítě.

7. Na stránce **Adresní prostory virtuální sítě** zadejte rozsah adres, které chcete použít pro vaši virtuální síť. Toto jsou dynamické IP adresy (vyhrazené IP adresy), které budou přiřazené pro virtuální počítače a další instance rolí, které nasadíte do této virtuální sítě. Je obzvlášť důležité vybrat rozsah, který se nepřekrývá s žádným z rozsahů, které se používají ve vaší místní síti. Budete se muset domluvit se správcem vaší sítě, který možná bude muset vyčlenit z adresního prostoru místní sítě rozsah IP adres, které budete moci použít pro virtuální síť.

8. Zadejte následující informace, a poté kliknutím na značku zaškrtnutí zahájíte vytváření virtuální sítě.
 - **Adresní prostor**: Přidejte rozsah interních IP adres, které chcete používat pro tuto virtuální síť, včetně počáteční IP adresy a počtu. Je důležité vybrat rozsah, který se nepřekrývá s žádným z rozsahů, které se používají ve vaší místní síti. Budete se muset domluvit se správcem vaší sítě, který možná bude muset vyčlenit z adresního prostoru místní sítě rozsah IP adres, které budete moci použít pro virtuální síť.
 - **Přidání podsítě**: Dodatečné podsítě nejsou vyžadovány, ale možná budete chtít vytvořit samostatnou podsíť pro virtuální počítače, které budou mít statické vyhrazené IP adresy. Nebo možná budete chtít mít virtuální počítače v podsíti oddělené od dalších instancí rolí.
 - **Přidání podsítě brány**: Podsíť brány je vyžadována pro síť Point-to-Site VPN. Klikněte pro přidání podsítě brány. Podsíť brány se používá pouze pro bránu virtuální sítě.

9. Po vytvoření virtuální sítě uvidíte stav **Vytvořená** v části **Stav** na stránce sítí na portálu Azure Classic. Po vytvoření virtuální sítě můžete vytvořit bránu dynamického směrování.

### Část 2: Vytvoření brány dynamického směrování

Typ brány musí být nakonfigurován jako dynamický. Brány statického směrování nebudou s touto funkcí fungovat.

1. V portálu Azure Classic na stránce **Sítě** klikněte na virtuální síť, kterou jste právě vytvořili, a přejděte na stránku **Řídicí panel**.

2. Klikněte na tlačítko **Vytvořit bránu** umístěné v dolní části stránky **Řídicí panel**. Zobrazí se zpráva s dotazem **Opravdu chcete vytvořit bránu pro virtuální síť „vaše_síť“?**. Kliknutím na **Ano** zahájíte proces vytváření brány. Vytvoření brány může trvat přibližně 15 minut.

## Oddíl 2 – Generování a nahrávání certifikátů

Certifikáty slouží k ověřování klientů VPN pro sítě Point-to-Site VPN. Můžete použít certifikáty vygenerované podnikovým certifikačním řešením, stejně jako certifikáty podepsané svým držitelem. Do Azure můžete nahrát až 20 kořenových certifikátů.

Chcete-li použít certifikát podepsaný svým držitelem, pomůžou vám s tím následující kroky. Plánujete-li použít podnikové certifikační řešení, budou se kroky v jednotlivých částech lišit, stále však budete muset provést následující kroky:

### Část 1: Určení nebo vygenerování kořenového certifikátu

Pokud nepoužíváte podnikové certifikační řešení, budete muset vygenerovat kořenový certifikát podepsaný svým držitelem. Kroky v této části byly napsány pro Windows 8. Kroky pro Windows 10 najdete v tématu [Práce s kořenovými certifikáty podepsanými svým držitelem pro konfigurace Point-to-Site](vpn-gateway-certificates-point-to-site.md).

Jeden způsob jak vytvořit certifikát X.509 je pomocí nástroje vytvoření certifikátu (makecart.exe). Chcete-li použít nástroj makecert, stáhněte a nainstalujte si bezplatně sadu [Microsoft Visual Studio Express](https://www.visualstudio.com/products/visual-studio-express-vs.aspx).

1. Přejděte do složky Microsoft Visual Studio Tools a spusťte příkazový řádek v režimu správce.

2. Příkaz v následujícím příkladu vytvoří a nainstaluje kořenový certifikát do Osobního úložiště certifikátů na vašem počítači, a vytvoří odpovídající soubor *.cer*, který později nahrajete do portálu Azure Classic.

3. Přejděte do adresáře, kam chcete umístit soubor .cer a spusťte následující příkaz, kde *RootCertificateName* je název, který chcete použít pro certifikát. Spustíte-li následující příklad beze změn, výsledkem bude kořenový certifikát a odpovídající soubor *RootCertificateName.cer*.

>[AZURE.NOTE] Vzhledem k tomu, že jste vytvořili kořenový certifikát, ze kterého se budou generovat klientské certifikáty, měli byste tento certifikát exportovat i s jeho privátním klíčem, a uložit jej na bezpečné místo, odkud ho bude možné obnovit.

    makecert -sky exchange -r -n "CN=RootCertificateName" -pe -a sha1 -len 2048 -ss My "RootCertificateName.cer"

### Část 2: Nahrání souboru .cer s kořenovým certifikátem do portálu Azure Classic

Pro každý kořenový certifikát budete muset nahrát odpovídající soubor .cer do Azure. Můžete nahrát až 20 certifikátů.

1. Při generování kořenového certifikátu v předchozím kroku jste vytvořili také soubor *.cer*. Nyní tento soubor nahrajete do portálu Azure Classic. Všimněte si, že soubor .cer neobsahuje privátní klíč kořenového certifikátu. Můžete nahrát až 20 kořenových certifikátů.

2. V portálu Azure Classic na stránce **Certifikáty** pro vaši virtuální síť klikněte na **Nahrát kořenový certifikát**.

3. Na stránce **Nahrání certifikátu** vyhledejte soubor .cer kořenového certifikátu, a poté klikněte na značku zaškrtnutí.

### Část 3: Vygenerování klientského certifikátu

Následující kroky jsou určeny pro vygenerování klientského certifikátu z kořenového certifikátu podepsaného svým držitelem. Kroky v této části byly napsány pro Windows 8. Kroky pro Windows 10 najdete v tématu [Práce s kořenovými certifikáty podepsanými svým držitelem pro konfigurace Point-to-Site](vpn-gateway-certificates-point-to-site.md). Pokud používáte podnikové certifikační řešení, postupujte podle pokynů pro toto řešení. 

1. Na počítači, který jste použili pro vytvoření kořenového certifikátu podepsaného svým držitelem, otevřete příkazový řádek sady Visual Studio v režimu správce.

2. Změňte adresář na umístění, kam chcete uložit soubor s klientským certifikátem. *RootCertificateName* odkazuje na kořenový certifikát podepsaný svým držitelem, který jste vygenerovali. Spustíte-li následující příklad (a změníte RootCertificateName na název vašeho kořenového certifikátu), výsledkem bude klientský certifikát s názvem „ClientCertificateName“ v Osobním úložišti certifikátů.

3. Zadejte následující příkaz:

        makecert.exe -n "CN=ClientCertificateName" -pe -sky exchange -m 96 -ss My -in "RootCertificateName" -is my -a sha1

4. Všechny certifikáty jsou uloženy v Osobním úložišti certifikátů na vašem počítači. Ověříte to pomocí nástroje *certmgr*. Tímto postupem můžete vygenerovat tolik klientských certifikátů, kolik je potřeba. Doporučujeme vám vytvořit jedinečný klientský certifikát pro každý počítač, který chcete připojit k virtuální síti.

## Oddíl 3 – Export a instalace klientského certifikátu

Instalace klientského certifikátu na každý počítač, který chcete připojit k virtuální síti, je povinný krok. Následující kroky vás provedou ruční instalací klientského certifikátu.

1. Klientský certifikát musí být nainstalován na každém počítači, který chcete připojit k virtuální síti. To znamená, že pravděpodobně vytvoříte více klientských certifikátů, které poté budete potřebovat exportovat. Chcete-li exportovat klientské certifikáty, použijte *certmgr.msc*. Klikněte pravým tlačítkem na klientský certifikát, který chcete exportovat, klikněte na **všechny úlohy**, a poté klikněte na **exportovat**.
2. Exportujte *klientský certifikát* s privátním klíčem. Bude to soubor *.pfx*. Ujistěte se, že jste si poznamenali nebo si pamatujete heslo (klíč), které jste pro tento certifikát nastavili.
3. Zkopírujte soubor *.pfx* na klientský počítač. Na klientském počítači spusťte instalaci dvojím kliknutím na soubor *.pfx*. Až budete vyzváni, zadejte heslo. Neměňte umístění instalace.

## Oddíl 4 – Konfigurace klienta VPN

Chcete-li se připojit k virtuální síti, bude potřeba nakonfigurovat také klienta VPN. Pro připojení klient vyžaduje klientský certifikát a správnou konfiguraci klienta VPN. Klienta VPN nakonfigurujete pomocí následujících kroků ve správném pořadí.

### Část 1: Vytvoření konfiguračního balíčku klienta VPN

1. V portálu Azure Classic na stránce **Řídicí panel** vaší virtuální sítě přejděte do nabídky rychlého přehledu v pravém rohu, a klikněte na balíček VPN, který se týká klienta, kterého chcete připojit k virtuální síti.

2. 
Podporovány jsou následující operační systémy:
 - Windows 7 (32bitové a 64bitové verze)
 - Windows Server 2008 R2 (pouze 64bitové verze)
 - Windows 8 (32bitové a 64bitové verze)
 - Windows 8.1 (32bitové a 64bitové verze)
 - Windows Server 2012 (pouze 64bitové verze)
 - Windows Server 2012 R2 (pouze 64bitové verze)
 - Windows 10

3. Vyberte balíček ke stažení, který odpovídá operačnímu systému klienta, na kterém bude nainstalován.
 - Pro 32bitové klienty vyberte **Stáhnout 32bitovou verzi balíčku klienta VPN**.
 - Pro 64bitové klienty vyberte **Stáhnout 64bitovou verzi balíčku klienta VPN**.

4. Vytvoření vašeho balíčku bude trvat několik minut. Když je balíček hotový, můžete soubor stáhnout. Soubor *.exe*, který stáhnete, můžete bezpečně uložit na místním počítači.

5. Po vygenerování a stažení balíčku klienta VPN z portálu Azure Classic můžete balíček klienta nainstalovat na klientský počítač, ze kterého se chcete připojit k virtuální síti. Pokud plánujete nainstalovat balíček klienta VPN na více klientských počítačů, ujistěte se, že je na každém z nich nainstalovaný také klientský certifikát. Balíček klienta VPN obsahuje informace o konfiguraci pro konfigurování klientského softwaru VPN, který je součástí Windows. Balíček neinstaluje další software.

### Část 2: Instalace balíčku konfigurace VPN na klienta a zahájení připojení

1. Zkopírujte konfigurační soubor na počítač, který chcete připojit k virtuální síti a dvakrát klikněte na soubor .exe. Jakmile je balíček nainstalován, můžete zahájit připojení VPN.
Všimněte si, že konfigurační balíček není podepsán společností Microsoft. Balíček můžete chtít podepsat pomocí podpisové služby vaší organizace, nebo podepsat sami pomocí nástroje [SignTool]( http://go.microsoft.com/fwlink/p/?LinkId=699327). Balíček lze použít i bez podepsání. Nicméně pokud balíček není podepsaný, zobrazí se během instalace balíčku upozornění.
2. Na klientském počítači přejděte na připojení VPN a vyhledejte právě vytvořené připojení VPN. Bude mít stejný název jako vaše virtuální síť. Klikněte na **Připojit**.
3. Zobrazí se místní zpráva, která slouží k vytvoření certifikátu podepsaného svým držitelem pro koncový bod služby Gateway. Klikněte na **Pokračovat** pro použití zvýšených oprávnění.
4. Kliknutím na **Připojit** na stavové stránce **Připojení** zahájíte připojení.
5. Pokud uvidíte obrazovku **Výběr certifikátu**, ujistěte se, že zobrazený klientský certifikát je ten, který chcete pro připojení použít. Pokud není, vyberte pomocí šipky rozevíracího seznamu správný certifikát, a poté klikněte na **OK**.
6. Nyní jste připojeni ke svojí virtuální síti a máte úplný přístup ke všem službám a virtuálním počítačům hostovaným ve virtuální síti.

### Část 3: Ověření připojení VPN

1. Chcete-li ověřit, zda je připojení VPN aktivní, v příkazovém řádku se zvýšenými oprávněními spusťte příkaz *ipconfig/all*.
2. Zkontrolujte výsledky. Všimněte si, že IP adresa, kterou jste obdrželi, je jedna z adres z rozsahu adres připojení typu Point-to-Site, který jste určili během vytváření vaší virtuální sítě. Výsledek by se měl podobat následujícímu příkladu:

Příklad:



    PPP adapter VNetEast:
        Connection-specific DNS Suffix .:
        Description.....................: VNetEast
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



<!--HONumber=Jun16_HO2-->


