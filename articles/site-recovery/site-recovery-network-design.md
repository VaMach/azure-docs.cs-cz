---
title: "Navrhování infrastruktury sítě pro zotavení po havárii | Microsoft Docs"
description: "Tento článek popisuje aspekty návrhu sítě Azure Site Recovery"
services: site-recovery
documentationcenter: 
author: prateek9us
manager: jwhit
editor: 
ms.assetid: ce787731-d930-4f00-a309-e2fbc2e1f53b
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 07/20/2017
ms.author: pratshar
ms.openlocfilehash: 90ffd3dd1cf5068359afa1b60892cdee43ec0658
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="designing-your-network-for-disaster-recovery"></a>Navrhování sítě pro zotavení po havárii

Tento článek se přesměruje IT specialistů, kteří jsou zodpovědní za architektury, implementace a podpora kontinuity podnikových procesů a po havárii (BCDR) pro obnovení infrastruktury a kteří chtějí využívat Microsoft Azure Site obnovení (ASR) k podpoře a zdokonalování BCDR služeb. Tento článek popisuje praktické informace na strukturování sítě v lokalitě pro obnovení po havárii, je jej Azure nebo jiný místní lokality. 

## <a name="overview"></a>Přehled
[Azure Site Recovery (ASR)](https://azure.microsoft.com/services/site-recovery/) je služba Microsoft Azure, která orchestruje ochrany a obnovení virtualizovaných aplikací pro účely obnovení (BCDR) po havárii kontinuity podnikání. Tento dokument je určený pro Průvodce čtečce procesem navrhování sítě, zaměřené na architektury rozsahy IP adres a podsítí v lokalitě pro obnovení po havárii při replikaci virtuálních počítačů (VM) pomocí Site Recovery.

Kromě toho tento článek ukazuje, jak Site Recovery umožňuje architektury a implementace datacentru ve více lokalitách virtuální pro podporu služby BCDR v době testu nebo po havárii.

Ve světě, kde každý očekává 24 hodin denně 7 připojení je více než důležité někdy tak, aby vaši infrastrukturu a aplikace a spuštěná. Účelem kontinuity podnikových procesů a zotavení po havárii (BCDR) je obnovit selhání součásti, takže organizace mohou rychle obnovit normální činnost. Vývoj strategie obnovení po havárii pro řešení události pravděpodobně, zhoubné je velmi náročná. Toto je z důvodu je obtížné vyplývajících předpovídat budoucí, zvlášť, protože se týká nepravděpodobné události a zajistit odpovídající míry ochrany proti dalekosáhlou catastrophes vysoké náklady.

Zásadní pro plánování BCDR, cíl doba obnovení (RTO) a cíl bodu obnovení (RPO) musí být definovaný jako součást plánu zotavení po havárii. Při havárii narazilo zákazníka datového centra, pomocí Azure Site Recovery, mohou zákazníci rychle (nízkou RTO) převedení do online režimu replikované virtuální počítače umístěné v sekundárního datového centra nebo Microsoft Azure s minimální únikem (nízké RPO).

Převzetí služeb při selhání je možné díky automatické obnovení systému, který původně zkopíruje určené virtuální počítače z primární datové centrum do sekundárního datového centra nebo do Azure (v závislosti na scénáři) a pak se pravidelně aktualizuje repliky. Při plánování infrastruktury, by měly být považovány návrh sítě potenciální potíže, které by mohly bránit vám v schůzku společnosti RTO a plánovaný bod obnovení cíle.  

Když správci v úmyslu nasadit řešení zotavení po havárii, je jedna z otázek klíče dojmů, jak virtuální počítač bude dostupné po dokončení převzetí. Automatické obnovení systému umožňuje správci zvolit sítě, do kterého by virtuální počítač připojený k po převzetí služeb při selhání. Pokud primární lokalita je Azure nebo jde o místní web spravován serverem VMM, potom toho dosáhnete pomocí mapování sítě. Další informace o [mapování sítě v Azure do Azure DR](site-recovery-network-mapping-azure-to-azure.md) a [mapování sítě z nástroje VMM](site-recovery-network-mapping.md)


Při navrhování sítě pro obnovení lokality, správce má dvě možnosti:

* Použijte jiný rozsah IP adres pro síť v lokalitě pro obnovení. V tomto scénáři se virtuální počítač po převzetí služeb při selhání získat novou adresu IP a správce muset udělat aktualizace služby DNS. 
* Pomocí stejného rozsahu IP adres pro síť v lokalitě pro obnovení. V některých scénářích správci přednost zachovat IP adresy, které mají v primární lokalitě i po převzetí služeb při selhání. Ve scénáři normální správce musel aktualizace tras k označení nové umístění na IP adresy. Ale ve scénáři, kde je nasazená podsíť roztažené mezi primárním serverem a obnovení lokality, ponechá IP adresy pro virtuální počítače stane atraktivní možnosti. Roztažení podsíť z místní sítě pro síť Azure nebo mezi dvěma sítěmi Azure není možné.  

Když správci v úmyslu nasadit řešení zotavení po havárii, je mezi klíčové otázky jejich nezapomeňte, jak aplikace budou dostupné po dokončení převzetí. Moderní aplikace jsou téměř vždy v síti do určité míry, takže fyzickým přesunutím, že služby z jedné lokality do jiného představuje síťové výzvy závislé. Existují dva hlavní způsoby, které se tento problém řeší v řešení zotavení po havárii. Prvním přístupem je pevné IP adresy. Aplikace bez ohledu na přesun služeb a hostitelskými servery se v různých fyzických lokacích trvat konfiguraci IP adresy s nimi do nového umístění. Druhý přístup zahrnuje úplně změna IP adres při přechodu do obnovené lokalitě. Každý přístup má více změn implementace, které jsou shrnuté níž.

Při navrhování sítě pro obnovení lokality, správce má dvě možnosti:

## <a name="option-1-retain-ip-addresses"></a>Možnost 1: Zachovat IP adresy
Z hlediska proces obnovení po havárii pomocí pevné IP adresy se zdá být nejsnazší implementovat, ale má několik potenciální problémy, které v praxi alespoň oblíbených přístup. Azure Site Recovery poskytuje schopnost zachovat IP adresy ve všech scénářích. Předtím, než jeden rozhodne zachovat IP, má být poskytnut odpovídající myšlenku k omezení, které ukládá na možnostech převzetí služeb při selhání. Dejte nám se podívejte na faktorů, které vám můžou pomoct při provedení rozhodnutí zachování IP adresy, nebo ne. Toho lze dosáhnout dvěma způsoby pomocí roztažené podsíť nebo převzetím úplné podsítě.

### <a name="stretched-subnet"></a>Roztažené podsítě
Zde podsíť je k dispozici současně v primární a zotavení po Havárii umístění. Jednoduše řečeno, to znamená serveru a jeho konfigurace IP adresy (vrstvy 3) můžete přesunout do druhého serveru a sítě bude směrovat přenosy do nového umístění automaticky. Toto je jednoduchá řešení z hlediska serveru, ale má určité problémy:

* Z hlediska vrstvy 2 (Datová vrstva odkaz) bude vyžadovat síťová zařízení, která můžete spravovat roztažené sítě VLAN, ale to se staly menší problém, jako je nyní k dispozici. Druhý a obtížnější problém je, že pomocí roztažení potenciální domény selhání je rozšířeno k oběma sítím VLAN v podstatě stane jediným bodem selhání. To je nepravděpodobné situaci, došlo, všesměrového vysílání storm se spustil, ale nebylo možné izolované. Našli smíšený názory týkající se těchto potíží poslední jsme viděli mnoho implementací úspěšné, stejně jako "se nikdy implementaci tuto technologii zde".
* Roztažené podsíť není možné, pokud používáte Microsoft Azure jako web zotavení po Havárii.

### <a name="subnet-failover"></a>Podsíť převzetí služeb při selhání
Je možné implementovat podsíť převzetí služeb při selhání získat výhody řešení roztažené podsíť popsané výše bez roztažení podsíť ve více lokalitách. Zde jakékoli dané podsíti by nacházet v lokalitě 1 nebo 2 lokality, ale nikdy v obou lokalitách současně. Aby byla zachována v případě selhání adresní prostor IP adres, je možné prostřednictvím kódu programu uspořádejte infrastruktury směrovač přesunout podsítě z jedné lokality do jiného. V případě převzetí služeb při selhání, které by přesunout podsítí s přidruženého chráněné virtuální počítače. Hlavní nevýhodou tohoto přístupu je v případě selhání nutné přesunout celou podsíť, které mohou být OK ale může to mít vliv faktory členitosti převzetí služeb při selhání.

Podívejme se, jak je možné replikovat jejích virtuálních počítačů do umístění pro obnovení při selhání přes celou podsíť fiktivních enterprise s názvem Contoso. Podíváme se nejprve na tom, jak je moct spravovat podsítě při replikaci virtuálních počítačů mezi dvěma místními umístěními Contoso, a pak se budeme zabývat jak podsíť převzetí služeb při selhání funguje, když [Azure se používá jako lokality pro obnovení po havárii](#failover-to-azure).

#### <a name="failover-from-on-premises-to-azure"></a>Převzetí služeb při selhání z místního do Azure 
Azure Site Recovery (ASR) umožňuje Azure má být použit jako lokality pro obnovení po havárii pro virtuální počítače.  

Podívejme se na scénář, kde fiktivní společnost s názvem společnosti Woodgrove Bank má místní infrastrukturu hostování jejich řádku obchodních aplikací a jejich hostují své mobilní aplikace v Azure. Připojení mezi virtuálními počítači Woodgrove Bank v Azure a místními servery poskytuje site-to-site (S2S) virtuální privátní sítě (VPN) nebo ExpressRoute. S2S VPN umožňuje společnosti Woodgrove Bank virtuální sítě v Azure a zobrazit jako rozšíření místní sítě organizace Woodgrove Bank. Tato komunikace se aktivuje S2S VPN mezi hraniční společnosti Woodgrove Bank a virtuální síť Azure. Nyní Woodgrove chce použít automatické obnovení systému k replikaci jeho úlohy běžící primární oblasti Azure jiné oblasti Azure. Tato možnost splňuje potřeby Woodgrove, která chce ekonomické možnost zotavení po Havárii a je možné při ukládání dat do veřejných cloudových prostředích. Woodgrove má jak nakládat s aplikací a konfigurace, které závisí na pevně IP adresy, proto mají potřeba zachovat IP adresy pro své aplikace po převzetí služeb při selhání do jiné oblasti v Azure.

Woodgrove rozhodla k přidělování IP adres z rozsahu IP adres (172.16.1.0/24, 172.16.2.0/24) její prostředky, které běží v Azure.

Pro Woodgrove moct replikovat jejích virtuálních počítačů do Azure a zachovat stávající IP adresy musí se vytvořit virtuální síť Azure. Je nutné rozšíření místní sítě tak, aby aplikace můžete bezproblémově převzetí služeb při selhání z místní lokality do Azure. Azure umožňuje přidat připojení site-to-site, jakož i point-to-site VPN k virtuálním sítím vytvořeným v Azure. Při nastavování připojení site-to-site, síť Azure umožňuje směrovat provoz do místního umístění (Azure místní sítě nazve je) pouze v případě, že se liší od místní rozsah IP adres, rozsah IP adres, protože Azure nepodporuje roztažení podsítě.  To znamená, že pokud máte 192.168.1.0/24 podsítě v místě, nemůžete přidat 192.168.1.0/24 místní sítě v síti Azure. Toto je očekávané, protože Azure není známo, že neexistují žádné aktivní virtuální počítače v podsíti a zda podsíť se vytváří jenom pro účely zotavení po Havárii. Abyste mohli správně směrovat síťový provoz mimo síť Azure podsítě v síti a v místní síti nesmí být v konfliktu.

![Před převzetí služeb při selhání podsíť](./media/site-recovery-network-design/network-design7.png)

Před převzetí služeb při selhání

Chcete-li Woodgrove splnění jejich obchodní požadavky, musíme implementovat následující pracovních postupů:

* Vytvořit další síť, dejte nám volání síti pro obnovení, kde by se vytvořily virtuální počítače při selhání.
* Aby se zajistilo, že IP adresu pro virtuální počítač se uchovávají po selhání, přejděte na kartu Konfigurace v části Vlastnosti virtuálního počítače, zadejte stejnou IP Adresou, že virtuální počítač má místní a pak klikněte na Uložit. Pokud je virtuální počítač převzal, přiřadí Azure Site Recovery zadaná IP k virtuálnímu počítači.

![Vlastnosti sítě](./media/site-recovery-network-design/network-design8.png)

Jakmile se spustí převzetí služeb při selhání a virtuální počítače jsou vytvořeny v síti pro obnovení s požadovanou IP Adresou, připojení k této síti může být vytvořena pomocí [virtuální síť připojení mezi virtuálními](../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md). V případě potřeby, tato akce může provádět skriptování.  Jak jsme popsané v předchozí části o převzetí podsíť i v případě převzetí služeb při selhání do Azure, museli byste trasy odpovídajícím způsobem upravit tak, aby odpovídaly že této 192.168.1.0/24 je nyní přesunuta do Azure.

![Po převzetí služeb při selhání podsíť](./media/site-recovery-network-design/network-design9.png)

Po převzetí služeb při selhání

Pokud nemáte Azure Network, jak je znázorněno na obrázku výše. Můžete vytvořit připojení s2s vpn mezi 'Primární lokality' a 'Síti pro obnovení' po převzetí služeb při selhání.  


#### <a name="failover-to-a-secondary-on-premises-site"></a>Převzetí služeb při selhání na sekundární místní lokalitu
Dejte nám podívejte se na scénář kde chceme zachovat IP jednotlivých virtuálních počítačů a převzetí služeb při selhání-dokončení podsíť společně. Aplikace běžící v podsíti 192.168.1.0/24 má primární lokalita. Když se stane převzetí služeb při selhání, všechny virtuální počítače, které jsou součástí této podsíti převezme služby při selhání na lokalitě pro obnovení a zachovat jejich IP adresy. Trasy bude muset odpovídajícím způsobem upravit tak, aby odrážela skutečnost, že všechny virtuální počítače, které patří do podsítě 192.168.1.0/24 mají nyní přesunuta do lokalitě pro obnovení.

Na následujícím obrázku trasy mezi primární lokalitou a obnovení lokality, třetí lokality a primární lokality a třetí lokality a lokality obnovení bude muset odpovídajícím způsobem upravit.

Následující obrázky ukazuje podsítě před převzetí služeb při selhání. 192.168.0.1/24 podsíť je aktivní v primární lokalitě před převzetí služeb při selhání a stane aktivní obnovení lokality po převzetí služeb při selhání

![Před převzetí služeb při selhání](./media/site-recovery-network-design/network-design2.png)

Před převzetí služeb při selhání

Následující obrázek ukazuje sítě a podsítě po převzetí služeb při selhání.

![Po převzetí služeb při selhání](./media/site-recovery-network-design/network-design3.png)

Po převzetí služeb při selhání

V sekundární lokalitě je na místě a pomocí serveru VMM k její správě pak při povolování ochrany pro konkrétní virtuální počítač, síťové prostředky podle následující pracovní postup bude přidělíte automatické obnovení systému:

* Automatické obnovení systému přidělí IP adresu pro každé síťové rozhraní na virtuálním počítači z fondu statických adres IP definované v příslušné síti pro každou instanci System Center VMM.
* Pokud správce definuje stejný fond IP adres pro síť v lokalitě pro obnovení, který fondu IP adres sítě v primární lokalitě, při přidělování IP adresu, kterou virtuální počítač repliky automatické obnovení systému byste přidělit stejnou IP adresu jako primární virtuální počítač.  IP adresa je vyhrazena v nástroji VMM, ale není nastavena jako IP adresa převzetí služeb při selhání na hostiteli technologie Hyper-v. Těsně před převzetí služeb při selhání je nastavit IP adresu převzetí služeb při selhání na hostiteli technologie Hyper-v.


Pokud stejnou IP Adresou není k dispozici, automatické obnovení systému byste přidělit některé další dostupnou IP adresu z definovaných fond IP adres.

Po povolení ochrany virtuálního počítače můžete využít následující ukázkový skript ověření IP adresa, která byla přidělena k virtuálnímu počítači. Stejnou IP Adresou by nastavit jako IP převzetí služeb při selhání a přiřazené k virtuálnímu počítači v době převzetí služeb při selhání:

        $vm = Get-SCVirtualMachine -Name <VM_NAME>
        $na = $vm[0].VirtualNetworkAdapters>
        $ip = Get-SCIPAddress -GrantToObjectID $na[0].id
        $ip.address  

> [!NOTE]
> Ve scénáři, kde virtuální počítače používat službu DHCP je Správa IP adres zcela mimo kontrolu automatické obnovení systému. Správce má zajistit, že server DHCP, který obsluhuje IP adresy v lokalitě pro obnovení může sloužit ze stejného rozsahu jako u primární lokality.
>
>



## <a name="option-2-changing-ip-addresses"></a>Možnost 2: Při změně IP adres
Tento přístup vám zdá, že současných převažujících podle co jsme viděli. Používá formát změny IP adresu každý virtuální počítač, který je součástí převzetí služeb při selhání. Z nevýhod tohoto přístupu vyžaduje příchozí síťové ' Další ', že aplikace, který byl v IPx je nyní v IPy. I když IPx a IPy jsou logické názvy, záznamy DNS, které obvykle mají změnit nebo vyprázdněných v celé síti a položky v mezipaměti v tabulkách sítě muset aktualizovat ani vyprázdněny, proto s prodlevou může se zobrazit v závislosti na tom, jak infrastruktura DNS byl nastaven.. Tyto problémy lze zmírnit použitím nízké hodnoty TTL v případě aplikace v síti intranet a použitím [Manager provoz Azure s automatické obnovení systému](http://azure.microsoft.com/blog/2015/03/03/reduce-rto-by-using-azure-traffic-manager-with-azure-site-recovery/) pro internet aplikace založené na

### <a name="changing-the-ip-addresses---illustration"></a>Změna IP adresy - obrázku
Dejte nám podívejte se na tento scénář kde máte v úmyslu použít jinou IP adres mezi primárním serverem a obnovení lokality. V následujícím příkladu jsme také mít třetí lokality z kde aplikace hostované na primární server nebo obnovení webu můžete získat přístup.

![Různé IP - před převzetí služeb při selhání](./media/site-recovery-network-design/network-design10.png)


Na obrázku výše jsou některé aplikace hostované v podsíti 192.168.1.0/24 podsítí v primární lokalitě a byly nakonfigurovány spolu v lokalitě pro obnovení v podsíti 172.16.1.0/24 po převzetí služeb při selhání. Trasy připojení nebo síť VPN byla správně nakonfigurována, aby všechny tři servery navzájem přístup.

Jako obrázek níže znázorňuje při přechodu jednoho nebo více aplikací bude možné obnovit v podsíti obnovení. V takovém případě jsme nejsou Vynucené převzetí služeb při selhání celou podsíť ve stejnou dobu. Změna konfigurace sítě VPN nebo sítě tras se nevyžadují žádné změny. Převzetí služeb při selhání a některé aktualizace DNS se ujistěte se, aby aplikace zůstaly dostupné. Pokud chcete povolit dynamické aktualizace DNS nakonfigurovaný by zaregistrovat virtuální počítače sami pomocí nových IP po spuštění po převzetí služeb při selhání.

![Různé IP - po převzetí služeb při selhání](./media/site-recovery-network-design/network-design11.png)


Virtuální počítač repliky po převzetí služeb při selhání mít IP adresu, která není stejná jako IP adresa primárního virtuálního počítače. Virtuální počítače aktualizuje server DNS, který používají po spuštění. Záznamy DNS, které obvykle mají změnit nebo vyprázdněných v celé síti, a v mezipaměti položky v tabulkách sítě mají aktualizován nebo vyprázdněny, takže není čelí výpadku, zatímco provést tyto změny stavu. Tento problém můžete zmírnit:

* Použití nízké hodnoty TTL pro aplikace v síti intranet.
* Pomocí Azure provoz Manager automatické obnovení systému pro internet na základě aplikací.
* Pomocí následujícího skriptu v rámci plánu obnovení a aktualizovat Server DNS zajistit včasné aktualizace (skript Pokud není potřeba registraci dynamického DNS je nakonfigurován)

        param(
        string]$Zone,
        [string]$name,
        [string]$IP
        )
        $Record = Get-DnsServerResourceRecord -ZoneName $zone -Name $name
        $newrecord = $record.clone()
        $newrecord.RecordData[0].IPv4Address  =  $IP
        Set-DnsServerResourceRecord -zonename $zone -OldInputObject $record -NewInputObject $Newrecord

### <a name="changing-the-ip-addresses--dr-to-azure"></a>Změna IP adresy – zotavení po Havárii do Azure
[Nastavení infrastruktury sítě pro Microsoft Azure jako web pro zotavení po havárii](http://azure.microsoft.com/blog/2014/09/04/networking-infrastructure-setup-for-microsoft-azure-as-a-disaster-recovery-site/) příspěvku na blogu vysvětluje, jak při zachování IP adresy není požadavkem instalaci požadované síťové infrastruktury Azure. Se spustí s popisující aplikace a podívejte se na postup nastavení sítě na místě a na Azure a pak končí s jak to provést testovací převzetí služeb a plánované převzetí služeb při selhání.

## <a name="next-steps"></a>Další kroky
[Další informace](site-recovery-vmm-to-vmm.md#prepare-for-network-mapping) jak Site Recovery mapuje zdrojové a cílové sítě, pokud se používá VMM server pro správu primární lokality.
