---
title: "Aspekty plánování pro Azure zásobníku integrované systémy | Microsoft Docs"
description: "Zjistěte, co můžete dělat teď plánování a příprava pro více uzly Azure zásobníku."
services: azure-stack
documentationcenter: 
author: twooley
manager: byronr
editor: 
ms.assetid: 90f8fa1a-cace-4bfa-852b-5abe2b307615
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/16/2017
ms.author: twooley
ms.openlocfilehash: 8484f7947f23a00c05b34babf13cd75f9d227740
ms.sourcegitcommit: a7c01dbb03870adcb04ca34745ef256414dfc0b3
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/17/2017
---
# <a name="planning-considerations-for-azure-stack-integrated-systems"></a>Aspekty plánování pro Azure zásobníku integrované systémy

*Platí pro: Azure zásobníku integrované systémy*

Pokud byste chtěli v systému Azure zásobníku integrované, budete chtít pochopit hlavní informace o plánování řešení nasazení a jak systému zapadá do vašeho datového centra. Toto téma obsahuje souhrnné informace o těchto aspektů.

Pokud se rozhodnete koupit integrovaný systém, pomáhá dodavatele hardwaru, výrobce (OEM), vás provede velkou část procesu plánování podrobněji. Také provede skutečné nasazení.

## <a name="management-considerations"></a>Aspekty správy

Azure zásobník je zapečetěné systému, kde infrastruktury je uzamčené z oprávnění i a síťový perspektivy. Sítě seznamy řízení přístupu (ACL) se použijí pro blokovat veškerý příchozí provoz neoprávněným a všechny zbytečné komunikace mezi součástmi infrastruktury. Díky tomu je obtížné neoprávněným uživatelům přístup k systému.

Pro každodenní správu a operace neexistuje žádný správce neomezený přístup k infrastruktuře. Operátory Azure zásobníku musí spravovat systém prostřednictvím portálu správce nebo prostřednictvím Správce Azure Resource Manager (prostřednictvím prostředí PowerShell nebo rozhraní REST API). Není k dispozici přístup k systému pomocí jiné nástroje pro správu, například Správce technologie Hyper-V nebo Správce clusteru převzetí služeb při selhání. K ochraně systému, nelze nainstalovat software jiných výrobců (například agenty) uvnitř součástí infrastruktury Azure zásobníku. Vzájemná funkční spolupráce s externí správu a zabezpečení softwaru dojde k pomocí prostředí PowerShell nebo rozhraní REST API.

Vyšší úroveň přístupu je potřeba pro řešení potíží, které nejsou přeložit prostřednictvím výstrahy zprostředkování kroky, musíte spolupracovat s podpory. Prostřednictvím podpory je metoda zajistit dočasný správce s úplnými oprávněními přístup k systému provádět pokročilejší operace. 

## <a name="deploy-connected-or-disconnected"></a>Nasazení připojeno nebo odpojeno
 
Můžete nasadit Azure zásobníku buď připojené k Internetu (a do Azure) nebo odpojena. K získání využívat výhod z zásobník Azure, včetně hybridní scénáře mezi zásobník Azure a Azure, by chcete nasadit připojení k Azure. V následující tabulce pomáhá shrnout hlavní rozdíly mezi režimy nasazení.

| Oblast | Režimu připojení | Odpojeném režimu |
| -------- | ------------- | ----------|
| Zprostředkovatel identity | Azure Active Directory (Azure AD) nebo Active Directory Federation Services (AD FS) | Pouze služba AD FS. |
| Syndikace Marketplace. | Stažení položky přímo z Azure Marketplace na Marketplace s cílem v Azure zásobníku | Vyžaduje ruční správa v zásobníku Azure Marketplace |
| Model správy licencí | Platím jako--používání nebo na základě kapacity | Na základě kapacity pouze|
| Opravy a aktualizace  | Stáhnout balíky aktualizací přímo do Azure zásobníku | Vyžaduje vyměnitelné médium a samostatné připojeného zařízení |
| | | |

Nelze změnit režim nasazení bez opětovného nasazení úplnou později.

## <a name="identity-considerations"></a>Aspekty identity

### <a name="choose-identity-provider"></a>Vyberte poskytovatele správy identit

Musíte vzít v úvahu identity poskytovatele, kterého chcete použít pro nasazení zásobník Azure, Azure AD ani AD FS. Po nasazení bez opětovného nasazení úplnou nelze přepnout poskytovatelů identit.

Svou volbu zprostředkovatele identity nemá žádný vliv na klientské virtuální počítače, identitu systému a účty, které používají, zda bylo možné připojit doméně služby Active Directory atd. Toto je samostatný.

**Důvodů, proč zvažte použití služby Azure AD**

- Už máte stávajících investic ve službě Azure AD (např. Azure nebo Office 365).
- Chcete použít stejnou identitu napříč cloudy Azure a Azure zásobníku.
- Chcete zajistit podporu víceklientské scénáře, kde je možné hostovat různé organizace ve stejné instanci Azure zásobníku.
- Chcete použít správu na základě REST directory přes Azure AD Graph k zřizování uživatelů, skupin, atd. prostřednictvím rozhraní API.

> [!NOTE]
> Nasazení služby Azure zásobníku nemůže připojit k instanci služby Azure AD a existující instance služby AD FS ve stejnou dobu. Pokud nasadíte s Azure AD a chcete použít existující instanci služby AD FS, můžete vytvořit federaci místní instance služby AD FS se službou Azure AD.

**Důvodů, proč zvažte použití služby AD FS**

- Neexistuje žádné nebo pouze částečný připojení k Internetu.
- Nejsou zákonných nebo suverenity požadavky.
- Chcete použít vlastní identity systému (například vaše podnikové služby Active Directory) pro operátor a uživatelských účtů. K tomuto účelu můžete vytvořit federaci s existující instancí služby AD FS (v systému Windows Server 2012, 2012 R2 nebo 2016), který je zálohovaný službou Active Directory.
- Nemáte žádné investovali Azure a nechcete použít Azure AD.

> [!NOTE]
> Zásobník Azure může provést federaci pouze s jinou instanci služby AD FS, kterou je zajištěna služby Active Directory. Jiných zprostředkovatelů identity nejsou podporovány. Pokud máte jiných poskytovatelů identit, které chcete používat s Azure zásobníku, zvažte použití Azure nasazení na základě AD.

### <a name="ad-fs-and-graph-integration"></a>Integrace služby AD FS a graf

Pokud se rozhodnete nasadit zásobník Azure pomocí služby AD FS jako zprostředkovatele identity, musíte integrovat instanci služby AD FS v zásobníku Azure s existující instancí služby AD FS přes důvěryhodnost federace. To umožňuje identit v existující doménové struktury služby Active Directory k ověření s prostředky v Azure zásobníku.

Můžete také integrovat službu grafu v zásobníku Azure s existující služby Active Directory. To umožňuje spravovat na základě rolí řízení přístupu (RBAC) v Azure zásobníku. Když je delegovaný přístup k prostředku, vyhledá komponentu grafu uživatelský účet v existující doménové struktuře služby Active Directory pomocí protokolu LDAP.

Následující diagram znázorňuje integrované služby AD FS a graf tok přenosů.
![Diagram zobrazuje tok přenosů služby AD FS a graf](media/azure-stack-planning-considerations/ADFSIntegration.PNG)

## <a name="licensing-model"></a>Model správy licencí

Musíte rozhodnout, které licenčním modelu, který chcete použít. Pro připojené nasazení můžete platím jako--používání nebo na základě kapacity licencování. Platím jako--používání vyžaduje připojení k Azure sestavy využití, které se pak fakturuje prostřednictvím Azure commerce. Pouze na základě kapacity licencování je podporována, pokud nasazujete odpojený od Internetu. Další informace o licencování modelech najdete v tématu [Microsoft Azure zásobníku balení a ceny](https://azure.microsoft.com/mediahandler/files/resourcefiles/5bc3f30c-cd57-4513-989e-056325eb95e1/Azure-Stack-packaging-and-pricing-datasheet.pdf).

## <a name="naming-decisions"></a>Pojmenování rozhodnutí

Budete muset myslíte o tom, jak chcete naplánujte zásobník Azure obor názvů, zejména název oblasti a název externí domény. Plně kvalifikovaný název domény (FQDN) nasazení Azure zásobníku pro veřejné koncové body se rozumí kombinace těchto dvou názvy &lt; *oblast*&gt;&lt;*external_FQDN*  &gt;, například *east.cloud.fabrikam.com*. V tomto příkladu bude k dispozici na následující adresy URL portálů zásobník Azure:

- https://Portal.East.cloud.Fabrikam.com
- https://adminportal.East.cloud.Fabrikam.com

Následující tabulka shrnuje tato rozhodnutí pojmenování domény.

| Name (Název) | Popis | 
| -------- | ------------- | 
|Název oblasti | Název vaší první oblasti Azure zásobníku. Tento název se používá jako součást plně kvalifikovaný název domény pro veřejnou virtuální IP adresy (VIP) se spravuje zásobník Azure. Název oblasti obvykle, bude identifikátor fyzické umístění, jako je například umístění datového centra. | 
| Název externí domény | Název zóny, systému DNS (Domain Name) pro koncové body pomocí externího virtuální IP adresy. Použít ve plně kvalifikovaný název domény pro tyto veřejné virtuální IP adresy. | 
| Název domény privátní (interní) | Název domény (a interní DNS zóny) vytvořit v zásobníku Azure pro správu infrastruktury. 
| | |

## <a name="certificate-requirements"></a>Požadavky na certifikát

Pro nasazení budete potřebovat zajistit certifikáty, Secure Sockets Layer (SSL) pro veřejné koncové body. Certifikáty na vysoké úrovni, mají tyto požadavky:

> [!IMPORTANT]
> Informace o certifikátu v tomto článku je k dispozici pouze jako obecné pokyny. Než získáte všechny certifikáty pro Azure zásobníku, spolupracovat s partnerským hardwaru výrobce OEM. Zadá podrobnější pokyny certifikátu a požadavky.

- Můžete použít certifikát se zástupným znakem jednoho nebo můžete použít sadu vyhrazené certifikáty a použít zástupné znaky pouze pro koncové body, jako je například úložiště a Key Vault.
- Certifikáty musí být vydaný veřejnou důvěryhodné certifikační autority (CA) nebo spravované zákazníkem certifikační Autority.
 
V následující tabulce jsou služby a počet veřejné koncové body, které vyžadují certifikáty pro počáteční nasazení Azure zásobníku. 

| Použít pro | Koncový bod 
| -------- | ------------- | 
| Azure Resource Manager (správce) | adminmanagment. [Oblast]. [external_domain] |
| Azure Resource Manager (uživatel) | Správa. [Oblast]. [external_domain] |
| Portál (správce) | adminportal. [Oblast]. [external_domain] |
| Portál (uživatel) | portál. [Oblast]. [external_domain] |
| Key Vault (uživatel) | &#42;. trezor. [Oblast]. [external_domain] |
| Key Vault (správce) | &#42;. adminvault. [Oblast]. [external_domain] |
| Úložiště | &#42;. objekt BLOB. [Oblast]. [external_domain]<br>&#42;. Tabulka. [Oblast]. [external_domain]<br>&#42;. fronty. [Oblast]. [external_domain]  |
| Graf ** | graf. [Oblast]. [external_domain] |
| AD FS ** | Služba AD FS. [Oblast]. [external_domain] |
| | |

** Certifikáty pro koncové body grafu a služby AD FS jsou potřeba jenom pro nasazení služby AD FS.

Pokud chcete použít certifikát jeden zástupný znak, je třeba celkem šest alternativní názvy subjektu (SAN) pro počáteční nasazení Azure zásobníku. Tyto sítě SAN jsou: 

- &#42;. [Oblast]. [external_domain]
- &#42;. trezor. [Oblast]. [external_domain]
- &#42;. adminvault. [Oblast]. [external_domain]
- &#42;. objekt BLOB. [Oblast]. [external_domain]
- &#42;. Tabulka. [Oblast]. [external_domain] 
- &#42;. fronty. [Oblast]. [external_domain]

## <a name="time-synchronization"></a>Synchronizaci času

Čas serveru Azure zásobníku musí synchronizovat se serverem externí čas, který je možné přeložit pomocí IP adresy. Čas serveru v podnikové síti jsou vyžadovány pro odpojené nasazení.

## <a name="network-connectivity"></a>Připojení k síti

### <a name="dns-integration"></a>Integrace služby DNS

Překládat externí názvy DNS z Azure zásobníku (například www.bing.com), budete muset zadat serverů DNS, které můžete použít Azure zásobníku přesměrovat žádosti DNS, pro které není autoritativní zásobník Azure. Jako vstupy nasazení je nutné zadat alespoň dva servery, které chcete použít jako servery DNS pro předávání pro odolnost proti chybám.

K překladu názvů DNS Azure zásobníku koncových bodů z mimo Azure zásobníku (například z podnikové doménové struktury), musíte integrovat servery DNS, které hostitel externí zóny DNS pro Azure zásobníku se servery DNS, které jsou hostiteli nadřazenou zónu, kterou chcete použít. To vyžaduje překlad názvu DNS mezi zásobník Azure a existující zóny DNS v datovém centru. K tomu budete používat metody, jako je například podmíněné předávání nebo zónu delegování. Doporučujeme, abyste podmíněné předávání, pokud máte tohoto hostitele nadřazené zóně pro přímou kontrolu nad servery DNS pro Azure zásobníku externího oboru názvů DNS. (Pokud se vaše externí zóny DNS zásobníku se zobrazí jako podřízenou doménu s názvem vaší podnikové doméně (například azurestack.contoso.com a contoso.com), musíte použít delegování zóny místo.

### <a name="network-infrastructure"></a>Síťová infrastruktura

Síťové infrastruktury pro Azure zásobníku se skládá z několika logické sítě, které jsou nakonfigurované v přepínačích. Následující diagram znázorňuje tyto logické sítě a způsob jejich integraci s top-of-rack (TOR), řadiče pro správu základní desky (BMC) a ohraničení přepínače (síť zákazníka).

![Diagram a přepínače připojení logické sítě](media/azure-stack-planning-considerations/NetworkDiagram.png)

V následující tabulce jsou uvedeny logické sítě a přidružené podsítě rozsahy IPv4, které je nutné naplánovat.

| Logické sítě | Popis | Velikost | 
| -------- | ------------- | ------------ | 
| Veřejná virtuální IP adresy | Veřejné IP adresy pro malého služby Azure zásobníku se zbytkem používat klientské virtuální počítače. Infrastruktura Azure zásobníku používá 32 adres z této sítě. Pokud máte v úmyslu používat službu App Service a poskytovateli prostředků SQL, se používá více 7. | / 26 (62 hostitelé) – /22 (1022 hostitele)<br><br>Doporučená = /24 (254 hostitele) | 
| Přepínač infrastruktury | Point-to-Point IP adresy pro účely směrování, vyhrazené přepínače rozhraní pro správu a zpětné smyčky adresy přiřazené k přepínači. | /26 | 
| Infrastruktura | Používá se pro interní součásti zásobníku Azure ke komunikaci. | /24 |
| Privátní | Použít pro sítě úložiště a privátní virtuální IP adresy. | /24 | 
| BMC | Používá ke komunikaci s bmc na fyzických hostitelích. | /27 | 
| | | |

### <a name="uplink-to-border-switches"></a>Pro odesílání k přepínačům ohraničení

Budete potřebovat odesílání nakonfigurované tak, aby přepínače ohraničení ve vašem datovém centru. Je možné směrovat provoz této vrstvy 3 z přepínačů top-of-rack (TOR), které jsou součástí systému Azure zásobníku integrované některou z následujících metod:

- Border Gateway Protocol (BGP) 
- statické směrování

Doporučujeme protokolu BGP, protože umožňuje automatické aktualizace tras, které jsou publikovány softwaru zatížení vyrovnávání multiplexor (SLB MUX) pro externí sítě. To je důležité, pokud přidáte rozsahů veřejných IP adres po nasazení. Pokud tak učiníte z partnerského vztahu protokolu BGP TOR přepne do agregační přepínače, přepínače TOR připojeni k, veřejnou IP adresu rozsahy, přidejte po nasazení se automaticky inzerované agregační přepínač bez ručních zásahů. Pokud používáte statické směrování, je nutné ručně aktualizovat trasy pro nové rozsahy pokaždé, když přidáte veřejných blok IP adres.

### <a name="proxy-server"></a>Proxy server

Azure zásobníku podporuje pouze transparentní proxy servery. Transparentní proxy zachytí požadavky na síťovou vrstvou bez nutnosti konfigurace žádné speciální klienta.

### <a name="publish-azure-stack-services"></a>Publikování služby Azure zásobníku

Budete muset zpřístupnění služby Azure zásobníku uživatelům z mimo Azure zásobníku. Azure zásobníku nastaví různé koncové body pro její role infrastruktury. Tyto koncové body jsou přiřazeny virtuální IP adresy z fondu veřejných IP adres. Položka DNS se vytvoří pro každý koncový bod v externí zónu DNS, který byl zadán v době nasazení. Například portál user portal je přiřazen záznam hostitele DNS z "portálu. <*oblast*>. <*external_FQDN*>." 

#### <a name="ports-and-urls"></a>Porty a adresy URL

Aby služby Azure zásobníku (například portálů, Azure Resource Manager, DNS, atd.) k dispozici pro externí sítě, musíte povolit příchozí přenosy s těmito koncovými body pro konkrétní adresy URL, porty a protokoly.
 
V nasazení, kde transparentní proxy odesílání tradiční proxy serveru, musíte povolit určité porty a adresy URL pro odchozí komunikaci. Mezi ně patří porty a adresy URL pro identitu, syndikace marketplace, opravy a aktualizace, registrace a data o využití.

Další informace naleznete v tématu:
- [Příchozí porty a protokoly](https://docs.microsoft.com/azure/azure-stack/azure-stack-integrate-endpoints#ports-and-protocols-inbound)
- [Odchozí porty a adresy URL](https://docs.microsoft.com/azure/azure-stack/azure-stack-integrate-endpoints#ports-and-urls-outbound)

### <a name="connect-azure-stack-to-azure"></a>Připojit k Azure Azure zásobníku

U scénářů s hybridní cloudové budete muset naplánovat, jak se chcete připojit zásobník Azure do Azure. Existují dvě podporované metody pro připojení virtuálních sítí v Azure zásobníku virtuálních sítí v Azure: 
- **Site-to-site**. Připojení virtuální privátní sítě (VPN) prostřednictvím protokolu IPsec (IKE v1 a IKE v2). Tento typ připojení vyžaduje zařízení VPN nebo směrování a vzdálený přístup (RRAS). Další informace o branách VPN v Azure najdete v tématu [o službě VPN Gateway](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways). Komunikace přes tento tunel je šifrovaný a je bezpečné. Ale je omezena šířka pásma maximální propustnost tunelového propojení (100 – 200 MB/s).
- **Odchozí NAT**. Ve výchozím nastavení budou všechny virtuální počítače v Azure zásobníku mít připojení k externím sítím přes odchozí adres (NAT) Každý virtuální sítě, který je vytvořen v zásobníku Azure získá přiřazen veřejnou IP adresu. Ať už virtuální počítač je přímo přiřazenou veřejnou IP adresu, nebo je za službou Vyrovnávání zatížení s veřejnou IP adresu, bude mít odchozí přístup přes odchozí NAT pomocí virtuální IP adresu ve virtuální síti. Tento postup funguje pouze pro komunikaci, která se spouští virtuální počítač a je určený pro externí sítě (Internetu nebo intranetu). Nelze se používá ke komunikaci s virtuálním počítačem mimo.

#### <a name="hybrid-connectivity-options"></a>Možnosti hybridní připojení

Hybridní připojení je důležité vzít v úvahu, jaký typ nasazení, můžete chtít nabízet a kde bude nasazen. Budete muset zvážit, jestli chcete izolovat síťový provoz na každého klienta, a jestli budete mít k intranetu nebo Internetu nasazení.

- **Jednoho klienta Azure zásobníku**. Nasazení zásobník Azure, která vypadá, alespoň z hlediska sítě, pokud je jeden klienta. Může být, že mnoho klientů odběry, ale stejně jako služby intranetu, veškerý provoz přenáší přes stejné sítě. Síťový provoz z jednoho předplatného přenáší přes síťové připojení stejné jako jiné předplatné a nemusí být izolované přes šifrované tunelové propojení.

- **Zásobník Azure víceklientské**. Nasazení služby Azure zásobníku, kde musí být každého klienta předplatného provoz, který je vázaný sítě, které jsou externí vzhledem k Azure zásobníku izolované od ostatních klientů síťový provoz.
 
- **Nasazení v síti intranet**. Nasazení služby Azure zásobníku, která se nachází v podnikovém intranetu, obvykle na prostor privátní IP adresy a za jeden nebo více bran firewall. Veřejné IP adresy nejsou skutečně veřejné, protože nedá se trasovat přímo prostřednictvím veřejného Internetu.

- **Nasazení Internet**. Nasazení služby zásobník Azure, která je připojena k veřejnosti Internetu a používá směrovatelných v Internetu veřejné IP adresy pro veřejný rozsah virtuálních IP adres. Nasazení můžete stále nacházejí za bránou firewall, ale veřejné rozsah virtuálních IP adres je přímo dosažitelný z veřejného Internetu a Azure.
 
Následující tabulka shrnuje hybridní scénáře připojení s výhody, nevýhody a případy použití.

| Scénář | Metoda připojení | Odborníci na | Nevýhody | Je vhodný pro |
| -- | -- | --| -- | --|
| Jeden klient Azure zásobník nasazení v síti intranet | Odchozí NAT | Lepší šířku pásma pro přenosy na rychlejší. Jednoduchá implementace; nemá žádné brány vyžaduje. | Provoz nejsou šifrovaná; žádná izolace nebo šifrování nad rámec TOR. | Podnikové nasazení, kde jsou rovnoměrně důvěryhodné všichni klienti.<br><br>Podnikům, které mají okruh Azure ExpressRoute do Azure. |
| Víceklientské Azure zásobník nasazení v síti intranet | Site-to-site VPN | Je bezpečné provoz z virtuální sítě klienta do cílové. | Tunelové propojení VPN typu site-to-site je omezena šířka pásma.<br><br>Vyžaduje bránu virtuální sítě a zařízení v cílové síti VPN. | Musí být zabezpečená podnikového nasazení, kde některé přenosy dat klientů od ostatních klientů. |
| Jeden klient Azure zásobník internet nasazení | Odchozí NAT | Lepší šířku pásma pro přenosy na rychlejší. | Provoz nejsou šifrovaná; žádná izolace nebo šifrování nad rámec TOR. | Hostování scénáře, kde klient získá vlastní nasazení Azure zásobníku a vyhrazené okruhu do prostředí Azure zásobníku. Například ExpressRoute a Multiprotocol Label přepínání (MPLS).
| Víceklientské Azure zásobník internet nasazení | Site-to-site VPN | Je bezpečné provoz z virtuální sítě klienta do cílové. | Tunelové propojení VPN typu site-to-site je omezena šířka pásma.<br><br>Vyžaduje bránu virtuální sítě a zařízení v cílové síti VPN. | Scénáře, kde chce víceklientská Cloudová nabízejí poskytovatele hostování, kde není klienti důvěřovat navzájem a přenosy dat musí být zašifrován.
|  |  |  |  |  |

#### <a name="using-expressroute"></a>Pomocí služby ExpressRoute

Zásobník Azure můžete připojit k Azure přes [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) pro jednoho klienta intranetu i víceklientské scénáře. Budete potřebovat zřízené okruh ExpressRoute prostřednictvím [poskytovatele připojení](https://docs.microsoft.com/azure/expressroute/expressroute-locations).

Následující diagram znázorňuje ExpressRoute pro jednoho klienta scénář (kde "Zákazníka připojení" je okruh ExpressRoute).

![Diagram znázorňující jednoho klienta ExpressRoute scénář](media/azure-stack-planning-considerations/ExpressRouteSingleTenant.PNG)

Následující diagram znázorňuje ExpressRoute pro scénář více klientů.

![Diagram znázorňující víceklientské ExpressRoute scénář](media/azure-stack-planning-considerations/ExpressRouteMultiTenant.PNG)

## <a name="external-monitoring"></a>Externí monitorování
Získat jediné zobrazení všech výstrah ze zařízení a nasazení Azure zásobníku a integrovat do existujících IT služby pracovních postupů správy pro zpracování lístků výstrahy, kterou můžete integrovat zásobník Azure se externí datacenter sledování řešení.

Je součástí řešení Azure zásobníku, hostitele životního cyklu hardwaru počítače mimo Azure zásobníku, který spouští nástroje pro správu poskytuje výrobce OEM pro hardware. Můžete použít tyto nástroje nebo jiná řešení, které přímo integrovat existující řešení monitorování ve vašem datovém centru.

Následující tabulka shrnuje seznam aktuálně dostupných možností.

| Oblast | Externí řešení monitorování |
| -- | -- |
| Azure softwaru zásobníku | - [Azure zásobníku sady Management Pack pro nástroj Operations Manager](https://azure.microsoft.com/blog/management-pack-for-microsoft-azure-stack-now-available/)<br>- [Modul plug-in Nagios](https://exchange.nagios.org/directory/Plugins/Cloud/Monitoring-AzureStack-Alerts/details)<br>-Založené na REST API – volání | 
| Fyzické servery (přes rozhraní IPMI BMC) | -Sada management pack dodavatele operations Manager<br>-OEM řešení poskytuje dodavatel hardwaru<br>-Dodavatele hardwaru Nagios moduly plug-in | Výrobce OEM partnera podporováno monitorování řešení (součást) | 
| Síťová zařízení (SNMP) | -Zjišťování síťových zařízení operations Manager<br>-OEM řešení poskytuje dodavatel hardwaru<br>– Modul plug-in Nagios přepínač |
| Sledování stavu předplatného klienta | - [Sada System Center Management Pack pro systém Windows Azure](https://www.microsoft.com/download/details.aspx?id=50013) | 
|  |  | 

Vezměte na vědomí následující požadavky:
- Řešení, které používáte, musí být bez agenta. Nelze instalovat agenty třetích stran uvnitř součásti zásobníku Azure. 
- Pokud chcete použít System Center Operations Manager, vyžaduje Operations Manager 2012 R2 nebo Operations Manager 2016.

## <a name="backup-and-disaster-recovery"></a>Zálohování a zotavení po havárii

Plánování zálohování a zotavení po havárii zahrnuje plánování pro obě základní zásobník Azure infrastrukturu, která hostuje virtuální počítače IaaS a PaaS služby a pro klienta aplikace a data. Musíte naplánovat pro tyto samostatně.

### <a name="protect-infrastructure-components"></a>Chránit součásti infrastruktury

Azure zásobníku zálohuje součásti infrastruktury do sdílené složky, který určíte.

- Budete potřebovat externí sdílené složce SMB na existující server systému Windows soubor nebo zařízení třetích stran.
- Měli byste použít této stejné sdílené složce pro zálohování síťové přepínače a životního cyklu hostitele hardwaru. Dodavatele hardwaru, od výrobců OEM pomůže poskytnout pokyny pro zálohování a obnovení všechny tyto komponenty jsou tyto externí do protokolů Azure. Jste zodpovědná za spuštění zálohování pracovních postupů na základě doporučení výrobce OEM.

Pokud dojde k závažné ztráty dat, můžete použít infrastrukturu zálohy dat nasazení opakovaným přidáním jako certifikát kořenové certifikační Autority nasazení pro vstupy a identifikátory, účty služby, federované prostředky (v odpojených nasazení), plánů, nabídky odběry, kvóty, RBAC roli zásady a přiřazení a tajné klíče Key Vault.
 
### <a name="protect-tenant-applications-on-iaas-virtual-machines"></a>Chránit aplikace od klienta na virtuální počítače IaaS

Azure zásobníku nezálohuje až klienta aplikace a data. Je nutné naplánovat zálohování a po havárii obnovení ochrany na cíl, externí do protokolů Azure. Ochranu klienta je aktivita řízené klienta. Pro virtuální počítače IaaS můžete klientů používat technologie v hosta k ochraně souborů, složek, data aplikací a stavu systému. Jako poskytovatel enterprise nebo službě, můžete však nabízí řešení zálohování a obnovení ve stejném datovém centru nebo externě v cloudu.

Zálohování virtuálních počítačů Linux nebo IaaS ve Windows, musíte použít zálohování produktů s přístupem k hostovaného operačního systému k ochraně souboru, složky, stav operačního systému a dat aplikací. Zálohování Azure, System Center Data Center Protection Manager, můžete použít nebo podporovány produkty třetích stran.

Pokud chcete replikovat data do sekundárního umístění a orchestraci převzetí služeb při selhání aplikace, pokud dojde k havárii, můžete použít Azure Site Recovery nebo podporované produkty třetích stran. (Na počáteční verze integrované systémy Azure Site Recovery nebudou podporovat navrácení služeb po obnovení. Však můžete dosáhnout navrácení služeb po obnovení ruční proces.) Aplikace, které podporují nativní replikace (jako je Microsoft SQL Server) navíc můžete replikovat data do jiného umístění, kde je aplikace spuštěna.

> [!IMPORTANT]
> V původním vydáním integrované systémy podporujeme ochrany technologie, které fungují na úrovni hosta virtuálního počítače IaaS. Nelze instalovat agenty na základní servery infrastruktury.

## <a name="next-steps"></a>Další kroky

- Informace o případy použití, nákup, partneři a OEM výrobci hardwaru najdete v tématu [zásobník Azure](https://azure.microsoft.com/overview/azure-stack/) stránky produktu.
- Informace o plán a geografická dostupnosti pro zásobník Azure integrované systémy, najdete v dokumentu white paper: [Azure zásobník: rozšíření Azure](https://azure.microsoft.com/resources/azure-stack-an-extension-of-azure/). 
