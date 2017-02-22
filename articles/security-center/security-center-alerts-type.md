---
title: "Výstrahy zabezpečení podle typu ve službě Azure Security Center | Dokumentace Microsoftu"
description: "Tento dokument pomáhá porozumět typům výstrah zabezpečení dostupným ve službě Azure Security Center."
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
ms.date: 02/06/2017
ms.author: yurid
translationtype: Human Translation
ms.sourcegitcommit: dcda8b30adde930ab373a087d6955b900365c4cc
ms.openlocfilehash: bdd7d3d6e532efe7c3ed8225dd29a895725f2ff9


---
# <a name="security-alerts-by-type-in-azure-security-center"></a>Výstrahy zabezpečení podle typu ve službě Azure Security Center
Tento dokument vám pomůže porozumět různým typům výstrah zabezpečení dostupným ve službě Azure Security Center. Další informace o správě těchto výstrah najdete v tématu [Správa a zpracování výstrah zabezpečení ve službě Azure Security Center](security-center-managing-and-responding-alerts.md).

> [!NOTE]
> Pokud chcete povolit rozšířené detekce, upgradujte na Azure Security Center Standard. K dispozici je bezplatná 90denní zkušební verze. Pokud chcete provést upgrade, vyberte v [zásadách zabezpečení](security-center-policies.md) cenovou úroveň. Další informace najdete na [stránce s cenami](https://azure.microsoft.com/pricing/details/security-center/).
>
>

## <a name="what-type-of-alerts-are-available"></a>Jaké typy výstrah jsou k dispozici?
Azure Security Center poskytuje celou řadu výstrah uspořádaných podle jednotlivých fází modelu Cyber Kill Chain. Následující obrázek obsahuje několik příkladů různých výstrah a jejich vztah k některým z těchto fází.

![Model Kill Chain](./media/security-center-alerts-type/security-center-alerts-type-fig1.png)

**Cílení a útok**

* Příchozí útoky RDP/SSH
* Útoky na aplikaci a útoky DDoS (partneři brány WAF)
* Zjištění neoprávněného vniknutí (partneři brány NG Firewall)

**Instalace a zneužití**

* Známé malwarové signatury (partneři AM)
* Pokusy o malware a zneužití v paměti
* Podezřelé spouštění procesů
* Úhybné manévry pro zabránění objevení
* Laterální pohyb
* Interní sondování
* Podezřelá aktivita prostředí PowerShell

**Po průniku**  

* Komunikace se známou škodlivou IP adresou (průsak dat ven nebo příkazy a ovládání)
* Použití ohrožených prostředků pro připojení dalších útoků (skenování odchozích portů, útoky hrubou silou v protokolu RPD nebo SSH a spam)

Různé typy útoků jsou přidružené k jednotlivým fázím a cílí na různé subsystémy. Za účelem řešení útoků během těchto fází obsahuje služba Security Center tři kategorie výstrah:

* Analýza chování virtuálního počítače (VMBA)
* Analýza sítě
* Analýza prostředků

## <a name="virtual-machine-behavioral-analysis"></a>Analýza chování virtuálního počítače
Azure Security Center může pomocí analýzy chování identifikovat ohrožené prostředky na základě analýzy protokolů událostí virtuálního počítače, jako jsou události vytváření procesů, události přihlášení atd. Kromě toho se pomocí vzájemné souvislosti s dalšími signály hledají podpůrné důkazy rozšířené kampaně.

> [!NOTE]
> Další informace o tom, jak detekce služby Security Center pracuje, najdete v článku [Funkce detekce ve službě Azure Security Center](security-center-detection-capabilities.md).
>
>

### <a name="crash-analysis"></a>Analýza stavu systému
Metoda analýzy paměti ve výpisu stavu systému slouží ke zjištění sofistikovaného malwaru, který je schopný se vyhnout běžným řešením zabezpečení. Různé formy malwaru se snaží snížit pravděpodobnost svého zjištění antivirovými produkty tím, že nikdy nezapisují na disk nebo softwarové komponenty ukládané na disk šifruje. Takový malware je jen obtížně zjistitelný pomocí obvyklých antimalwarových postupů. Takový malware lze ale zjistit pomocí analýzy paměti, protože aby malware mohl fungovat, musí v paměti zanechat stopy.

Pokud dojde k chybě softwaru, ve výpisu stavu systému se zaznamená část paměti v době selhání. Taková chyba může být způsobena malwarem, běžnou aplikací nebo systémovým problémem. Díky analýze paměti ve výpisu stavu systému dokáže služba Security Center zjišťovat techniky, které využívají zranitelností softwaru, získávají přístup k důvěrným datům a nenápadně přetrvávají v napadeném počítači. Díky tomu, že se tato analýza provádí v back-endu služby Security Center, má minimální dopad na výkon hostitele.

Následující pole jsou společná pro výstrahy na analýzu výpisu stavu systému uvedené níže:

* DUMPFILE: Název souboru s výpisem stavu systému
* PROCESSNAME: Název procesu, ve kterém došlo k chybě
* PROCESSVERSION: Verze procesu, ve kterém došlo k chybě

### <a name="shellcode-discovered"></a>Zjištěn skrytý spustitelný kód
Skrytý spustitelný kód je datová část, která se spouští potom, co malware zneužije chybu zabezpečení softwaru. Tato výstraha znamená, že při analýze výpisu stavu systému byl nalezen spustitelný kód, který vykazuje chování typické pro škodlivý software. Někdy se může takovým způsobem chovat i software bez zlých úmyslů, pro běžný vývoj softwaru ale toto chování není typické.

Tato výstraha navíc obsahuje následující pole:

* ADDRESS: Umístění skrytého spustitelného kódu v paměti

Příklad tohoto typu výstrahy:

![Výstraha na skrytý spustitelný kód](./media/security-center-alerts-type/security-center-alerts-type-fig2.png)

### <a name="module-hijacking-discovered"></a>Zjištěno napadení modulu
Systém Windows umožňuje softwaru využívat základní systémové funkce prostřednictvím knihoven DLL (Dynamic Link Libraries). Při napadení knihoven DLL malware pozmění pořadí načítání knihoven DLL tak, aby umožnil načtení škodlivých datových částí do paměti, ze které je pak bude možné spustit. Tato výstraha znamená, že při analýze výpisu stavu systému byly nalezeny dva moduly s podobným názvem, ale různými cestami. Jedna z načtených cest přitom odpovídá běžnému umístění binárních souborů systému Windows.

Vývojáři legitimního softwaru někdy mění pořadí načítání knihoven DLL bez zlých úmyslů, například při instrumentaci, rozšiřování systému Windows nebo rozšiřování aplikací. Azure Security Center rozlišuje škodlivé změny pořadí načítání knihoven DLL od pravděpodobně bezpečných změn na základě profilů podezřelého chování. Výsledek kontroly je u výstrahy uveden v poli SIGNATURE a určuje závažnost výstrahy, popis výstrahy a nápravné kroky. Další informace o povaze nestandardního modulu může poskytnout analýza kopie modulu uložené na disku. Při této analýze se provádí například ověření digitálního podpisu nebo antivirová kontrola.

Kromě společných polí popsaných výše v části „Zjištěn skrytý spustitelný kód“ tato výstraha obsahuje taky následující pole:

* SIGNATURE: Udává, jestli napadený modul odpovídá profilu podezřelého chování
* HIJACKEDMODULE: Název napadeného modulu systému Windows
* HIJACKEDMODULEPATH: Cesta k napadenému modulu systému Windows
* HIJACKINGMODULEPATH: Cesta k napadajícímu modulu

Příklad tohoto typu výstrahy:

![Výstraha na napadení modulu](./media/security-center-alerts-type/security-center-alerts-type-fig3.png)

### <a name="masquerading-windows-module-detected"></a>Zjištěno maskování za modul systému Windows
Malware může používat běžné názvy spustitelných souborů (např. SVCHOST.EXE) nebo modulů (např. NTDLL.DLL) systému Windows, aby nevyvolával podezření a skryl svou škodlivou povahu před správci systému. Tato výstraha znamená, že při analýze výpisu stavu systému byly nalezeny moduly, které svým názvem odpovídají systémovým modulům Windows, ale nesplňují další kritéria typická pro systémové moduly.. Další informace o povaze maskovaného modulu může poskytnout analýza kopie modulu na disku. Součástí analýzy může být:

* Potvrzení, že je příslušný soubor součástí balíčku legitimního softwaru
* Ověření digitálního podpisu souboru
* Antivirová kontrola souboru

Kromě společných polí popsaných výše v části „Zjištěn skrytý spustitelný kód“ tato výstraha obsahuje taky následující pole:

* DETAILS: Udává, jestli jsou metadata modulu platná a jestli byl modul načten ze systémové cesty.
* NAME: Název maskovaného modulu systému Windows
* PATH: Cesta k maskovanému modulu systému Windows.

Tato výstraha také extrahuje a zobrazuje určitá pole ze záhlaví PE modulu, např. pole CHECKSUM a TIMESTAMP. Tato pole se zobrazí, pouze pokud se v modulu nacházejí. Podrobnosti o těchto polích naleznete ve [specifikaci formátů Microsoft PE a COFF](https://msdn.microsoft.com/windows/hardware/gg463119.aspx).

Příklad tohoto typu výstrahy:

![Výstraha na maskování za modul systému Windows](./media/security-center-alerts-type/security-center-alerts-type-fig4.png)

### <a name="modified-system-binary-discovered"></a>Zjištěna úprava binárního systémového souboru
Malware může upravit binární systémové soubory, aby mohl nepozorovaně přistupovat k datům nebo skrytě přetrvávat v napadeném systému. Tato výstraha znamená, že při analýze stavu systému byly v paměti nebo na disku nalezeny upravené binární soubory jádra systému Windows.
Vývojáři legitimního softwaru někdy upravují systémové soubory v paměti bez zlých úmyslů, například soubory balíčku Detours pro zajištění kompatibility aplikací. Azure Security Center rozlišuje škodlivé moduly od pravděpodobně bezpečných modulů na základě profilů podezřelého chování. Výsledek kontroly určuje závažnost výstrahy, popis výstrahy a nápravné kroky.

Kromě společných polí popsaných výše v části „Zjištěn skrytý spustitelný kód“ tato výstraha obsahuje taky následující pole:

* MODULENAME: Název upraveného binárního systémového souboru
* MODULEVERSION: Verze upraveného binárního systémového souboru

Příklad tohoto typu výstrahy:

![Výstraha na upravený binární systémový soubor](./media/security-center-alerts-type/security-center-alerts-type-fig5.png)

### <a name="suspicious-process-executed"></a>Spuštění podezřelého procesu
Security Center identifikuje provádění podezřelých procesů na cílovém virtuálním počítači a aktivuje výstrahu. Detekce nevyhledává konkrétní název procesu, ale podle jeho parametrů. Proto je služba Security Center schopná zjistit spustitelný soubor i v případě, že jej útočník přejmenuje.

Příklad tohoto typu výstrahy:

![Výstraha na spuštění podezřelého procesu](./media/security-center-alerts-type/security-center-alerts-type-fig6-new.png)

### <a name="multiple-domain-accounts-queried"></a>Dotazování více doménových účtů
Security Center dokáže zjistit vícenásobné pokusy o dotazování doménových účtů, což obvykle provádějí útočníci během sondování sítě. Útočníci mohou tuto techniku využít k dotazování domény na identifikaci toho, kdo jsou uživatelé, které účty jsou správci domény, které počítače jsou řadiče domény a také potenciálního vztahu důvěryhodnosti s dalšími doménami.

Příklad tohoto typu výstrahy:

![Výstraha na dotazování více doménových účtů](./media/security-center-alerts-type/security-center-alerts-type-fig7-new.png)

## <a name="network-analysis"></a>Analýza sítě
Detekce síťových hrozeb ve službě Security Center funguje tak, že se automaticky shromažďují informace o zabezpečení z přenosu Azure IPFIX (Internet Protocol Flow Information Export). Za účelem identifikace hrozeb služba tyto informace analyzuje a často přitom koreluje data z různých zdrojů.

### <a name="suspicious-outgoing-traffic-detected"></a>Zjištěn podezřelý odchozí provoz
Síťová zařízení lze zjistit a profilovat velmi podobně jako jiné typy systémů. Útočníci obvykle začínají skenováním portů (port scanning) nebo hledáním konkrétního otevřeného portu (port sweeping). Následující příklad ukazuje podezřelý provoz SSH z virtuálního počítače, který může provádět útok na SSH hrubou silou nebo cílit na konkrétní port externího prostředku.

![Výstraha na podezřelý odchozí provoz](./media/security-center-alerts-type/security-center-alerts-type-fig8.png)

Tato výstraha vám pomůže identifikovat prostředek, který byl použitý k zahájení tohoto útoku, ohrožený počítač, čas detekce, protokol a port. V tomto okně se zobrazuje taky seznam nápravných kroků, kterými můžete problém zmírnit.

### <a name="network-communication-with-a-malicious-machine"></a>Síťová komunikace se škodlivým počítačem
Azure Security Center může pomocí kanálů analýzy hrozeb Microsoft rozpoznat ohrožené počítače, které komunikují se škodlivými IP adresami. V mnoha případech jde o adresy řídicího centra. V tomto případě služba Azure Security Center rozpoznala komunikaci provedenou pomocí malwaru Pony Loader (známého taky pod názvem [Fareit](https://www.microsoft.com/security/portal/threat/encyclopedia/entry.aspx?Name=PWS:Win32/Fareit.AF)).

![výstraha na síťovou komunikaci](./media/security-center-alerts-type/security-center-alerts-type-fig9.png)

Tato výstraha vám pomůže identifikovat prostředek použitý k provedení útoku, ohrožený prostředek, IP adresu oběti i útočníka a čas detekce.

> [!NOTE]
> V zájmu ochrany osobních údajů jsme ze snímku obrazovky odebrali skutečné IP adresy.
>
>

### <a name="possible-outgoing-denial-of-service-attack-detected"></a>Zjištění možného odchozího útoku DoS
Neobvyklý síťový provoz pocházející z jednoho virtuálního počítače může vést ve službě Security Center k aktivaci výstrahy na potenciální útok DoS.

Příklad tohoto typu výstrahy:

![Odchozí útok DoS](./media/security-center-alerts-type/security-center-alerts-type-fig10-new.png)

## <a name="resource-analysis"></a>Analýza prostředků
Analýza prostředků ve službě Security Center se soustředí na služby modelu PaaS, jako je například integrace s funkcí [Zjišťování hrozeb Azure SQL DB](../sql-database/sql-database-threat-detection-get-started.md). V závislosti na výsledcích analýzy z těchto oblastí služba Security Center aktivuje výstrahu vztahující se na prostředek.

### <a name="potential-sql-injection"></a>Potenciální útok prostřednictvím injektáže SQL
Při útoku SQL Injection se škodlivý kód vkládá do řetězců, které jsou potom předány instanci SQL Serveru k parsování a spuštění. SQL Server spouští všechny syntakticky platné dotazy, které obdrží. Všechny procedury, které sestavují SQL příkazy, by tedy měly být chráněny proti tomuto typu útoku. Zjišťování hrozeb SQL využívá k určení podezřelých událostí, které mohou probíhat ve vašich službách Azure SQL Database, strojové učení, analýzu chování a detekci anomálií. Příklad:

* Pokus o přístup k databázi bývalým zaměstnancem
* Útoky prostřednictvím injektáže SQL
* Neobvyklý přístup k produkční databázi uživatelem z domova

![Výstraha na potenciální útok prostřednictvím injektáže SQL](./media/security-center-alerts-type/security-center-alerts-type-fig11.png)

Tato výstraha vám pomůže identifikovat napadený zdroj, ukazuje čas detekce a stav útoku a poskytuje odkaz na další kroky šetření.

### <a name="vulnerability-to-sql-injection"></a>Zranitelnost vůči útoku prostřednictvím injektáže SQL
Tato výstraha se aktivuje v případě zjištění chyby aplikace v databázi, což může značit možnou zranitelnost vůči útokům prostřednictvím injektáže SQL.

![Výstraha na potenciální útok prostřednictvím injektáže SQL](./media/security-center-alerts-type/security-center-alerts-type-fig12-new.png)

### <a name="unusual-access-from-unfamiliar-location"></a>Neobvyklý přístup z neznámého umístění
Tato výstraha se aktivuje v případě, že byl na serveru zjištěn přístup z neznámé IP adresy, která se v poslední době neobjevovala.

![Výstraha na neobvyklý přístup](./media/security-center-alerts-type/security-center-alerts-type-fig13-new.png)

## <a name="see-also"></a>Viz také
V tomto dokumentu jste se dozvěděli o různých typech výstrah zabezpečení ve službě Security Center. Pokud se o službě Security Center chcete dozvědět víc, pročtěte si tato témata:

* [Řešení bezpečnostních incidentů v Azure Security Center](security-center-incident.md)
* [Funkce detekce ve službě Azure Security Center](security-center-detection-capabilities.md)
* [Průvodce plánováním a provozem služby Azure Security Center](security-center-planning-and-operations-guide.md)
* [Azure Security Center – nejčastější dotazy](security-center-faq.md) – Přečtěte si nejčastější dotazy o použití této služby.
* [Blog o zabezpečení Azure](http://blogs.msdn.com/b/azuresecurity/) – Přečtěte si příspěvky o zabezpečení Azure a dodržování předpisů.



<!--HONumber=Dec16_HO1-->


