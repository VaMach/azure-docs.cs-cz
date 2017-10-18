---
title: "Výstrahy zabezpečení podle typu ve službě Azure Security Center | Dokumentace Microsoftu"
description: "Tento článek popisuje různé druhy výstrah zabezpečení dostupných ve službě Azure Security Center."
services: security-center
documentationcenter: na
author: YuriDio
manager: mbaldwin
editor: 
ms.assetid: b3e7b4bc-5ee0-4280-ad78-f49998675af1
ms.service: security-center
ms.topic: hero-article
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/20/2017
ms.author: yurid
ms.openlocfilehash: 274c50dad9b8a1d79a71a29b04cb8e44ad91893c
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="understanding-security-alerts-in-azure-security-center"></a>Principy výstrah zabezpečení ve službě Azure Security Center
Tento článek vám pomůže porozumět různým typům výstrah zabezpečení a souvisejícím přehledům, které jsou dostupné ve službě Azure Security Center. Další informace o správě těchto výstrah a incidentů najdete v tématu [Správa a zpracování výstrah zabezpečení ve službě Azure Security Center](security-center-managing-and-responding-alerts.md).

Pokud chcete nastavit rozšířené detekce, upgradujte na Azure Security Center Standard. K dispozici je bezplatná 60denní zkušební verze. Pokud chcete provést upgrade, v [zásadách zabezpečení](security-center-policies.md) vyberte **cenovou úroveň**. Další informace najdete na [stránce s cenami](https://azure.microsoft.com/pricing/details/security-center/).

> [!NOTE]
> Ve službě Security Center byla vydána ve verzi Limited Preview nová sada detekcí, které využívají záznamy auditu, což je běžný auditní rámec, k detekci škodlivého chování na počítačích s Linuxem. Pokud se chcete připojit k verzi Preview, zašlete [nám](mailto:ASC_linuxdetections@microsoft.com) e-mail s ID vašich předplatných.

## <a name="what-type-of-alerts-are-available"></a>Jaké typy výstrah jsou k dispozici?
Azure Security Center používá celou řadu různých [možností detekce](security-center-detection-capabilities.md) k upozornění zákazníků na potenciální útoky namířené proti jejich prostředím. Tyto výstrahy obsahují cenné informace o tom, co výstrahu aktivovalo, o prostředcích na které cílí, a o zdroji útoku. Informace obsažené ve výstrahách se liší podle typu analýzy použité k rozpoznání příslušné hrozby. Incidenty mohou obsahovat také další kontextové informace, které mohou být užitečné při dalším zkoumání hrozeb.  Tento článek obsahuje informace o následujících typech výstrah:

* Analýza chování virtuálního počítače (VMBA)
* Analýza sítě
* Analýza prostředků
* Kontextové informace

## <a name="virtual-machine-behavioral-analysis"></a>Analýza chování virtuálního počítače
Azure Security Center může pomocí analýzy chování identifikovat ohrožené prostředky na základě analýzy protokolů událostí virtuálního počítače. Například události vytváření procesů a události přihlášení. Kromě toho se pomocí vzájemné souvislosti s dalšími signály hledají podpůrné důkazy rozšířené kampaně.

> [!NOTE]
> Další informace o tom, jak detekce služby Security Center pracuje, najdete v článku [Funkce detekce ve službě Azure Security Center](security-center-detection-capabilities.md).
>

### <a name="crash-analysis"></a>Analýza stavu systému
Metoda analýzy paměti ve výpisu stavu systému slouží ke zjištění sofistikovaného malwaru, který je schopný se vyhnout běžným řešením zabezpečení. Různé formy malwaru se snaží snížit pravděpodobnost svého zjištění antivirovými produkty tím, že nikdy nezapisují na disk nebo softwarové komponenty ukládané na disk šifruje. Kvůli této technice je malware jen obtížně zjistitelný pomocí obvyklých antimalwarových postupů. Tento typ malwaru lze ale zjistit pomocí analýzy paměti, protože aby malware mohl fungovat, musí v paměti zanechat stopy.

Pokud dojde k chybě softwaru, ve výpisu stavu systému se zaznamená část paměti v době selhání. Taková chyba může být způsobena malwarem, běžnou aplikací nebo systémovými problémem. Díky analýze paměti ve výpisu stavu systému dokáže služba Security Center zjišťovat techniky, které využívají zranitelností softwaru, získávají přístup k důvěrným datům a nenápadně přetrvávají v napadeném počítači. Díky tomu, že se tato analýza provádí v back-endu služby Security Center, má minimální dopad na výkon hostitele.

Následující pole jsou společná pro příklady výstrah na stav systému, které se objevují dále v tomto článku:

* DUMPFILE: Název souboru s výpisem stavu systému.
* PROCESSNAME: Název procesu, ve kterém došlo k chybě.
* PROCESSVERSION: Verze procesu, ve kterém došlo k chybě.

### <a name="shellcode-discovered"></a>Zjištěn skrytý spustitelný kód
Skrytý spustitelný kód je datová část, která se spouští potom, co malware zneužije chybu zabezpečení softwaru. Tato výstraha znamená, že při analýze výpisu stavu systému byl nalezen spustitelný kód, který vykazuje chování typické pro škodlivý software. Někdy se může takovým způsobem chovat i software bez zlých úmyslů, pro běžný vývoj softwaru ale toto chování není typické.

Příklad výstrahy na skrytý spustitelný kód navíc obsahuje následující pole:

* ADDRESS: Umístění skrytého spustitelného kódu v paměti.

Tady je příklad tohoto typu výstrahy:

![Výstraha na skrytý spustitelný kód](./media/security-center-alerts-type/security-center-alerts-type-fig2.png)

### <a name="module-hijacking-discovered"></a>Zjištěno napadení modulu
Systém Windows umožňuje softwaru využívat základní systémové funkce prostřednictvím knihoven DLL (Dynamic Link Libraries). Při napadení knihoven DLL malware pozmění pořadí načítání knihoven DLL tak, aby umožnil načtení škodlivých datových částí do paměti, ze které je pak bude možné spustit. Tato výstraha znamená, že při analýze výpisu stavu systému byly nalezeny dva moduly s podobným názvem, ale načítané z různých cest. Jedna z načtených cest odpovídá běžnému umístění binárních souborů systému Windows.

Vývojáři legitimního softwaru někdy mění pořadí načítání knihoven DLL bez zlých úmyslů, například při instrumentaci, rozšiřování operačního systému Windows nebo rozšiřování aplikací. Azure Security Center rozlišuje škodlivé změny pořadí načítání knihoven DLL od pravděpodobně bezpečných změn na základě profilů podezřelého chování. Výsledek kontroly je u výstrahy uveden v poli SIGNATURE a určuje závažnost výstrahy, popis výstrahy a nápravné kroky. Chcete-li zjistit, jestli je modul legitimní nebo škodlivý, proveďte analýzu kopie napadeného modulu uložené na disku. Například můžete ověřit digitální podpis souboru nebo spustit antivirovou kontrolu.

Kromě společných polí popsaných výše v části „Zjištěn skrytý spustitelný kód“ tato výstraha obsahuje také následující pole:

* SIGNATURE: Udává, jestli napadený modul odpovídá profilu podezřelého chování.
* HIJACKEDMODULE: Název napadeného modulu systému Windows.
* HIJACKEDMODULEPATH: Cesta k napadenému modulu systému Windows.
* HIJACKINGMODULEPATH: Cesta k napadajícímu modulu.

Tady je příklad tohoto typu výstrahy:

![Výstraha na napadení modulu](./media/security-center-alerts-type/security-center-alerts-type-fig3.png)

### <a name="masquerading-windows-module-detected"></a>Zjištěno maskování za modul systému Windows
Malware může používat běžné názvy spustitelných souborů (např. SVCHOST.EXE) nebo modulů (např. NTDLL.DLL) systému Windows, aby se *vmísil mezi ostatní* a skryl svou škodlivou povahu před správci systému. Tato výstraha znamená, že při analýze výpisu stavu systému byly nalezeny moduly, které svým názvem odpovídají systémovým modulům Windows, ale nesplňují další kritéria typická pro systémové moduly. Další informace o povaze maskovaného modulu může poskytnout analýza kopie modulu na disku. Součástí analýzy může být:

* Potvrzení, že je příslušný soubor součástí balíčku legitimního softwaru.
* Ověření digitálního podpisu souboru.
* Antivirová kontrola souboru.

Kromě společných polí popsaných výše v části „Zjištěn skrytý spustitelný kód“ tato výstraha obsahuje také následující pole:

* DETAILS: Udává, jestli jsou metadata modulu platná a jestli byl modul načten ze systémové cesty.
* NAME: Název maskovaného modulu systému Windows.
* PATH: Cesta k maskovanému modulu systému Windows.

Tato výstraha také extrahuje a zobrazuje určitá pole ze záhlaví PE modulu, např. pole CHECKSUM a TIMESTAMP. Tato pole se zobrazí, pouze pokud se v modulu nacházejí. Podrobnosti o těchto polích naleznete ve [specifikaci formátů Microsoft PE a COFF](https://msdn.microsoft.com/windows/hardware/gg463119.aspx).

Tady je příklad tohoto typu výstrahy:

![Výstraha na maskování za modul systému Windows](./media/security-center-alerts-type/security-center-alerts-type-fig4.png)

### <a name="modified-system-binary-discovered"></a>Zjištěna úprava binárního systémového souboru
Malware může upravit binární systémové soubory, aby mohl nepozorovaně přistupovat k datům nebo skrytě přetrvávat v napadeném systému. Tato výstraha znamená, že při analýze stavu systému byly v paměti nebo na disku nalezeny upravené binární soubory jádra systému Windows.

Vývojáři legitimního softwaru někdy upravují systémové soubory v paměti bez zlých úmyslů, například soubory balíčku Detours pro zajištění kompatibility aplikací. Azure Security Center rozlišuje škodlivé moduly od pravděpodobně bezpečných modulů na základě profilů podezřelého chování. Výsledek kontroly určuje závažnost výstrahy, popis výstrahy a nápravné kroky.

Kromě společných polí popsaných výše v části „Zjištěn skrytý spustitelný kód“ tato výstraha obsahuje také následující pole:

* MODULENAME: Název upraveného binárního systémového souboru.
* MODULEVERSION: Verze upraveného binárního systémového souboru.

Tady je příklad tohoto typu výstrahy:

![Výstraha na upravený binární systémový soubor](./media/security-center-alerts-type/security-center-alerts-type-fig5.png)

### <a name="suspicious-process-executed"></a>Spuštění podezřelého procesu
Security Center identifikuje spouštění podezřelého procesu na cílovém virtuálním počítači a potom aktivuje výstrahu. Detekce nevyhledává konkrétní název procesu, ale parametry spustitelného souboru. Proto je služba Security Center schopna zjistit spustitelný soubor i v případě, že jej útočník přejmenuje.

Tady je příklad tohoto typu výstrahy:

![Výstraha na spuštění podezřelého procesu](./media/security-center-alerts-type/security-center-alerts-type-fig6-new.png)

### <a name="multiple-domains-accounts-queried"></a>Dotazování více doménových účtů
Security Center dokáže zjistit vícenásobné pokusy o dotazování doménových účtů služby Active Directory, což obvykle provádějí útočníci během sondování sítě. Útočníci můžou tuto techniku využít k dotazování domény na identifikaci uživatelů, účtů správců domény, počítačů, které jsou řadiče domény a také potenciálního vztahu důvěryhodnosti s dalšími doménami.

Tady je příklad tohoto typu výstrahy:

![Výstraha na dotazování více doménových účtů](./media/security-center-alerts-type/security-center-alerts-type-fig7-new.png)

### <a name="local-administrators-group-members-were-enumerated"></a>Výpis členů místní skupiny Administrators

Security Center aktivuje výstrahu, když se v systému Windows Server 2016 a Windows 10 aktivuje událost zabezpečení 4798. To se stane, když dojde k výpisu členů skupin místních správců, což obvykle provádí útočníci během rekognoskace sítě. Útočníci mohou využívat tuto techniku k dotazování na identitu uživatelů s oprávněními správce.

Tady je příklad tohoto typu výstrahy:

![Místní správce](./media/security-center-alerts-type/security-center-alerts-type-fig14-new.png)

### <a name="anomalous-mix-of-upper-and-lower-case-characters"></a>Neobvyklá kombinace velkých a malých písmen

Security Center aktivuje výstrahu, když zjistí použití kombinace velkých a malých písmen na příkazovém řádku. Některé útočníci můžou používat tuto techniku ke skrytí před pravidly počítače pro velká a malá písmena nebo hodnoty hash.

Tady je příklad tohoto typu výstrahy:

![Neobvyklá kombinace](./media/security-center-alerts-type/security-center-alerts-type-fig15-new.png)

### <a name="suspected-kerberos-golden-ticket-attack"></a>Podezření na útok Kerberos Golden Ticket

Prozrazený klíč [krbtgt](https://technet.microsoft.com/library/dn745899.aspx) může útočník zneužít k vytvoření „zlatých lístků“ protokolu Kerberos, které útočníkovi umožní zosobnit libovolného uživatele. Security Center aktivuje výstrahu, když zjistí tento typ aktivity.

> [!NOTE] 
> Další informace o zlatém lístku protokolu Kerberos najdete v [průvodci zmírněním následků krádeže přihlašovacích údajů Windows 10](http://download.microsoft.com/download/C/1/4/C14579CA-E564-4743-8B51-61C0882662AC/Windows%2010%20credential%20theft%20mitigation%20guide.docx).

Tady je příklad tohoto typu výstrahy:

![Zlatý lístek](./media/security-center-alerts-type/security-center-alerts-type-fig16-new.png)

### <a name="suspicious-account-created"></a>Vytvoření podezřelého účtu

Security Center aktivuje výstrahu při vytvoření účtu, který se nápadně podobá existujícímu integrovanému účtu s oprávněním správce. Tento postup můžou použít útočníci k vytvoření podvodného účtu, aby se vyhnuli jeho zaznamenání při lidském ověření.
 
Tady je příklad tohoto typu výstrahy:

![Podezřelý účet](./media/security-center-alerts-type/security-center-alerts-type-fig17-new.png)

### <a name="suspicious-firewall-rule-created"></a>Vytvoření podezřelého pravidla brány firewall

Útočníci se můžou pokusit obejít zabezpečení hostitele vytvořením vlastních pravidel brány firewall, která umožňují škodlivým aplikacím komunikaci s příkazy a ovládáním nebo spuštění útoků prostřednictvím sítě přes napadeného hostitele. Security Center aktivuje výstrahu, když zjistí, že bylo vytvořeno nové pravidlo brány firewall ze spustitelného souboru v podezřelém umístění.
 
Tady je příklad tohoto typu výstrahy:

![Pravidlo brány firewall](./media/security-center-alerts-type/security-center-alerts-type-fig18-new.png)

### <a name="suspicious-combination-of-hta-and-powershell"></a>Podezřelá kombinace HTA a PowerShellu

Security Center aktivuje výstrahu, když zjistí, že Microsoft HTML Application Host (HTA) spouští příkazy PowerShellu. Jde o techniku používanou útočníky ke spuštění škodlivých skriptů PowerShellu.
 
Tady je příklad tohoto typu výstrahy:

![HTA a PS](./media/security-center-alerts-type/security-center-alerts-type-fig19-new.png)


## <a name="network-analysis"></a>Analýza sítě
Detekce síťových hrozeb ve službě Security Center funguje tak, že se automaticky shromažďují informace o zabezpečení z přenosu Azure IPFIX (Internet Protocol Flow Information Export). Za účelem identifikace hrozeb služba tyto informace analyzuje a často přitom koreluje data z různých zdrojů.

### <a name="suspicious-outgoing-traffic-detected"></a>Zjištěn podezřelý odchozí provoz
Síťová zařízení lze zjistit a profilovat velmi podobně jako jiné typy systémů. Útočníci obvykle začínají skenováním portů (port scanning) nebo hledáním konkrétního otevřeného portu (port sweeping). V následujícím příkladu máte podezřelý provoz SSH (Secure Shell) z virtuálního počítače. V tomto scénáři je možné, že došlo k útoku hrubou silou na SSH nebo útoku hledáním konkrétního otevřeného portu proti externímu prostředku.

![Výstraha na podezřelý odchozí provoz](./media/security-center-alerts-type/security-center-alerts-type-fig8.png)

Tato výstraha poskytuje informace, pomocí kterých můžete identifikovat prostředek použitý k zahájení útoku. Tato výstraha vám také pomůže identifikovat ohrožený počítač, čas detekce a použitý protokol a port. Na této stránce se zobrazuje také seznam nápravných kroků, kterými můžete problém zmírnit.

### <a name="network-communication-with-a-malicious-machine"></a>Síťová komunikace se škodlivým počítačem
Azure Security Center může pomocí informačních kanálů analýzy hrozeb Microsoft rozpoznat ohrožené počítače, které komunikují se škodlivými IP adresami. V mnoha případech jde o adresy řídicího centra. V tomto případě služba Security Center rozpoznala komunikaci provedenou pomocí malwaru Pony Loader (známého také pod názvem [Fareit](https://www.microsoft.com/security/portal/threat/encyclopedia/entry.aspx?Name=PWS:Win32/Fareit.AF)).

![výstraha na síťovou komunikaci](./media/security-center-alerts-type/security-center-alerts-type-fig9.png)

Tato výstraha vám pomůže identifikovat prostředek použitý k provedení útoku, ohrožený prostředek, IP adresu oběti i útočníka a čas detekce.

> [!NOTE]
> V zájmu ochrany osobních údajů jsme ze snímku obrazovky odebrali skutečné IP adresy.
>
>

### <a name="possible-outgoing-denial-of-service-attack-detected"></a>Zjištění možného odchozího útoku DoS
Neobvyklý síťový provoz pocházející z jednoho virtuálního počítače může vést ve službě Security Center k aktivaci výstrahy na potenciální útok DoS.

Tady je příklad tohoto typu výstrahy:

![Odchozí útok DoS](./media/security-center-alerts-type/security-center-alerts-type-fig10-new.png)

## <a name="resource-analysis"></a>Analýza prostředků
Analýza prostředků ve službě Security Center se soustředí na služby modelu Platforma jako služba (PaaS), jako je například integrace s funkcí [Zjišťování hrozeb Azure SQL Database](../sql-database/sql-database-threat-detection.md). V závislosti na výsledcích analýzy z těchto oblastí služba Security Center aktivuje výstrahu vztahující se na prostředek.

### <a name="potential-sql-injection"></a>Potenciální útok prostřednictvím injektáže SQL
Při útoku prostřednictvím injektáže SQL se škodlivý kód vkládá do řetězců, které jsou potom předány instanci SQL Serveru k parsování a spuštění. SQL Server spouští všechny syntakticky platné dotazy, které obdrží. Všechny procedury, které sestavují SQL příkazy, by tedy měly být chráněny proti tomuto typu útoku. Zjišťování hrozeb SQL využívá k určení podezřelých událostí, které mohou probíhat ve vašich databázích SQL Azure, strojové učení, analýzu chování a detekci anomálií. Například:

* Pokus o přístup k databázi bývalým zaměstnancem
* Útoky prostřednictvím injektáže SQL
* Neobvyklý přístup k produkční databázi uživatelem z domova

![Výstraha na potenciální útok prostřednictvím injektáže SQL](./media/security-center-alerts-type/security-center-alerts-type-fig11.png)

Informace v této výstraze vám pomůžou identifikovat napadený prostředek, čas detekce a stav útoku. Výstraha poskytuje také odkaz na další kroky šetření.

### <a name="vulnerability-to-sql-injection"></a>Zranitelnost vůči útoku prostřednictvím injektáže SQL
Tato výstraha se aktivuje v případě zjištění chyby aplikace v databázi. Tato výstraha může značit možnou zranitelnost vůči útokům prostřednictvím injektáže SQL.

![Výstraha na potenciální útok prostřednictvím injektáže SQL](./media/security-center-alerts-type/security-center-alerts-type-fig12-new.png)

### <a name="unusual-access-from-unfamiliar-location"></a>Neobvyklý přístup z neznámého umístění
Tato výstraha se aktivuje v případě, že byl na serveru zjištěn přístup z neznámé IP adresy, která se v poslední době neobjevovala.

![Výstraha na neobvyklý přístup](./media/security-center-alerts-type/security-center-alerts-type-fig13-new.png)

## <a name="contextual-information"></a>Kontextové informace
Během šetření analytici potřebují další kontext, aby došli k závěru ohledně povahy hrozby a způsobu, jak ji zmírnit.  Představte si například, že byla zjištěna síťová anomálie. Bez informací o tom, co dalšího se v síti děje, a s ohledem na cílový prostředek je velmi obtížné pochopit, jaká opatření je třeba provést. Bezpečnostní incident může zahrnovat artefakty, související události a další informace, které mohou vyšetřovateli pomoci. Dostupnost dalších informací se bude lišit v závislosti na typu zjištěné hrozby a konfiguraci vašeho prostředí. Nebudou dostupné pro všechny incidenty zabezpečení.

Pokud jsou dostupné další informace, zobrazí se v incidentu zabezpečení pod seznamem výstrah. Může se jednat o informace tohoto typu:

- Události vymazání protokolu
- Zařízení PNP zapojená z neznámého zařízení
- Výstrahy, na které není možné reagovat 

![Výstraha na neobvyklý přístup](./media/security-center-alerts-type/security-center-alerts-type-fig20.png) 


## <a name="see-also"></a>Viz také
V tomto článku jste se dozvěděli o různých typech výstrah zabezpečení ve službě Security Center. Pokud se o službě Security Center chcete dozvědět víc, pročtěte si tato témata:

* [Řešení bezpečnostních incidentů v Azure Security Center](security-center-incident.md)
* [Možnosti detekce v Azure Security Center](security-center-detection-capabilities.md)
* [Průvodce plánováním a provozem služby Azure Security Center](security-center-planning-and-operations-guide.md)
* [Časté otázky k Azure Security Center](security-center-faq.md): Přečtěte si nejčastější dotazy k používání této služby.
* [Blog o zabezpečení Azure](http://blogs.msdn.com/b/azuresecurity/) – Přečtěte si příspěvky o zabezpečení Azure a dodržování předpisů.
