---
title: "Aspekty plánování pro Azure zásobníku integrované systémy | Microsoft Docs"
description: "Zjistěte, co můžete dělat teď plánování a příprava pro více uzly Azure zásobníku."
services: azure-stack
documentationcenter: 
author: jeffgilb
manager: femila
editor: 
ms.assetid: 
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/16/2018
ms.author: jeffgilb
ms.reviewer: wfayed
ms.openlocfilehash: cd14f0e5259e5c0b6cbf11790bbdf08164267ffa
ms.sourcegitcommit: 2a70752d0987585d480f374c3e2dba0cd5097880
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/19/2018
---
# <a name="planning-considerations-for-azure-stack-integrated-systems"></a>Aspekty plánování pro Azure zásobníku integrované systémy
Pokud byste chtěli v systému Azure zásobníku integrované, byste měli porozumět některé hlavní informace o plánování řešení nasazení a jak systému zapadá do vašeho datového centra. Tento článek obsahuje přehled těchto aspektů, který vám pomůže provádět rozhodnutí důležité infrastruktury pro systém Azure zásobníku více uzly. Pochopení těchto aspektů pomáhá při práci s dodavatele hardwaru, od výrobců OEM jako jejich nasazení do vašeho datového centra Azure zásobníku.  

> [!NOTE]
> Azure systémech s více uzly zásobníku lze pouze zakoupit od dodavatelů povolený hardware. 

K nasazení zásobník Azure existuje sada rozhodnutí, která budete muset udělat správně integrovat s vaším prostředím Azure zásobníku. Budete muset při plánování zadejte tyto informace do poskytovatele řešení a aby bylo připravené pro dodavatele hardwaru, před zahájením nasazení pomohou proces přejděte rychle a bez problémů.

Informace požadované rozsahy napříč sítí, zabezpečení a informace o identitě s mnoha důležitých rozhodnutí, které mohou vyžadovat znalost z mnoha různých oblastech a vedoucím pracovníkům. Proto můžete chtít stáhnout osoby z více týmy ve vaší organizaci k zajištění, že máte všechny požadované informace, které jsou připravené před zahájením nasazení. Obraťte se na dodavatele hardwaru, při shromažďování těchto informací, protože mohou mít užitečné vaše rozhodování Rady, jak může pomoct.

Analýza a shromažďování požadované informace, možná budete muset udělat nějaké změny konfigurace před nasazením do prostředí vaší sítě. To může zahrnovat rezervování adresní prostory IP adres pro řešení Azure zásobníku a konfigurace směrovače, přepínače a brány firewall, aby příprava pro připojení k nové řešení přepínače zásobník Azure. Ujistěte se, že máte expert oblasti subjektu srovnáte a zahrnuta v rámci projektu nasazení vám pomohou při plánování.

## <a name="management-considerations"></a>Aspekty správy
Azure zásobník je zapečetěné systému, kde infrastruktury je uzamčené z oprávnění i a síťový perspektivy. Sítě seznamy řízení přístupu (ACL) se použijí pro blokovat veškerý příchozí provoz neoprávněným a všechny zbytečné komunikace mezi součástmi infrastruktury. Díky tomu je obtížné neoprávněným uživatelům přístup k systému.

Pro každodenní správu a operace neexistuje žádný správce neomezený přístup k infrastruktuře. Operátory Azure zásobníku musí spravovat systém prostřednictvím portálu správce nebo prostřednictvím Správce Azure Resource Manager (přes rozhraní příkazového řádku Azure, PowerShell nebo rozhraní REST API). Není k dispozici přístup k systému pomocí jiné nástroje pro správu, například Správce technologie Hyper-V nebo Správce clusteru převzetí služeb při selhání. K ochraně systému, nelze nainstalovat software jiných výrobců (například agenty) uvnitř součástí infrastruktury Azure zásobníku. Vzájemná funkční spolupráce s externí správu a zabezpečení softwaru dojde prostřednictvím rozhraní příkazového řádku Azure, PowerShell nebo rozhraní REST API.

Vyšší úroveň přístupu je potřeba pro řešení potíží, které nejsou přeložit prostřednictvím výstrahy zprostředkování kroky, musíte spolupracovat s podpory. Prostřednictvím podpory je metoda zajistit dočasný správce s úplnými oprávněními přístup k systému provádět pokročilejší operace. 

## <a name="identity-considerations"></a>Aspekty identity

### <a name="choose-identity-provider"></a>Vyberte poskytovatele správy identit
Musíte vzít v úvahu identity poskytovatele, kterého chcete použít pro nasazení zásobník Azure, Azure AD ani AD FS. Po nasazení bez opětovného nasazení úplnou nelze přepnout poskytovatelů identit.

Svou volbu zprostředkovatele identity nemá žádný vliv na klientské virtuální počítače, identitu systému a účty, které používají, zda bylo možné připojit doméně služby Active Directory atd. Toto je samostatný.

Další informace o výběru zprostředkovatele identity v [rozhodnutí o nasazení pro Azure zásobníku integrované systémy článek](.\azure-stack-deployment-decisions.md).

### <a name="ad-fs-and-graph-integration"></a>Integrace služby AD FS a graf
Pokud se rozhodnete nasadit zásobník Azure pomocí služby AD FS jako zprostředkovatele identity, musíte integrovat instanci služby AD FS v zásobníku Azure s existující instancí služby AD FS přes důvěryhodnost federace. To umožňuje identit v existující doménové struktury služby Active Directory k ověření s prostředky v Azure zásobníku.

Můžete také integrovat službu grafu v zásobníku Azure s existující služby Active Directory. To umožňuje spravovat na základě rolí řízení přístupu (RBAC) v Azure zásobníku. Když je delegovaný přístup k prostředku, vyhledá komponentu grafu uživatelský účet v existující doménové struktuře služby Active Directory pomocí protokolu LDAP.

Následující diagram znázorňuje integrované služby AD FS a graf tok přenosů.
![Diagram zobrazuje tok přenosů služby AD FS a graf](media/azure-stack-deployment-planning/ADFSIntegration.PNG)

## <a name="licensing-model"></a>Model správy licencí

Musíte rozhodnout, které licenčním modelu, který chcete použít. Pro připojené nasazení můžete platím jako--používání nebo na základě kapacity licencování. Platím jako--používání vyžaduje připojení k Azure sestavy využití, které se pak fakturuje prostřednictvím Azure commerce. Pouze na základě kapacity licencování je podporována, pokud nasazujete odpojený od Internetu. Další informace o licencování modelech najdete v tématu [Microsoft Azure zásobníku balení a ceny](https://azure.microsoft.com/mediahandler/files/resourcefiles/5bc3f30c-cd57-4513-989e-056325eb95e1/Azure-Stack-packaging-and-pricing-datasheet.pdf).

## <a name="naming-decisions"></a>Pojmenování rozhodnutí

Budete muset myslíte o tom, jak chcete naplánujte zásobník Azure obor názvů, zejména název oblasti a název externí domény. Plně kvalifikovaný název domény (FQDN) nasazení Azure zásobníku pro veřejné koncové body se rozumí kombinace těchto dvou názvy &lt; *oblast*&gt;&lt;*external_FQDN*  &gt;, například *east.cloud.fabrikam.com*. V tomto příkladu bude k dispozici na následující adresy URL portálů zásobník Azure:

- https://portal.east.cloud.fabrikam.com
- https://adminportal.east.cloud.fabrikam.com

Následující tabulka shrnuje tato rozhodnutí pojmenování domény.

| Název | Popis | 
| -------- | ------------- | 
|Název oblasti | Název vaší první oblasti Azure zásobníku. Tento název se používá jako součást plně kvalifikovaný název domény pro veřejnou virtuální IP adresy (VIP) se spravuje zásobník Azure. Název oblasti obvykle, bude identifikátor fyzické umístění, jako je například umístění datového centra. | 
| Název externí domény | Název zóny, systému DNS (Domain Name) pro koncové body pomocí externího virtuální IP adresy. Použít ve plně kvalifikovaný název domény pro tyto veřejné virtuální IP adresy. | 
| Název domény privátní (interní) | Název domény (a interní DNS zóny) vytvořit v zásobníku Azure pro správu infrastruktury. 
| | |

## <a name="certificate-requirements"></a>Požadavky na certifikát

Pro nasazení budete potřebovat zajistit certifikáty, Secure Sockets Layer (SSL) pro veřejné koncové body. Certifikáty na vysoké úrovni, mají tyto požadavky:

- Můžete použít certifikát se zástupným znakem jednoho nebo můžete použít sadu vyhrazené certifikáty a použít zástupné znaky pouze pro koncové body, jako je například úložiště a Key Vault.
- Certifikáty, může být vydaný veřejnou důvěryhodné certifikační autority (CA) nebo spravované zákazníkem certifikační Autority.

Další informace o jaké infrastruktury veřejných KLÍČŮ jsou certifikáty potřebné k nasazení zásobník Azure a získat je najdete v tématu, [požadavky na certifikáty infrastruktury veřejných klíčů Azure zásobníku](azure-stack-pki-certs.md).  


> [!IMPORTANT]
> Zadané informace o certifikátu PKI slouží jako obecné pokyny. Než získáte všechny certifikáty infrastruktury veřejných KLÍČŮ pro Azure zásobníku, spolupracovat s partnerským hardwaru výrobce OEM. Zadá podrobnější pokyny certifikátu a požadavky.



## <a name="time-synchronization"></a>Synchronizaci času
Musíte vybrat určitý čas, který server s se používá k synchronizaci Azure zásobníku.  Synchronizaci času je velmi důležité zásobník Azure a její role infrastruktury, jako se používá ke generování lístky protokolu Kerberos, které se používají k ověření interních služeb mezi sebou.

Je nutné zadat IP adresy pro server synchronizace času, protože i když většina komponenty v infrastruktuře můžete vyřešit adresu URL, některá podporují pouze IP adresy. Pokud jste se používá možnost odpojené nasazení, je nutné zadat čas serveru ve vaší podnikové síti, které jste opravdu dosažitelný z infrastruktury sítě v Azure zásobníku. Může to vyžadovat zvážit při plánování sítě integrace část projektu nasazení.


## <a name="network-connectivity"></a>Připojení k síti
Tato část obsahuje zásobník Azure síťové infrastruktury informace, které pomůže vám důležité rozhodnutí o tom, jak nejlépe zásobník Azure integrovat do stávajícího síťového prostředí. 

> [!NOTE]
> Překládat externí názvy DNS z Azure zásobníku (například www.bing.com), budete muset zadat serverů DNS, které můžete použít Azure zásobníku přesměrovat žádosti DNS, pro které není autoritativní zásobník Azure. Další informace o Azure zásobníku DNS najdete v části požadavky, [zásobník Azure datacenter integrace - DNS](azure-stack-integrate-dns.md).

### <a name="physical-network-design"></a>Návrh fyzické sítě
Řešení Azure zásobníku vyžaduje odolné a vysoce dostupné fyzické infrastruktury pro podporu jeho operace a služeb. Dole je diagram naše doporučený návrh:

![Doporučený návrh sítě Azure zásobníku](media/azure-stack-deployment-planning/recommended-design.png)


### <a name="logical-networks"></a>Logické sítě
Logické sítě představují abstrakci podkladové fyzické síťové infrastruktury. Používají se k organizaci a zjednodušení přiřazení v síti pro hostitele, virtuální počítače a služby. V rámci vytváření logické sítě síťové lokality nejsou vytvářeny zadat virtuální místní sítě (VLAN), podsítě IP a páry IP podsíť či síť VLAN, které jsou přidružené logické sítě v každém fyzické umístění.
Síťové infrastruktury pro zásobník Azure používá následující logické sítě, které jsou nakonfigurované v přepínačích:

### <a name="network-infrastructure"></a>Síťová infrastruktura
Síťové infrastruktury pro Azure zásobníku se skládá z několika logické sítě, které jsou nakonfigurované v přepínačích. Následující diagram znázorňuje tyto logické sítě a způsob jejich integraci s top-of-rack (TOR), řadiče pro správu základní desky (BMC) a ohraničení přepínače (síť zákazníka).

![Diagram a přepínače připojení logické sítě](media/azure-stack-deployment-planning/NetworkDiagram.png)

#### <a name="bmc-network"></a>BMC sítě
Tato síť je vyhrazen až po připojení všech řadiči pro správu (také označované jako služby procesory, například iDRAC, iLO, iBMC, atd.) pro síť pro správu. Pokud je k dispozici, hostitele životního cyklu hardwaru (HLH) budou umístěny v této síti a může poskytnout určitým softwarem OEM pro údržbu hardwaru nebo monitorování. 

#### <a name="private-network"></a>Privátní síť
Tato /24 (254 hostitele IP adresy) sítě je privátní oblast Azure zásobníku (nerozšiřuje nad rámec zařízení přepínač ohraničení oblasti Azure zásobníku) a je rozdělená do dvou podsítí:

- **Síť úložiště**. /25 (126 hostitele IP adresy) sítě použitá k podpoře použití prostory úložiště – přímé (S2D) a přenosy dat úložiště serveru SMB (Message Block) a migrace za provozu virtuálního počítače. 
- **Interní virtuální síť IP**. A/25 síti vyhrazený pro interní jen VIP pro vyrovnávání zatížení softwaru (SLB).

#### <a name="azure-stack-infrastructure-network"></a>Síť infrastruktury Azure zásobníku
To/24 síť je vyhrazená pro interní součásti zásobníku Azure, aby se mohli komunikovat a výměnu dat mezi sebou. Tato podsíť vyžaduje směrovatelné IP adresy, ale se ukládají privátní do řešení pomocí seznamů řízení přístupu (ACL).  Neočekává směrování nad rámec přepínače ohraničení s výjimkou velmi malé rozsah ekvivalentní velikost/27 síť, což je využít pomocí některé z těchto služeb v případě, které vyžadují přístup k externím prostředkům nebo k Internetu. 

#### <a name="public-infrastructure-network"></a>Infrastruktura veřejných sítě
To/27 síť je velmi malé rozsahu podsítě infrastruktury Azure zásobníku, již bylo zmíněno dříve, nevyžaduje veřejné IP adresy, ale vyžaduje přístup k Internetu prostřednictvím NAT nebo transparentní proxy server. Tato síť se přidělí pro nouzové obnovení konzoly System (ERCS). Virtuální počítač ERCS během registrace do Azure vyžaduje přístup k Internetu a musí být směrovatelné vaší síti pro správu pro účely odstraňování potíží.

#### <a name="public-vip-network"></a>Veřejné síti VIP
Veřejné síti VIP je přiřazen do síťového adaptéru v zásobníku Azure. Není logická síť na přepínači. SLB pomocí fondu adres a přiřadí/32 sítě pro úlohy klientů. V tabulce směrování přepínače jsou inzerovaný těchto 32 IP adres jako trasu k dispozici prostřednictvím protokolu BGP. Tato síť obsahuje externí dostupný nebo veřejné IP adresy. Infrastruktura Azure zásobníku používá aspoň 8 adres z této veřejné síti VIP, zatímco zbytek používá klientské virtuální počítače. Velikost sítě v této podsíti může být v rozsahu od minimálně /26 (64 hostitele) do maximálně /22 (1022 hostitele), doporučujeme, abyste naplánovali pro/24 sítě.

#### <a name="switch-infrastructure-network"></a>Přepínač síti infrastruktury.
To, 26 sítě je podsítě, která obsahuje podsítě/30 IP směrovatelné point-to-point (2 hostitele IP na) a vytváření, které jsou vyhrazené zpětných smyček nebo přepínač 32 podsítě pro integrované správy a ID směrovače protokolu BGP Tento rozsah IP adres musí být směrovatelné externě z řešení Azure zásobníku vašeho datového centra a mohou být privátní nebo veřejné IP adresy. V rámci nevyužívá dělené tabulky více scénář poskytovatele služeb, může být povinné, ale v rámci přísně řízená podnikové nasazení veřejné IP adresy, privátních IP adres může být například upřednostňované.

#### <a name="switch-management-network"></a>Síť pro správu přepínače
Tato /29 (6 hostitele IP adresy) sítě je vyhrazen pro správu porty přepínačů připojení. To umožňuje přístup out-of-band pro nasazení, správu a řešení potíží. Vypočítá ze sítě infrastruktury přepínač uvedených výše.

### <a name="transparent-proxy"></a>TRANSPARENTNÍ SERVER PROXY
Řešení Azure zásobník nepodporuje normální webové proxy servery. Pokud datacentru vyžaduje, aby veškerý provoz do používat proxy server, musíte nakonfigurovat transparentní proxy zpracovat veškerý provoz z racku pro zpracování podle zásad oddělení provozu mezi zóny ve vaší síti. Transparentní proxy (také označované jako zachycení, vložené nebo vynucené proxy) zachycuje normální komunikace síťové vrstvy bez nutnosti konfigurace žádné speciální klienta. Klienti nemusí být vědomi existenci proxy serveru.

### <a name="publish-azure-stack-services"></a>Publikování služby Azure zásobníku

Budete muset zpřístupnění služby Azure zásobníku uživatelům z mimo Azure zásobníku. Azure zásobníku nastaví různé koncové body pro její role infrastruktury. Tyto koncové body jsou přiřazeny virtuální IP adresy z fondu veřejných IP adres. Položka DNS se vytvoří pro každý koncový bod v externí zónu DNS, který byl zadán v době nasazení. Například portál user portal je přiřazen záznam hostitele DNS portálu.  *&lt;oblast >.&lt; plně kvalifikovaný název domény >*.

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

![Diagram znázorňující jednoho klienta ExpressRoute scénář](media/azure-stack-deployment-planning/ExpressRouteSingleTenant.PNG)

Následující diagram znázorňuje ExpressRoute pro scénář více klientů.

![Diagram znázorňující víceklientské ExpressRoute scénář](media/azure-stack-deployment-planning/ExpressRouteMultiTenant.PNG)

## <a name="external-monitoring"></a>Externí monitorování
Získat jediné zobrazení všech výstrah ze zařízení a nasazení Azure zásobníku a integrovat do existujících IT služby pracovních postupů správy pro zpracování lístků výstrahy, kterou můžete integrovat zásobník Azure se externí datacenter sledování řešení.

Je součástí řešení Azure zásobníku, hostitele životního cyklu hardwaru (HLH) počítače mimo Azure zásobníku, který spouští nástroje pro správu poskytuje výrobce OEM pro hardware. Můžete použít tyto nástroje nebo jiná řešení, které přímo integrovat existující řešení monitorování ve vašem datovém centru.

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
