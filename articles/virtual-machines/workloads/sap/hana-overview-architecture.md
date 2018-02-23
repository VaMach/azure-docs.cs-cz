---
title: "Přehled a architektura SAP HANA v Azure (velké instance) | Microsoft Docs"
description: "Přehled architektury nasazení SAP HANA v Azure (velké instance)."
services: virtual-machines-linux
documentationcenter: 
author: RicksterCDN
manager: timlt
editor: 
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 01/02/2018
ms.author: rclaus
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 4246ecfa50176400c54cd80857e25675290e7170
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/21/2018
---
# <a name="sap-hana-large-instances-overview-and-architecture-on-azure"></a>Přehled SAP HANA (velké instance) a architektura v Azure

## <a name="what-is-sap-hana-on-azure-large-instances"></a>Co je SAP HANA v Azure (velké instance)?

SAP HANA v Azure (velké Instance) je jedinečné řešení do Azure. Kromě zajištění virtuálních počítačů Azure pro účely nasazení a spuštění SAP HANA, Azure nabízí možnost spuštění a nasazení SAP HANA na úplné obnovení serverů, které jsou vyhrazené pro vás jako zákazník. SAP HANA v Azure (velké instance) řešení je založena na nesdílené počítačem nebo serverem holého hardwaru, který je přiřazen vám jako zákazník. Hardware serveru je součástí větší razítek, které obsahují výpočty a serveru, sítě a infrastruktury úložiště. Který, jako kombinace je HANA TDI certifikaci. Služba nabídku SAP HANA v Azure (velké instance) nabízí různé identifikátory SKU jiný server nebo velikosti od jednotky, které mají 72 procesorů a paměti 768 GB pro jednotky, které mají 960 procesory a 20 TB paměti.

Izolace zákazníka v rámci infrastruktury razítko se provádí v klientů, které podrobně vypadá takto:

- Sítě: Izolace zákazníků v rámci infrastruktury zásobníku prostřednictvím virtuální sítě na zákazníka přiřadit klienta. Klient je přiřazen jednoho zákazníka. Zákazník může mít několik klientů. Izolace sítě klientů, které zakazuje síťovou komunikaci mezi klienty v úrovni razítka infrastruktury. I když klienti patřit do stejné zákazníka.
- Součástí úložišť: izolaci prostřednictvím úložiště virtuálních počítačů, které mají přiřazené svazky úložiště. Svazky úložiště lze přiřadit pouze jeden úložiště virtuálnímu počítači. Úložiště virtuálního počítače je přiřazen výhradně jeden jednoho klienta v zásobníku certifikovaných infrastruktury SAP HANA TDI. V důsledku je přístupná v jedné konkrétní a související klientovi pouze svazky úložiště, které jsou přiřazeny k virtuálnímu počítači úložiště. A nejsou viditelné mezi různými klienty nasazené.
- Server nebo hostitele: jednotka serveru nebo hostitele není sdílena mezi zákazníky nebo klientů. Server nebo hostitele nasazený na zákazníka, je atomic holých výpočetní jednotka, přiřazený jeden jednoho klienta. **Ne** se používá vytváření oddílů hardwarové nebo softwarové dělení vést ke, jako zákazník, sdílení hostitel nebo server s jinou zákazníka. Úložné svazky, které jsou přiřazeny k virtuálnímu počítači úložiště konkrétní klienta jsou připojené k takové serveru. Klient může mít jeden do mnoha serveru jednotek různé identifikátory SKU výhradně přiřazen.
- V rámci SAP HANA s časovým razítkem infrastrukturu Azure (velké Instance) mnoha různými klienty jsou nasazené a izolované proti sobě prostřednictvím klienta koncepty na sítě, úložiště a výpočetní úroveň. 


Tyto jednotky úplné obnovení serveru jsou podporovány pouze spustit SAP HANA. Vrstvy aplikace SAP nebo zatížení střední VMware běží ve virtuálních počítačích Microsoft Azure. Razítka infrastruktury SAP HANA systémem Azure (velké Instance) jednotky jsou připojené k Azure Network páteřním, tak, které je k dispozici s nízkou latencí připojení mezi SAP HANA na jednotkách Azure (velké Instance) a virtuální počítače Azure.

Tento dokument je jednou z více dokumentů, které zahrnují SAP HANA v Azure (velké Instance). V tomto dokumentu jsme přejděte prostřednictvím základní architekturu a odpovědnosti, služeb a hlavní prostřednictvím funkce řešení. Pro většinu v oblastech, jako je vytváření sítí a připojení čtyři dokumenty nepřekrývají podrobnosti a přejít k podrobnostem seznamy. V dokumentaci SAP HANA v Azure (velké Instance) nezahrnuje aspekty SAP NetWeaver instalaci nebo nasazení SAP NetWeaver ve virtuálních počítačích Azure. SAP NetWeaver v Azure je popsaná v samostatné dokumenty najít ve stejném kontejneru dokumentace k Azure. 


Různé dokumenty velké Instance HANA materiálů zahrnovat tyto oblasti:

- [Přehled SAP HANA (velké Instance) a architektura v Azure](hana-overview-architecture.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
- [Infrastruktura SAP HANA (velké instance) a připojení v Azure](hana-overview-infrastructure-connectivity.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
- [Postup instalace a konfigurace SAP HANA (velké instance) na Azure](hana-installation.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
- [SAP HANA (velké instance) vysokou dostupnost a zotavení po havárii v Azure](hana-overview-high-availability-disaster-recovery.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
- [Poradce při potížích s SAP HANA (velké instance) a sledování v Azure](troubleshooting-monitoring.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
- [Nastavení v SUSE pomocí STONITH vysokou dostupnost](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/ha-setup-with-stonith)
- [Operační systém zálohování a obnovení pro typ II SKU](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/os-backup-type-ii-skus)

## <a name="definitions"></a>Definice

Několik společné definice se často používá v architektuře a technické příručce nasazení. Všimněte si následujících podmínek a jejich významů:

- **IaaS:** infrastruktury jako služby
- **PaaS:** platforma jako služba
- **SaaS:** Software jako služba
- **Součást SAP:** jednotlivých aplikací SAP, jako je například ECC, BW, správce řešení nebo v podnikovém portálu. SAP součástí může být založen na tradičních technologií ABAP nebo Java nebo jiných NetWeaver na základě aplikaci, například obchodních objektů.
- **Prostředí SAP:** jeden nebo více součástí SAP logicky seskupeny provést obchodní funkci, jako je třeba vývoje, QAS, školení, zotavení po Havárii nebo výroby.
- **SAP na šířku:** odkazuje na celý SAP prostředky ve vaší IT na šířku. Na šířku SAP zahrnuje všechny produkční a mimo provozní prostředí.
- **Systém SAP:** kombinace databázového systému a vrstva aplikace SAP ERP vývojový systém, SAP BW testovací systém, produkční systému SAP CRM, atd. Azure nasazení nepodporují dělení tyto dvě vrstvy mezi místními a Azure. Znamená systému SAP je buď nasadit místně, nebo je nasazené v Azure. Můžete však nasadit různých systémech šířku SAP do Azure nebo místní. Například můžete nasadit systémy SAP CRM vývoj a testování v Azure, při nasazení SAP CRM produkční systému místní. Pro SAP HANA v Azure (velké instance) je určena hostujete aplikační vrstvu SAP SAP systémů ve virtuálních počítačích Azure a související instance SAP HANA na jednotce v razítku HANA velké Instance.
- **Velké Instance razítka:** zásobníku infrastruktury hardwaru, který je SAP HANA TDI certifikaci a vyhrazené pro spuštění instance SAP HANA v rámci Azure.
- **SAP HANA v Azure (velké instance):** oficiální název nabídky v Azure ke spuštění HANA instancí v na SAP HANA TDI certifikaci hardwaru, která je nasazena v velké Instance razítka v různých oblastech Azure. Související termín **HANA velké Instance** je zkratka pro SAP HANA v Azure (velké instance) a je široce používá tato příručka technického nasazení.
- **Mezi různými místy:** popisuje scénář, kde jsou nasazené virtuální počítače k předplatnému Azure, který má site-to-site, více lokalit nebo připojení ExpressRoute mezi místní datových centrech a Azure. Dokumentace k společné Azure, tyto typy nasazení jsou také popsány jako mezi různými místy scénáře. Z důvodu pro připojení je rozšířit místní domény, OpenLDAP Active Directory v místě a místní DNS do Azure. Na šířku místní je rozšířeno na Azure majetek předplatná Azure. S touto příponou, virtuálních počítačů může být součástí místní domény. Uživatelé domény místní domény, můžete přístup k serverům a službu lze spouštět na těchto virtuálních počítačů (např. služby databázového systému). Komunikace a název rozlišení mezi virtuální počítače nasazené na místě a nasazené virtuální počítače Azure je možné. Například je typický scénář, ve které většina SAP jsou nasazené prostředky. V tématu příručky z [plánování a návrhu pro bránu VPN](../../../vpn-gateway/vpn-gateway-plan-design.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) a [vytvoření virtuální sítě s připojením Site-to-Site pomocí webu Azure portal](../../../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) podrobnější informace.
- **Klienta:** zákazníka nasazené v HANA velké instance razítka získá izolované do "klienta." Klient je izolované sítě, úložiště a výpočetní vrstvy od ostatních klientů. Proto že úložiště a výpočetní jednotky přiřazené různými klienty nelze uvidí mezi sebou nebo vzájemně komunikovat na úrovni HANA velké Instance razítka. Zákazník může rozhodnout pro nasazení do jiných klientů. Dokonce i pak není žádná komunikace mezi klienty na úrovni HANA velké Instance razítka.
- **Skladová položka kategorie:** pro velké instance HANA, nabízí tyto dvě kategorie SKU.
    - **Třída Type:** S72, S72m, S144, S144m, S192 a S192m
    - **Typ třídy II:** S384, S384m, S384xm, S576, S768 a S960


Existují různé další prostředky, které byly publikovány na nasazení SAP zatížení na veřejném cloudu Microsoft Azure. Důrazně doporučujeme, aby každý, kdo plánování a provádění nasazení SAP HANA v Azure je zkušeného a s ohledem na objekty zabezpečení Azure IaaS a nasazení SAP zatížení v Azure IaaS. Následující zdroje obsahují další informace a by měla odkazovat než budete pokračovat:


- [Použití SAP řešení na virtuálních počítačích Microsoft Azure](get-started.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="certification"></a>Certifikace

Kromě toho na certifikaci NetWeaver SAP vyžaduje speciální certifikační pro SAP HANA pro podporu SAP HANA na určité infrastruktuře, jako je například Azure IaaS.

Jádro Poznámka SAP na NetWeaver a pro SAP HANA stupeň certifikační [SAP Poznámka #1928533 – SAP aplikace v Azure: typy podporovaných produktů a virtuální počítač Azure](https://launchpad.support.sap.com/#/notes/1928533).

To [SAP Poznámka #2316233 - SAP HANA v Microsoft Azure (velké instance)](https://launchpad.support.sap.com/#/notes/2316233/E) je také důležité. Vysvětluje řešení popsané v tomto průvodci. Kromě toho jsou podporovány ke spuštění SAP HANA v typu GS5 virtuálních počítačů Azure. [Informace o tomto případě je publikována na webu SAP](http://global.sap.com/community/ebook/2014-09-02-hana-hardware/enEN/iaas.html).

SAP HANA na řešení Azure (velké instance) uvedené v &#2316233; Poznámka SAP poskytuje zákazníkům společnosti Microsoft a SAP schopnost nasadit velké SAP Business Suite SAP Business Warehouse (BW), S/4 HANA BW/4HANA nebo jiných SAP HANA úlohy v Azure. Řešení je založena na značce SAP HANA certifikovaných vyhrazený hardware ([SAP HANA přizpůsobit Datacenter integrace – TDI](https://scn.sap.com/docs/DOC-63140)). Spuštěna jako SAP HANA TDI nakonfigurované řešení vám poskytne důvěry vědět, že všechny aplikace na základě SAP HANA (včetně SAP Business Suite na SAP HANA, SAP Business Warehouse (BW) na SAP HANA, S4/HANA a BW4/HANA) budou pracovat na hardware infrastruktury.

Porovnání se spouštěním SAP HANA ve virtuálních počítačích Azure se toto řešení výhody – poskytuje pro mnohem větší objemy paměti. Pokud chcete povolit toto řešení, existují některé potřeba pochopit klíčové aspekty:

- Aplikace vrstvu a bez SAP aplikace SAP spustit v Azure virtuální počítače (VM hostovaných v razítka obvykle Azure hardwaru).
- Místní infrastruktury zákazníka, datových center a nasazení aplikací jsou připojené k Cloudová platforma Microsoft Azure přes Azure ExpressRoute (doporučeno) nebo virtuální privátní sítě (VPN). Active Directory (AD) a DNS jsou také rozšířit do Azure.
- Instance databáze SAP HANA pro pracovní vytížení HANA běží na SAP HANA v Azure (velké instance). Razítko velké Instance je připojený do sítě Azure, takže software na virtuálních počítačích Azure mohou komunikovat s běhu ve velkých instancích HANA instance HANA.
- Hardware SAP HANA v Azure (velké instance) je vyhrazený hardware, které jsou součástí infrastruktury jako služby (IaaS) s SUSE Linux Enterprise Server nebo Red Hat Enterprise Linux předinstalován. Jako pomocí Azure Virtual Machines další aktualizace a údržba pro operační systém je vaší povinností.
- Instalaci HANA nebo žádné další součásti, které jsou potřebné ke spuštění SAP HANA na jednotkách HANA velké instancí je vaší povinností, stejně jako všechny příslušné probíhající operace a správy SAP HANA v Azure.
- Kromě pomocí řešení popisovaných v tomto poli můžete nainstalovat další součásti ve vašem předplatném Azure, která se připojuje k SAP HANA v Azure (velké instance).  Například součásti, které umožňují komunikaci s nebo přímo k databázi SAP HANA (jump servery, servery s RDP, SAP HANA Studio SAP Data Services pro SAP BI scénáře, nebo řešení monitorování sítě).
- Jako v Azure nabízí velké instancí HANA podpůrné funkce vysoké dostupnosti a zotavení po havárii.

## <a name="architecture"></a>Architektura

Na hlavní SAP HANA na řešení Azure (velké instance) má aplikační vrstvu SAP, které se nacházejí ve virtuálních počítačích Azure a vrstva databáze, které se nacházejí na hardwaru SAP TDI nakonfigurované v razítku velké Instance ve stejné oblasti Azure, která je připojena k Azure IaaS.

> [!NOTE]
> Je třeba nasadit aplikační vrstvu SAP ve stejné oblasti Azure jako vrstva databázového systému SAP. Toto pravidlo je dobře zdokumentovaný v publikované informace o SAP zatížení v Azure. 

Přehled architektury SAP HANA v Azure (velké instance) poskytuje že SAP TDI certifikované konfigurace hardwaru (nevirtuálním, úplného operačního systému, vysoce výkonné server pro databázi SAP HANA) a možnost a flexibilitu Azure k prostředkům škálování pro aplikační vrstvy SAP podle svých potřeb.

![Přehled architektury systému SAP HANA v Azure (velké instance)](./media/hana-overview-architecture/image1-architecture.png)

Architektura vidět je rozdělené do tří částí:

- **Práva:** místní infrastruktury s různými aplikacemi v datových centrech koncovým uživatelům přístup k obchodních aplikací (například SAP). V ideálním případě by to místní infrastruktury je připojen do Azure s Azure [ExpressRoute](https://azure.microsoft.com/services/expressroute/).

- **Centrum:** ukazuje Azure IaaS a v takovém případě použijte virtuálních počítačů Azure k hostování SAP nebo jiné aplikace, které používají SAP HANA jako systém databázového systému. Menší HANA instancí, které poskytují funkce s pamětí virtuálních počítačích Azure nasazených ve virtuálních počítačích Azure spolu s jejich aplikační vrstvu. Další informace o [virtuální počítače](https://azure.microsoft.com/services/virtual-machines/).
<br />Síť Azure slouží k seskupení systémy SAP společně s jiných aplikací do virtuální sítě Azure (virtuální sítě). Tyto virtuální sítě připojit k místním systémům i, SAP HANA v Azure (velké instance).
<br />SAP NetWeaver aplikace a databáze, které podporují spouštění v Microsoft Azure najdete v tématu [SAP podporu Poznámka #1928533 – SAP aplikace v Azure: typy podporovaných produktů a virtuální počítač Azure](https://launchpad.support.sap.com/#/notes/1928533). Dokumentaci k nasazení SAP řešení v Azure zkontrolujte:

  -  [Pomocí SAP na virtuálních počítačích s Windows (VM)](../../virtual-machines-windows-sap-get-started.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
  -  [Použití SAP řešení na virtuálních počítačích Microsoft Azure](get-started.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

- **Vlevo:** ukazuje TDI SAP HANA Interface certifikaci hardwaru v razítku velké instanci Azure. Instance HANA velké jednotky jsou připojené k virtuálním sítím Azure svého předplatného pomocí stejnou technologii jako připojení z místního do Azure.

Razítko velké instanci Azure samotné kombinuje následující součásti:

- **Výpočty:** servery, které jsou založeny na procesory Intel Xeon E7-8890v3 nebo Intel Xeon E7-8890v4, s nezbytné výpočetní schopnosti, které jsou certifikované SAP HANA.
- **Síť:** A unified vysokorychlostní síťové fabric, která propojení výpočty, úložiště a LAN součásti.
- **Úložiště:** infrastruktury úložiště, které je přístupné přes jednotné síťových prostředcích infrastruktury. Kapacita konkrétní úložiště je k dispozici v závislosti na konkrétní SAP HANA v konfiguraci Azure (velké instance) nasazuje (větší kapacitu úložiště je k dispozici v dalších měsíčních nákladů).

V infrastruktuře víceklientské velké Instance razítka jsou zákazníci nasadit jako izolované klienty. Při nasazení klienta je třeba název předplatné Azure v rámci Azure registraci. Toto má být předplatné Azure, velké instance HANA bude účtovat proti. Tyto klienty mít vztah 1:1 k předplatnému Azure. Sítě vhodné, že je možné pro přístup k HANA velké Instance jednotku nasazené v jednoho klienta v jedné oblasti Azure z jiné sítě Azure Vnet, která patří do různých předplatných Azure. I když tyto předplatná Azure musí patřit do stejné Azure registrace. 

Stejně jako u virtuálních počítačích Azure, SAP HANA v Azure (velké instance) je k dispozici v několika oblastmi Azure. Chcete-li nabízí možnosti zotavení po havárii, můžete vyjádřit výslovný souhlas. Různé velké Instance razítka v rámci jedné politickým geografické oblasti jsou připojené k sobě navzájem. Například HANA velké Instance razítka v oblasti USA – západ a oblasti USA – východ jsou připojené prostřednictvím propojení s vyhrazenou síť za účelem zotavení po Havárii replikace. 

Stejně jako, můžete si vybrat mezi různé typy virtuálních počítačů s virtuálními počítači Azure, můžete z různých SKU z HANA velké instancí, které jsou přizpůsobené pro různé zatížení typy SAP HANA. SAP se týká paměti poměr soketu procesoru u různých úloh podle generace procesor Intel. Následující tabulka uvádí typy SKU nabízí.

Od července 2017 je k dispozici v několika konfigurací ve Azure oblastech z oblasti USA – západ i USA – východ, Austrálie – východ, Austrálie – jihovýchod, západní Evropa a Severní Evropa SAP HANA v Azure (velké instance):

| Řešení SAP | Procesor | Memory (Paměť) | Úložiště | Dostupnost |
| --- | --- | --- | --- | --- |
| Optimalizovaná pro OLAP: SAP BW BW/4HANA<br /> nebo SAP HANA pro obecné zatížení OLAP | Na Azure S72 SAP HANA<br /> – 2 procesor Intel Xeon® x E7 8890 v3<br /> 36 jader procesoru a 72 procesoru vláken |  768 GB |  3 TB | Dostupné |
| --- | Na Azure S144 SAP HANA<br /> – Procesor Intel Xeon® x 4 E7 8890 v3<br /> 72 jader procesoru a 144 procesoru vláken |  1,5 TB |  6 TB | Nenabízí se už |
| --- | Na Azure S192 SAP HANA<br /> – Procesor Intel Xeon® x 4 E7 8890 v4<br /> 96 jader procesoru a 192 procesoru vláken |  2.0 TB |  8 TB | Dostupné |
| --- | Na Azure S384 SAP HANA<br /> – 8 procesor Intel Xeon® x E7 8890 v4<br /> 192 jader procesoru a 384 procesoru vláken |  4.0 TB |  16 TB | Dostupné |
| Optimalizovaná pro OLTP: SAP Business Suite<br /> na SAP HANA nebo S nebo 4HANA (OLTP)<br /> Obecné OLTP | Na Azure S72m SAP HANA<br /> – 2 procesor Intel Xeon® x E7 8890 v3<br /> 36 jader procesoru a 72 procesoru vláken |  1,5 TB |  6 TB | Dostupné |
|---| Na Azure S144m SAP HANA<br /> – Procesor Intel Xeon® x 4 E7 8890 v3<br /> 72 jader procesoru a 144 procesoru vláken |  3.0 TB |  12 TB | Nenabízí se už |
|---| Na Azure S192m SAP HANA<br /> – Procesor Intel Xeon® x 4 E7 8890 v4<br /> 96 jader procesoru a 192 procesoru vláken  |  4.0 TB |  16 TB | Dostupné |
|---| Na Azure S384m SAP HANA<br /> – 8 procesor Intel Xeon® x E7 8890 v4<br /> 192 jader procesoru a 384 procesoru vláken |  6.0 TB |  18 TB | Dostupné |
|---| Na Azure S384xm SAP HANA<br /> – 8 procesor Intel Xeon® x E7 8890 v4<br /> 192 jader procesoru a 384 procesoru vláken |  8.0 TB |  22 TB |  Dostupné |
|---| Na Azure S576 SAP HANA<br /> – Procesor Intel Xeon® x 12 E7 8890 v4<br /> 288 jader procesoru a 576 procesoru vláken |  12.0 TB |  28 TB | Dostupné |
|---| Na Azure S768 SAP HANA<br /> – Procesor Intel Xeon® x 16 E7 8890 v4<br /> 384 jader procesoru a 768 procesoru vláken |  16.0 TB |  36 TB | Dostupné |
|---| Na Azure S960 SAP HANA<br /> – 20 procesor Intel Xeon® x E7 8890 v4<br /> 480 jader procesoru a 960 procesoru vláken |  20.0 TB |  46 TB | Dostupné |

- Jader procesoru = součet jiný-technologie hyper-threaded jader procesoru součtu procesorů jednotky serverů.
- Vláken procesoru = součet výpočetní vláken poskytované technologie hyper-threaded jader procesoru součtu procesorů jednotky serverů. Všechny jednotky jsou nakonfigurované ve výchozím nastavení použít technologie Hyper-Threading.


Konkrétní konfigurace vybrali jsou závislé na zatížení, prostředky procesoru a požadovanou paměť. Je možné pro OLTP pracovního vytížení k používání SKU, které jsou optimalizované pro úlohy OLAP. 

Základ hardwaru pro všechny nabídky jsou SAP HANA TDI certifikaci. Ale jsme rozlišit mezi dvěma různými třídami hardware, který rozděluje SKU do:

- S72, S72m, S144, S144m, S192 a S192m, které označujeme jako typu Type I třídy, z jednotky SKU.
- S384, S384m, S384xm, S576, S768 a S960, které označujeme jako typ II třída z jednotky SKU.

Je důležité si uvědomit, že dokončení HANA velké Instance razítka není přidělen výhradně pro jednoho zákazníka & č. 39; použijte s. Tuto skutečnost se vztahuje na stojany s výpočetní a úložnou kapacitu připojené prostřednictvím síťových prostředcích infrastruktury také nasazené v Azure. Velké instancí HANA infrastruktury, jako je například Azure, nasadí různých zákazníků &quot;klienty&quot; , které jsou izolované od sebe navzájem v následující tři úrovně:

- Sítě: Izolace pomocí virtuální sítě v rámci HANA velké Instance razítka.
- Úložiště: Izolaci prostřednictvím úložiště virtuálních počítačů, které mají svazky úložiště přiřadit a izolovat svazky úložiště mezi klienty.
- Výpočetní: Vyhrazené přiřazení serveru jednotek pro jednoho klienta. Žádné pevné nebo softwarové oddíly jednotky serverů. Bez sdílení jedné jednotky serveru nebo hostitele mezi klienty. 

Nasazení velkých instancí HANA jednotky mezi různými klienty jako takový, nejsou viditelné navzájem. Ani velké jednotky Instance HANA nasazené v jiných klientů může komunikovat navzájem přímo na úrovni HANA velké Instance razítka. Pouze HANA velké Instance jednotky v rámci jednoho klienta může komunikovat navzájem na úrovni HANA velké Instance razítka.
Nasazené klienta v razítko velké instanci se přiřadí fakturace vhodné jedno předplatné. Sítě však vhodné, že je přístupná ze sítě Azure Vnet z jiných předplatných Azure v rámci stejné Azure registrace. Pokud nasadíte pomocí jiného předplatného Azure ve stejné oblasti Azure, také můžete požádat o klienta oddělené HANA velké Instance.

Existují významné rozdíly mezi systémem SAP HANA HANA velké instancí a SAP HANA běžící na virtuálních počítačích Azure nasazené v Azure:

- Neexistuje žádné vrstva virtualizace pro SAP HANA v Azure (velké instance). Zobrazí výkon základní hardware úplné obnovení.
- Na rozdíl od Azure jsou vyhrazené SAP HANA na serveru Azure (velké instance) na konkrétního zákazníka. Není možné, že jednotky serverů nebo hostitele je pevný nebo konfigurace soft-rozdělena na oddíly. V důsledku toho velké Instance HANA jednotka se používá jako jako celek do klienta a které vám přiřazen jako zákazník. Restartování nebo vypnutí serveru automaticky nevede k operačního systému a SAP HANA nasazován na jiném serveru. (Pro typ I třídy SKU, jedinou výjimkou je, pokud server může dojít k potížím a opětovné nasazení je nutné provést na jiném serveru.)
- Na rozdíl od Azure, kde jsou vybrané typy procesoru hostitele pro nejlepší poměr ceny a výkonu, typy procesoru zvolené pro SAP HANA v Azure (velké instance) jsou nejvyšší provádění řádku procesor Intel E7v3 a E7v4.


### <a name="running-multiple-sap-hana-instances-on-one-hana-large-instance-unit"></a>Spuštěním několika instancí SAP HANA na jednu jednotku velké Instance HANA
Je možné k hostování více než jeden aktivní instance SAP HANA na jednotkách HANA velké Instance. Chcete-li stále poskytovat možnosti úložiště snímků a zotavení po havárii, taková konfigurace vyžaduje svazku nastavit na jednu instanci. Od nyní velké Instance HANA jednotky lze dále rozdělit takto:

- S72, S72m, S144, S192: V přírůstcích po 256 GB s 256 GB nejmenší počáteční jednotky. Maximum paměti jednotky lze spojovat různé násobky jako 256 GB, 512 GB a tak dále.
- S144m a S192m: V přírůstcích po 256 GB s 512 GB nejmenší jednotky. Maximum paměti jednotky lze spojovat různé násobky jako 512 GB, 768 GB a tak dále.
- Zadejte třídu II: V přírůstcích po 512 GB s nejmenší počáteční jednotky, 2 TB. Maximum paměti jednotky lze spojovat různé násobky jako 512 GB, 1 TB, 1,5 TB a tak dále.

Některé příklady spuštěním několika instancí SAP HANA může vypadat podobně jako:

| Skladová jednotka (SKU) | Velikost paměti | Velikost Storage | Velikosti s více databází |
| --- | --- | --- | --- |
| S72 | 768 GB | 3 TB | Instance HANA 1 × 768 GB<br /> nebo Instance 1 × 512 GB + 1 × 256 GB Instance<br /> nebo instance 3 x 256 GB | 
| S72m | 1,5 TB | 6 TB | Instance 3x512GB HANA<br />nebo Instance 1 × 512 GB + 1 × 1 TB Instance<br />nebo instance 6 x 256 GB<br />nebo instance 1x1.5 TB | 
| S192m | 4 TB | 16 TB | Instance 8 x 512 GB<br />nebo instance 4 x 1 TB<br />nebo instancí 4 x 512 GB + 2 × 1 TB instancí<br />nebo instancí 4 x 768 GB + instancí 2 x 512 GB<br />nebo Instance 1 × 4 TB |
| S384xm | 8 TB | 22 TB | Instance 4 x 2 TB<br />nebo instancí 2 x 4 TB<br />nebo instancí 2 × 3 TB + 1 × 2 TB instancí<br />nebo instancí 2x2.5 TB + 1 × 3 TB instancí<br />nebo Instance. 1 x 8 TB |


Zobrazí se na nápad. Existují také další varianty jistě. 

### <a name="using-sap-hana-data-tiering-and-extension-nodes"></a>Pomocí rozšíření a SAP HANA dat vrstvení uzlů
SAP podporuje model dat vrstvení pro SAP BW různé verze SAP NetWeaver a SAP BW/4HANA. Podrobnosti ohledně k vrstvení Data modelu najdete v tomto dokumentu a v tomto dokumentu odkazuje SAP blog: [SAP BW/4HANA SAP BW ON HANA s SAP HANA rozšíření uzly a](https://www.sap.com/documents/2017/05/ac051285-bc7c-0010-82c7-eda71af511fa.html#).
Velké instancemi HANA můžete použít možnost 1 konfigurace SAP HANA rozšíření uzlů podle popisu v této – nejčastější dotazy a SAP blog dokumenty. Možnost 2 konfigurace se dá nastavit pomocí následujících HANA velké Instance SKU: S72m, S192, S192m, S384 a S384m.  
Prohlížení dokumentace výhoda nemusí být vidět okamžitě. Ale vyhledávání do pokyny pro nastavení velikosti SAP, můžete zobrazit několik výhod pomocí možnosti-1 a 2 možnost rozšíření uzly SAP HANA. Sem třeba:

- Pokyny k nastavení velikosti SAP HANA obvykle vyžadují dvojnásobek datový svazek jako paměť. Ano při spuštění vaší instance SAP HANA aktivní data, máte jenom 50 % nebo méně paměti, naplní se data. Zbývající paměti je v ideálním případě uchovávat pro SAP HANA provádění své práce.
- Znamená, že v jednotce HANA velké Instance S192 s 2 TB paměti, systémem databázi SAP BW máte pouze 1 TB jako datový svazek.
- Pokud používáte další uzel rozšíření SAP HANA možnost-1, také S192 HANA velké Instance SKU, ho, získáte další kapacitu 2 TB pro datový svazek. V konfiguraci možnost 2 i a další 4 TB pro záložním datový svazek. Ve srovnání s aktivního uzlu, kapacita paměti úplné 'záložním' rozšíření uzlu lze použít pro data ukládání pro možnost 1 a dvojité paměť lze použít pro datový svazek v konfiguraci uzlu Rozšíření SAP HANA možnost-2.
- V důsledku skončili s kapacitou 3 TB pro vaše data a hot záložním poměr 1:2 pro možnost 1 a 5 TB dat a 1:4 poměr v konfiguraci uzlu Rozšíření možnost-2.

Čím datový svazek ve srovnání s paměti, že záložním data jste se žádostí o Čím jsou šance na je však uložený na místo na disku.


## <a name="operations-model-and-responsibilities"></a>Operace modelu a odpovědnosti

Služba součástí SAP HANA v Azure (velké instance) je zarovnán služby Azure IaaS. Zobrazí instance HANA velké instancí s nainstalovaným operačním systémem, která je optimalizovaná pro SAP HANA. Stejně jako u virtuálních počítačů Azure IaaS, většinu úloh posílení operačního systému, budete potřebovat, instalace HANA, operační operačního systému a HANA, instalace další software, a aktualizaci operačního systému a HANA je vaší povinností. Microsoft bez vynucení aktualizace operačního systému, nebo HANA na vás.

![Odpovědnosti SAP HANA v Azure (velké instance)](./media/hana-overview-architecture/image2-responsibilities.png)

Jak můžete vidět v diagramu výše, SAP HANA v Azure (velké instance) je víceklientské infrastruktury jako služby nabídky. A v důsledku toho dělení zodpovědnosti na hranici infrastruktury operačního systému, ve většině případů. Microsoft je zodpovědná za všechny aspekty služby pod čarou operačního systému a odpovídáte výše na řádku, včetně operačního systému. Nejaktuálnější místní metody, které může nasazení dodržování předpisů, zabezpečení, správy aplikací, základ a operačního systému správy, můžete nadále používat. V systémech se zobrazí jako, pokud jsou ve vaší síti všechny namapoval.

Ale tato služba je optimalizovaná pro SAP HANA, existují oblasti, kde jste s Microsoftem musí spolupracují v zájmu použít možnosti základní infrastruktury pro dosažení co nejlepších výsledků.

Následující seznam uvádí další podrobnosti o vrstvy a vaše odpovědnosti:

**Práce v síti,** všechny interní sítě pro razítko velké Instance systémem SAP HANA, přístup k úložišti, připojení mezi instancí (Škálováním na více systémů a další funkce), připojení k povahu a připojení k Azure kde aplikační vrstvu SAP je hostovaná v Azure Virtual Machines. Zahrnuje také připojení k síti WAN mezi datovými centry Azure pro zotavení po havárii pro účely replikace. Všechny sítě jsou rozdělena na oddíly prostřednictvím klienta a provedli QOS.

**Úložiště:** virtualizovaného oddíly úložiště pro všechny svazky, které jsou potřebné SAP HANA servery, a také pro snímky. 

**Servery:** vyhrazených fyzických serverů ke spuštění databáze SAP HANA, přiřazen klientům. Servery typu I třídy SKU jsou abstrahované hardwaru. Konfigurace serveru s těmito typy serverů se shromažďují a udržuje na profily, které můžou přesouvat z jednoho fyzického hardwaru na jiný fyzický hardware. Takové (ruční) přesunu profilu operacemi je možné charakterizovat trochu opravy služby Azure. Servery SKU – třída typu II nejsou nabízí takové funkce.

**SDDC:** software pro správu, který se používá ke správě dat soustředí jako softwarově definované entity. Ho fond zdrojů pro škálování, dostupnosti a z důvodů výkonu umožňuje společnosti Microsoft.

**Podporované OS:** operačního systému zvolíte (SUSE Linux nebo Red Hat Linux), se spuštěným na serverech. Bitových kopií operačního systému, které jsou k dispozici jsou obrázky poskytnutý dodavatelem pro jednotlivé Linux společnosti Microsoft pro účely spuštění SAP HANA. Musíte mít předplatné s dodavatelem Linux pro tento konkrétní obrázek optimalizované SAP HANA. Vaše odpovědnosti zahrnují registrace bitové kopie s dodavatelem operačního systému. Z bodu předání společností Microsoft a jste také zodpovědná za jakékoli další opravy operačního systému Linux. Tato opravy také obsahuje další balíčky, které může být nezbytné pro úspěšnou instalaci SAP HANA (naleznete na SAP HANA instalace dokumentace a poznámky SAP) a které nebyly zahrnuty podle konkrétní dodavatele Linux v jejich SAP HANA optimalizované bitové kopie operačního systému. Je zodpovědností zákazníků zahrnuje taky opravy operačního systému, které souvisí s selhání nebo optimalizace operačního systému a jeho ovladače související s konkrétní serverový hardware. Nebo žádné zabezpečení nebo funkční opravy operačního systému. Odpovědnosti zákazníka je také monitorování a plánování kapacity služby:

- Využití prostředků procesoru
- Paměťové nároky
- Diskové svazky související s volného místa, IOPS a čekací doba
- Svazek provoz sítě mezi HANA velké Instance a SAP aplikační vrstvu

Základní infrastruktura velké instancí HANA poskytuje funkce pro zálohování a obnovení svazku operačního systému. Pomocí této funkce je také vaší povinností.

**Middleware:** SAP HANA především Instance. Správu, operace a monitorování jsou vaše zodpovědnosti. Za předpokladu, který vám umožňuje používat úložiště snímků pro zálohování a obnovení a zotavení po havárii pro účely není funkce. Tyto možnosti jsou poskytovány infrastruktury. Vaše odpovědnosti však také zahrnovat navrhování vysoké dostupnosti nebo zotavení po havárii s těmito možnostmi, jejich využití a monitorování úložiště snímků již byly úspěšně provedeny.

**Data:** vaše data spravovaná přes SAP HANA a další data, jako je zálohování sdílí soubory umístěné na svazcích nebo souboru. Vaše odpovědnosti zahrnují monitorování volného místa na disku a obsah na svazcích, správy a monitorování úspěšné provedení zálohy diskové svazky a úložiště snímků. Úspěšné provedení replikaci dat do lokalit zotavení po Havárii však zodpovídá Microsoft.

**Aplikace:** instancí aplikace SAP nebo v případě jiných SAP aplikace, aplikační vrstvu těchto aplikací. Vaše odpovědnosti zahrnují nasazení, správu, operace a monitorování těchto aplikací týkající se plánování kapacity využití prostředků procesoru, využití paměti, spotřebu úložiště Azure a spotřeba šířky pásma sítě v rámci virtuálních sítí Azure a z virtuálních sítí Azure s SAP HANA v Azure (velké instance).

**WAN:** připojení, je vytvořit z místního nasazení Azure pro úlohy. Naše zákazníky s instancí velké HANA použijte Azure ExpressRoute pro připojení k síti. Toto připojení není součástí SAP HANA na řešení Azure (velké instance), takže jste zodpovědní za nastavení pro toto připojení.

**Archiv:** chcete archivovat kopií dat pomocí vlastních metod v účtech úložiště. Archivace vyžaduje správu, dodržování předpisů, náklady a operace. Jste zodpovědní za generování archivu kopií a záloh v Azure a uložit je do kompatibilní způsobem.

Najdete v článku [SLA pro SAP HANA v Azure (velké instance)](https://azure.microsoft.com/support/legal/sla/sap-hana-large/v1_0/).

## <a name="sizing"></a>Nastavení velikosti

Nastavení velikosti pro velké instance HANA je nejsou jiné než dimenzování pro HANA obecně. U stávajících a nasadit systémy, které chcete přesunout z jiných relační k HANA, SAP poskytuje řadu sestav, které běží na vaše stávající systémy SAP. Pokud databázi přesunete do HANA, tyto sestavy zkontrolujte data a výpočet požadavků na paměť pro instanci HANA. Přečtěte si následující poznámky SAP, chcete-li získat další informace o tom, jak spustit tyto sestavy a jak získat jejich nejnovější opravy nebo verze:

- [Poznámka SAP #1793345 - dimenzování pro sadě SAP HANA](https://launchpad.support.sap.com/#/notes/1793345)
- [Poznámka: #1872170 - Suite na velikosti sestavy HANA a S/4 HANA SAP](https://launchpad.support.sap.com/#/notes/1872170)
- [Poznámka SAP #2121330 – nejčastější dotazy: SAP BW na HANA Změna velikosti sestav](https://launchpad.support.sap.com/#/notes/2121330)
- [Poznámka: #1736976 - sestavu nastavení velikosti pro BW HANA SAP](https://launchpad.support.sap.com/#/notes/1736976)
- [SAP Poznámka #2296290 - nová sestava nastavení velikosti pro BW na HANA](https://launchpad.support.sap.com/#/notes/2296290)

Pro implementace zelená pole je k dispozici vypočítat požadavky na paměť implementace SAP softwaru nad HANA SAP rychlé přizpůsobení velikosti symbolů.

Požadavky na paměť pro HANA se zvyšuje s růstem datový svazek, chcete mít přehled o paměťové nároky a možné předpovědět, co se má být v budoucnosti. V závislosti na požadavcích paměti, poté můžete namapovat vaše vyžádání do jedné Instance SKU velké HANA.

## <a name="requirements"></a>Požadavky

Tento seznam sestaví požadavky na spuštění SAP HANA v Azure (větší instance).

**Microsoft Azure:**

- Předplatné Azure, který může být propojený SAP HANA v Azure (velké instance).
- Kontrakt Microsoft Premier Support. V tématu [SAP podporu Poznámka #2015553 – SAP v Microsoft Azure: podpora požadavky](https://launchpad.support.sap.com/#/notes/2015553) pro konkrétní informace související se spouštěním SAP v Azure. HANA velké instance jednotky pomocí 384 a více procesorů, musíte také rozšířit smlouvu Premier Support zahrnout Azure rychlé odpovědi (ARR).
- Povědomí o velké instance SKU je nutné po provedení velikosti uplatňovat s SAP HANA.

**Připojení k síti:**

- Azure ExpressRoute mezi místním nasazením a Azure: překážek místní datacentra připojit k Azure, nezapomeňte uspořádat alespoň 1 GB/s připojení od vašeho poskytovatele internetových služeb. 

Operační systém:

- Licence pro SUSE Linux Enterprise Server 12 pro SAP aplikace.

> [!NOTE] 
> Operační systém dodaných společností Microsoft není registrován u SUSE, ani je propojená s instancí SMT.

- SUSE Linux předplatné správy nástroj (SMT) nasazené v Azure na virtuální počítač Azure. To poskytuje možnost pro SAP HANA v Azure (velké instance) pro zápis a v uvedeném pořadí aktualizován SUSE (jako je žádný přístup k Internetu v rámci HANA velké instance datového centra). 
- Licence pro Red Hat Enterprise Linux 6.7 nebo 7.2 pro SAP HANA.

> [!NOTE]
> Operační systém dodaných společností Microsoft není registrován u Red Hat, ani je připojen k instanci Red Hat odběr Manager.

- Red Hat odběr Manager nasazené v Azure na virtuální počítač Azure. Správce předplatného Red Hat poskytuje možnost pro SAP HANA v Azure (velké instance) pro zápis a v uvedeném pořadí aktualizován Red Hat (jako je žádný přímý přístup k Internetu z v rámci klienta nasazené na značce velké instanci Azure).
- SAP vyžaduje, abyste měli podporu smlouvy s poskytovatelem Linux. Tento požadavek není vymazat řešení pro velké instancí HANA nebo fakt, na který vaší spuštění Linux v Azure. Na rozdíl od pomocí některé z Galerie obrázků Linux Azure, poplatek služby není součástí řešení nabídky velkých instancí HANA. Je na vás jako zákazník splnit požadavky SAP o smlouvách, podporu s distributorem Linux.   
   - Pro systémy SUSE Linux vyhledat požadavky na podporu kontraktu v [SAP Poznámka #1984787 - SUSE LINUX Enterprise Server 12: poznámky k instalaci](https://launchpad.support.sap.com/#/notes/1984787) a [&#1056161; Poznámka SAP - SUSE Priority podporu pro aplikace SAP](https://launchpad.support.sap.com/#/notes/1056161).
   - Red Hat Linux je potřeba mít úrovně správné předplatné, které zahrnují podporu a služby (aktualizace pro operační systémy velkých instancí HANA. Red Hat doporučuje získávání "RHEL pro [SAP řešení](https://access.redhat.com/solutions/3082481)" předplatného. 

Pro různé verze SAP HANA s různými verzemi Linux na matici podpory, zkontrolujte [&#2235581; Poznámka SAP](https://launchpad.support.sap.com/#/notes/2235581).


**Databáze:**

- Licence a součásti instalace softwaru pro SAP HANA (platforma nebo enterprise edition).

**Aplikace:**

- Licence a softwarové součásti od instalace pro všechny aplikace SAP připojení k SAP HANA a související SAP podporovat smluv.
- Licence a softwarové součásti od instalace pro všechny aplikace bez SAP používá ve vztahu k SAP HANA v prostředí Azure (velké instance) a související smlouvy o podpoře.

**Dovedností:**

- Zkušenosti a znalosti o Azure IaaS a jeho komponenty.
- Zkušenosti a znalosti o nasazení SAP zatížení v Azure.
- Instalace SAP HANA certifikované osobní.
- SAP architekti dovednosti pro návrh vysokou dostupnost a zotavení po havárii kolem SAP HANA.

**SAP:**

- Očekává se, SAP zákazníky a mají podporu kontrakt s SAP
- Zejména pro implementace na třídu typu II HANA velké Instance SKU důrazně doporučujeme prostudovat s SAP ve verzích systému SAP HANA a závěrečné konfigurace na velké velikostí škálování hardwaru.


## <a name="storage"></a>Úložiště

Rozložení úložiště pro SAP HANA v Azure (velké instance) se nakonfiguruje prostřednictvím SAP HANA na Azure Service Management prostřednictvím SAP Doporučené pokyny, dokumentovány v článku [požadavky na úložiště SAP HANA](http://go.sap.com/documents/2015/03/74cdb554-5a7c-0010-82c7-eda71af511fa.html) dokumentu white paper.

HANA velké instance typu I třídy se dodávají s čtyřikrát paměti svazek jako svazek úložiště. Pro typ třídy II jednotek HANA velké Instance úložiště není má být čtyřikrát Další. Jednotky se dodávají s svazek, který je určený pro ukládání HANA zálohování transakčního protokolu. Najít další podrobnosti v [postup instalace a konfigurace SAP HANA (velké instance) na Azure](hana-installation.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

Viz následující tabulka uvádí přidělení úložiště. V tabulce jsou uvedeny zhruba kapacity pro jiné svazky zadaná u jiné Instance velké HANA jednotek.

| HANA velké Instance SKU | hana/data | hana/log | / sdílené Hana | Hana/log/zálohování |
| --- | --- | --- | --- | --- |
| S72 | 1280 GB | 512 GB | 768 GB | 512 GB |
| S72m | 3328 GB | 768 GB |1280 GB | 768 GB |
| S192 | 4608 GB | 1024 GB | 1536 GB | 1024 GB |
| S192m | 11,520 GB | 1536 GB | 1792 GB | 1536 GB |
| S384 | 11,520 GB | 1536 GB | 1792 GB | 1536 GB |
| S384m | 12 000 GB | 2050 GB | 2050 GB | 2040 GB |
| S384xm | 16 000 GB | 2050 GB | 2050 GB | 2040 GB |
| S576 | 20 000 GB | 3100 GB | 2050 GB | 3100 GB |
| S768 | 28,000 GB | 3100 GB | 2050 GB | 3100 GB |
| S960 | 36,000 GB | 4100 GB | 2050 GB | 4100 GB |


Skutečné nasazené svazky se může lišit podle trochu nasazení a nástroj, který se používá k zobrazení velikosti svazků.

Pokud jste rozdělit HANA velké Instance SKU, bude vypadat několik příkladů možné dělení částí:

| Oddíl paměti v GB | hana/data | hana/log | / sdílené Hana | Hana/log/zálohování |
| --- | --- | --- | --- | --- |
| 256 | 400 GB | 160 GB | 304 GB | 160 GB |
| 512 | 768 GB | 384 GB | 512 GB | 384 GB |
| 768 | 1280 GB | 512 GB | 768 GB | 512 GB |
| 1024 | 1792 GB | 640 GB | 1024 GB | 640 GB |
| 1536 | 3328 GB | 768 GB | 1280 GB | 768 GB |


Tyto velikosti jsou čísla hrubý svazku, která mírně založené na nasazení a nástroje pro podívejte se na svazky se může lišit. Také existují další oddíl velikosti thinkable, jako je 2,5 TB. Velikosti úložiště by se počítá pomocí podobné vzorec jako použité pro výše uvedené oddíly. Termín oddíly neznamená, že operační systém, paměť nebo prostředky procesoru jsou v některém způsobem rozdělena na oddíly. Označuje právě oddílů pro úložiště pro různé HANA instance, které chcete nasadit na jedna jednotka HANA velké Instance. 

Jako zákazník může mít potřebujete další úložiště, máte možnost Přidat úložiště přikoupit dodatečné úložiště v jednotkách 1 TB. Toto dodatečné úložiště se dá přidat jako další svazek nebo můžete použít k rozšíření jeden nebo více existujících svazcích. Není možné snížit velikost svazků původně nasazení a většinou zdokumentovat, výše uvedené tabulek. Také není možné změnit názvy svazků nebo se připojte názvy. Svazky úložiště, jak je popsáno výše jsou připojeny k instanci HANA velké jednotky jako NFS4 svazky.

Jako zákazník můžete použít snímky úložiště pro zálohování a obnovení a po havárii pro účely obnovení. Další podrobnosti jsou podrobně popsané na [SAP HANA (velké instance) vysokou dostupnost a zotavení po havárii v Azure](hana-overview-high-availability-disaster-recovery.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

### <a name="encryption-of-data-at-rest"></a>Šifrování neaktivních uložených dat
Úložiště použitého pro velké instancí HANA umožňuje transparentní šifrování dat, který je uložen na discích. V době nasazení HANA velké jednotky Instance máte možnost tak, aby měl tento druh povolit šifrování. Můžete také změnit po nasazení již na šifrované svazky. Přesunutí ze bez šifrování šifrovaných svazcích je transparentní a nevyžaduje s prodlevou. 

S typem I třídy SKU, logická jednotka je uložený ve, spouštěcí svazek je zašifrován. V případě typu třídy II SKU z HANA velké instancí je potřeba šifrovat spouštění logické jednotky s metody operačního systému. Další informace kontaktujte tým Microsoft Service Management.


## <a name="networking"></a>Sítě

Architektura sítě Azure je klíčovou součástí úspěšné nasazení SAP aplikací na velké instance HANA. SAP HANA na nasazení Azure (velké instance) obvykle mají větší šířku SAP v několika různých řešeních pro SAP s různou velikost databáze, využití prostředků procesoru a využití paměti. Pravděpodobně ne všechny tyto systémy SAP je založena na SAP HANA tak vaše SAP šířku by pravděpodobně hybridní, který používá:

- Nasazení SAP systémy místně. Z důvodu jeho velikosti nelze tyto systémy aktuálně hostované v Azure; classic Příkladem může být produkčního systému SAP ERP systémem Microsoft SQL Server (jako databáze), který vyžaduje více procesorů nebo paměťové prostředky, které můžete zadat virtuální počítače Azure.
- Nasazení na základě SAP HANA SAP systémy místně.
- Nasazené SAP systémy ve virtuálních počítačích Azure. Tyto systémy můžou být vývoj, testování, izolovaného prostoru, nebo produkční instance pro všechny aplikace na základě SAP NetWeaver, které můžete úspěšně nasazovat v Azure (na virtuálních počítačích), na základě poptávky prostředků využívání a paměti. Tyto systémy také může být na základě databází, jako je SQL Server (najdete v části [SAP podporu Poznámka #1928533 – SAP aplikace v Azure: typy podporovaných produktů a virtuální počítač Azure](https://launchpad.support.sap.com/#/notes/1928533/E)) nebo SAP HANA (najdete v části [certifikované platformy IaaS aplikace SAP HANA](http://global.sap.com/community/ebook/2014-09-02-hana-hardware/enEN/iaas.html)).
- Nasazení SAP aplikační servery v Azure (na virtuálních počítačích), které využívají SAP HANA v Azure (velké Instance) v Azure velké Instance razítka.

Při typických hybridní SAP na šířku (se čtyřmi nebo více různé scénáře nasazení) existují mnoho zákazníků případů dokončení SAP šířku běžící v Azure. Jako virtuální počítače Microsoft Azure se stávají výkonnější, se zvyšuje počet zákazníků přesunutí svá řešení SAP do Azure.

Azure sítě v kontextu systému SAP nasazené v Azure není složité. Je založena na následující zásady:

- Virtuální sítě Azure (virtuální sítě) musí být připojení k okruhu Azure ExpressRoute, která se připojuje k místní síti.
- Okruh ExpressRoute, obvykle připojení místní do Azure by měl mít šířky pásma s 1 GB/s nebo vyšší. Tato minimální šířku pásma přenosu dat mezi místními systémy a systémy s operačním systémem na virtuálních počítačích Azure (stejně jako připojení k systémům Azure z koncoví uživatelé na místě) umožňuje odpovídající šířku pásma.
- Všechny systémy SAP v Azure je nutné nastavit ve virtuálním sítím Azure pro komunikaci mezi sebou.
- Active Directory a DNS hostované na místě jsou rozšířené do Azure prostřednictvím ExpressRoute z místní.


> [!NOTE] 
> Z fakturace hlediska jenom jedno předplatné Azure může být propojený jenom jeden jednoho klienta v velké Instance razítka v určité oblasti Azure, a naopak jednoho klienta razítko velké Instance může být propojený jenom jedno předplatné. Tuto skutečnost není různé na všechny ostatní fakturovatelný objekty v Azure

Nasazení SAP HANA v Azure (velké instance) v několika různých oblastech Azure, výsledkem samostatné klienta k nasazení ve velkých Instance razítka. Nicméně můžete spustit i v rámci stejného předplatného Azure tak dlouho, dokud tyto instance jsou součástí stejné šířku SAP. 

> [!IMPORTANT] 
> SAP HANA v Azure (velké instance) podporuje pouze nasazení Správa prostředků Azure.

 

### <a name="additional-azure-vnet-information"></a>Další informace o virtuální síť Azure

Aby bylo možné připojit virtuální síti Azure expressroute, je nutné vytvořit bránu Azure (viz [o brány virtuální sítě pro ExpressRoute](../../../expressroute/expressroute-about-virtual-network-gateways.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)). Služba Azure gateway lze použít buď s ExpressRoute infrastruktury mimo Azure (nebo s Azure velké instance razítka), nebo se pro připojení mezi virtuálními sítěmi Azure (viz [nakonfigurovat připojení VNet-to-VNet pro Resource Manager pomocí prostředí PowerShell](../../../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)). Tak dlouho, dokud tato připojení pocházejí z různých směrovače MS Enterprise okraje (MSEE) se můžete připojit bránu Azure až na čtyři různá připojení ExpressRoute.  Další informace najdete v tématu [infrastruktury SAP HANA (velké instance) a připojení v Azure](hana-overview-infrastructure-connectivity.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). 

> [!NOTE] 
> Propustnost poskytuje služba Azure gateway se liší pro oba případy použití (viz [o službě VPN Gateway](../../../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)). Maximální propustnost, kterou jsme můžete dosáhnout s bránou virtuální sítě je 10 GB/s, pomocí připojení ExpressRoute. Mějte na paměti, že kopírování souborů mezi virtuální počítač Azure umístěný ve službě Azure VNet a systému místní (jako jedna kopie datový proud) není dosahuje úplnou propustnost různých SKU brány. Využívat šířku pásma dokončení brány virtuální sítě, musíte použít víc datových proudů nebo zkopírujte různé soubory v paralelní datové proudy jednoho souboru.


### <a name="networking-architecture-for-hana-large-instances"></a>Architektura sítě pro velké instance HANA
Sítě architekturu pro velké instance HANA jak je uvedeno níže, je možné oddělit do čtyř částí:

- Místní sítě a připojení ExpressRoute do Azure. Tato část je doména, zákazníků a připojení k Azure prostřednictvím ExpressRoute. Toto je část v pravém dolním rohu grafiky níže.
- Azure sítě jako stručně výše popsané s virtuálními sítěmi Azure, který znovu mít brány. Toto je oblast potřebujete-li najít odpovídající návrhy pro vaše požadavky na aplikace, zabezpečení a požadavky na dodržování předpisů. Pomocí HANA velké instancí je jiný bod úvahu z hlediska počtu SKU zvolit virtuálních sítí a Azure brány. Toto je část v pravém horním rohu grafiky.
- Připojení velké instancí HANA prostřednictvím ExpressRoute technologie do Azure. Tato část nasazení a postará Microsoft. Všechny je nutné provést jako zákazník je poskytnutí některých IP rozsahy adres a po nasazení vaše prostředky v HANA velké instance připojení ExpressRoute okruhu Azure VNet(s) (viz [infrastruktury SAP HANA (velké instance) a připojení v Azure](hana-overview-infrastructure-connectivity.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)). 
- Sítě v HANA velké instance, který je ve většině případů transparentní pro vás jako zákazník.

![Virtuální síť Azure připojené k SAP HANA na Azure (velké instance) a na místě](./media/hana-overview-architecture/image3-on-premises-infrastructure.png)

Fakt použít HANA velké instancí nezmění požadavek na získat vaše místní prostředky připojené prostřednictvím ExpressRoute do Azure. Také nemění požadavek na jeden nebo více virtuálních sítí patřících spustit virtuální počítače Azure hostované aplikační vrstvu, která se připojuje k HANA instance hostitele, který v jednotkách HANA velké Instance. 

Rozdíl pro nasazení SAP v čistě Azure dodává s tato fakta který:

- Instance HANA velké jednotky vašeho zákazníka klienta jsou připojené prostřednictvím jiného okruhu ExpressRoute do vaší VNet(s) Azure. Chcete-li samostatné zatížení, místní pro ExpressRoute virtuálních sítí Azure a odkazy mezi virtuálními sítěmi Azure a HANA velké instance nesdílejí stejný směrovače.
- Profil zatížení mezi SAP aplikační vrstvu a HANA Instance je jiný druh mnoho malých požadavků a shluků jako data přenáší (sady výsledků) ze SAP HANA do aplikační vrstvu.
- Architektura aplikace SAP je více citlivá na latenci sítě než typické scénáře, kde získá data vyměňují mezi místními a Azure.
- Bránu virtuální sítě má alespoň dvě připojení ExpressRoute a obě připojení sdílet maximální šířka pásma pro příchozí data brány virtuální sítě.

Latence sítě zkušeného mezi virtuálními počítači Azure a HANA velké jednotky instance může být vyšší než typické latence operace round-trip sítě virtuálních počítačů VM. Závisí na oblast Azure, hodnoty měřenou může být vyšší než 0.7 ms čekací doba přenosu jsou klasifikovány jako níže průměr v [SAP Poznámka #1100926 – nejčastější dotazy: výkon sítě](https://launchpad.support.sap.com/#/notes/1100926/E). Nicméně zákazníkům nasazené aplikace na základě SAP HANA produkční SAP velmi úspěšně na velké instance SAP HANA. Zákazníci, kteří nasazenou hlášených skvělé vylepšení svých aplikací SAP systémem SAP HANA pomocí Instance HANA velké jednotky. Nicméně byste měli otestovat své obchodní procesy důkladně v Azure HANA velké instancí.
 
Chcete-li poskytovat latence deterministický sítě mezi virtuálními počítači Azure a velké Instance HANA, je nezbytné volba skladová položka brány virtuální sítě Azure. Na rozdíl od vzory přenosů dat mezi místní a virtuální počítače Azure můžete vyvíjet vzoru provoz mezi virtuálními počítači Azure a velké instancí HANA malé ale vysoké shluky požadavky a datové svazky přenášet. Aby bylo možné používat takové shluky zpracovává dobře, důrazně doporučujeme použití UltraPerformance SKU brány. Pro třídu typu II HANA velké Instance SKU je povinné použití brány UltraPerformance SKU jako brána virtuální sítě Azure.  

> [!IMPORTANT] 
> Zadaný celkový provoz sítě mezi databáze vrstvy a aplikaci SAP, HighPerformance nebo UltraPerformance SKU brány pro virtuální sítě je podporována pro připojení k SAP HANA v Azure (velké instance). Pro velké HANA instance typu II SKU je podporováno pouze UltraPerformance skladová položka brány jako brána virtuální sítě Azure.



### <a name="single-sap-system"></a>Jednoho systému SAP

Na místní infrastrukturu výše uvedeném připojené prostřednictvím ExpressRoute do Azure a okruh ExpressRoute připojí do Microsoft Enterprise Edge směrovač (MSEE) (viz [technický přehled ExpressRoute](../../../expressroute/expressroute-introduction.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)). Po vytvoření této trase připojí do páteřní Microsoft Azure, a všechny oblasti Azure dostupná.

> [!NOTE] 
> SAP krajiny běžící v Azure, připojte k nejbližší oblast Azure z povahu SAP MSEE. Azure velké Instance razítka jsou připojené prostřednictvím vyhrazená zařízení MSEE. Chcete-li minimalizovat latence sítě mezi virtuálními počítači Azure v Azure IaaS a velké Instance razítka.

Brána virtuální sítě pro virtuální počítače Azure, který je hostitelem instance aplikace SAP, je připojená k tento okruh ExpressRoute a stejnou virtuální síť je připojený k samostatné směrovači MSEE vyhrazený pro připojení k velké Instance razítka.

Toto je jednoduchý příklad jednoho systému SAP, kde aplikační vrstvu SAP je hostován v Azure a databázi SAP HANA běží na SAP HANA v Azure (velké instance). Předpokladem je, že šířku pásma brány virtuální sítě 2 GB/s nebo 10 GB/s propustnost nepředstavuje úzkým místem.

### <a name="multiple-sap-systems-or-large-sap-systems"></a>Více systémů SAP nebo velké systémy SAP

Pokud více systémů SAP nebo velké SAP systémy jsou nasazeny, připojení k SAP HANA v Azure (velké instance), jeho & č. 39; s možné logicky předpokládat, že propustnosti brány virtuální sítě může stát úzkým místem. V takovém případě budete muset rozdělení aplikace vrstvy na více virtuálních sítí Azure. Toto nastavení také může být recommendable vytvořit speciální virtuální sítě, který je připojen k HANA velké instance pro případy, jako je:

- Provádění zálohování přímo z instancí HANA ve velkých instancí HANA k virtuálnímu počítači v Azure, který je hostitelem sdílených složek NFS
- Kopírování velkých zálohy nebo další soubory z Instance HANA velké jednotky na spravovat v Azure místo na disku.

Pomocí samostatné virtuální sítě, že hostitel virtuálních počítačů, které spravovat úložiště zabraňuje dopad na velkých souborů nebo přenos dat z velké instancí HANA do Azure na bráně virtuální sítě, která má virtuální počítače spuštěné SAP aplikační vrstvu. 

Pro více škálovatelná architektura sítě:

- Využívejte více virtuálními sítěmi Azure pro jeden, větší SAP aplikační vrstvu.
- Nasaďte jeden samostatný virtuální síť Azure pro každý systém SAP nasazení ve srovnání s kombinace těchto systémů SAP v oddělené podsítě pod stejnou virtuální síť.

 Větší škálovatelnost sítě architektura pro SAP HANA v Azure (velké instance):

![Nasazení SAP aplikační vrstvu nad více virtuálními sítěmi Azure](./media/hana-overview-architecture/image4-networking-architecture.png)

Nasazení SAP aplikační vrstvu nebo součásti, prostřednictvím více virtuálními sítěmi Azure, jako v příkladu nahoře, zavedl nevyhnutelné latence režie, které došlo při komunikaci mezi aplikacemi hostované v těchto virtuálních sítí Azure. Ve výchozím nastavení síťový provoz mezi virtuálními počítači Azure umístěné v různých virtuálních sítí směrovat přes směrovači MSEE v této konfiguraci. Nicméně od září 2016 tento směrování lze optimalizovat. Způsob, jak optimalizovat a omezit latence při komunikaci mezi oběma virtuálními sítěmi je partnerského vztahu Azure virtuálních sítí ve stejné oblasti. I když tyto virtuální sítě jsou v různých předplatných. Virtuální síť Azure partnerský vztah, komunikace mezi virtuálními počítači ve dvou různých virtuálních sítí Azure pomocí páteřní síti Azure přímou komunikaci mezi sebou. Tím zobrazující podobnou latenci jako v případě, že virtuální počítače by být ve stejné virtuální síti. Zatímco provoz, rozsahů IP adres, které jsou připojené přes bránu virtuální sítě Azure adresování směrován přes bránu VNet jednotlivé sítě vnet. Můžete získat podrobnosti o službě Azure VNet partnerský vztah v článku [VNet peering](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview).


### <a name="routing-in-azure"></a>Směrování v Azure

Existují tři aspekty směrování důležité sítě pro SAP HANA v Azure (velké instance):

1. SAP HANA v Azure (velké instance) lze přistupovat pouze prostřednictvím virtuálních počítačích Azure a prostřednictvím vyhrazené připojení ExpressRoute. Ne přímo z místní. Přímý přístup z místního s jednotkami velké Instance HANA dodaných společností Microsoft, není možné okamžitě z důvodu přenositelné směrování omezení aktuální architektury síť Azure, použít pro velké instance SAP HANA. Někteří klienti správy a všechny aplikace, které vyžadují přímý přístup, jako je například SAP řešení správce spuštěného na místě, se nemůže připojit k databázi SAP HANA.

2. Pokud máte velké Instance HANA jednotky nasazené ve dvou různých oblastech Azure za účelem zotavení po havárii, platí stejné omezení směrování přechodný. Nebo jinými slovy, nebude IP adresy Instance HANA velké jednotky v jedné oblasti (například USA – západ) směrovat na jednotce HANA velké Instance je nasazena v jiné oblasti (například USA – východ). Toto je nezávislé na využití sítě Azure partnerského vztahu v oblastech nebo křížové připojení okruhy ExpressRoute, který je připojen velké Instance HANA jednotky k virtuálním sítím Azure. Jak ukazuje trochu další dolů v této dokumentaci. Toto omezení, která se dodává s nasazené architektury, bude zakázat okamžité využití replikace systému HANA jako funkci obnovení po havárii.

3. SAP HANA na jednotkách Azure (velké instance) mají přiřazenou IP adresu z fondu serverů IP adresy rozsahu můžete jako zákazník odeslána (viz [infrastruktury SAP HANA (velké instance) a připojení v Azure](hana-overview-infrastructure-connectivity.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) podrobnosti).  Tato IP adresa je přístupná prostřednictvím předplatných Azure a ExpressRoute, která se připojuje virtuální sítě Azure k HANA v Azure (velké instance). IP adresa, přiřazené mimo tento fond IP serveru rozsah adres je přímo přiřazena k jednotce hardwaru a není NAT'ed už jako to byla velká písmena v prvním nasazení tohoto řešení. 

> [!NOTE] 
> Pokud potřebujete k překonání omezení v přechodný směrování, jak je popsáno v uvedených první dva seznam položek, budete muset použít další součásti pro směrování. Součásti, které lze použít k překonání omezení může být: zpětného proxy pro data trasy, která do a z. Například F5 BIG-IP, NGINX s Traffic Manager nasazené v Azure jako virtuální brány firewall nebo provoz směrování řešení.
> Pomocí [IPTables pravidla](http://www.linuxhomenetworking.com/wiki/index.php/Quick_HOWTO_%3a_Ch14_%3a_Linux_Firewalls_Using_iptables#.Wkv6tI3rtaQ) v virtuálního počítače s Linuxem umožňující směrování mezi místními umístěními a Instance HANA velké jednotky, nebo mezi jednotky HANA velké Instance v různých oblastech.
> Upozorňujeme, že implementace a podpora pro vlastní řešení zahrnující síťových zařízení třetích stran nebo IPTables není poskytované společností Microsoft. Podpora musí být od příslušného dodavatele komponenta, kterou používá nebo integrátor. 

### <a name="internet-connectivity-of-hana-large-instances"></a>Připojení k Internetu velké instancí HANA
Velké instancí HANA nemají přímé připojení k Internetu. Toto je omezení vaše možnosti, například zaregistrovat bitovou kopii operačního systému přímo s dodavatelem operačního systému. Proto možná budete muset spolupracovat s místní server SLES SMT nebo RHEL odběr Manager

### <a name="data-encryption-between-azure-vms-and-hana-large-instances"></a>Šifrování dat mezi virtuálními počítači Azure a velké instancí HANA
Není zašifrovaná data přenesená mezi instancemi velké HANA a virtuálních počítačích Azure. Můžete však výhradně pro výměnu mezi straně HANA databázového systému a aplikací na základě JDBC nebo rozhraní ODBC povolit šifrování přenosů. Referenční dokumentace [této dokumentace podle SAP](http://help-legacy.sap.com/saphelp_hanaplatform/helpdata/en/db/d3d887bb571014bf05ca887f897b99/content.htm?frameset=/en/dd/a2ae94bb571014a48fc3b22f8e919e/frameset.htm&current_toc=/en/de/ec02ebbb57101483bdf3194c301d2e/plain.htm&node_id=20&show_children=false)

### <a name="using-hana-large-instance-units-in-multiple-regions"></a>Pomocí HANA velké jednotky Instance v několika oblastech

Z jiných důvodů k nasazení SAP HANA v Azure (velké instance) může mít v několika oblastech Azure, kromě zotavení po havárii. Možná chcete získat přístup ze všech virtuálních počítačích nasazených v různých virtuálních sítí v oblastech HANA velké instancí. Vzhledem k tomu, že IP adresy přiřazené k jiné jednotky velké instancí HANA nebudou rozšířeny nad rámec virtuální sítě Azure (které jsou přímo připojené prostřednictvím jejich brány na instance), je mírné změnu návrhu sítě VNet zavedená výše: bránu virtuální sítě Azure může zpracovat čtyři jiné okruhy ExpressRoute mimo jiné Msee a každý virtuální síť, která je připojena k jednomu razítek velké Instance může být připojen k velké Instance razítka v jiné oblasti Azure.

![Virtuálních sítí Azure připojené k Azure velké Instance razítka v různých oblastech Azure](./media/hana-overview-architecture/image8-multiple-regions.png)

Předchozí obrázek ukazuje, jak různé sítě Azure Vnet v obou oblastech připojeni k dva různé okruhy ExpressRoute, které se používají pro připojení k SAP HANA v Azure (velké instance) v obou oblastech Azure. Nově přináší připojení jsou obdélníková červené čáry. Tato připojení z virtuální sítě Azure virtuální počítače spuštěné v jednom z těchto virtuálních sítí mohou mít přístup ke každé z různých instancí velké HANA jednotky nasazené ve dvou oblastech. Jak vidíte v grafiky výše uvedené, se předpokládá, že máte dvě připojení ExpressRoute z místního dvou oblastech Azure; Doporučujeme důvodů zotavení po havárii.

> [!IMPORTANT] 
> Pokud se používají více okruhů ExpressRoute, by měla zajistit správné směrování provozu použít předřazení AS Path a místní BGP předvoleb nastavení.


