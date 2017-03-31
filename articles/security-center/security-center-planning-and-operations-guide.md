---
title: "Průvodce plánováním a provozem služby Security Center | Dokumentace Microsoftu"
description: "Tento dokument vám pomůže s plánováním před přijetím řešení Azure Security Center a s vyřešením aspektů každodenního provozu."
services: security-center
documentationcenter: na
author: YuriDio
manager: swadhwa
editor: 
ms.assetid: f984e4a2-ac97-40bf-b281-2f7f473494c4
ms.service: security-center
ms.topic: hero-article
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/30/2017
ms.author: yurid
translationtype: Human Translation
ms.sourcegitcommit: dcda8b30adde930ab373a087d6955b900365c4cc
ms.openlocfilehash: 71fdf71d48959856697603c765e299f8a40b089e
ms.lasthandoff: 12/07/2016


---
# <a name="azure-security-center-planning-and-operations-guide"></a>Průvodce plánováním a provozem služby Azure Security Center
Tento průvodce je určený odborníkům na informační technologie (IT), IT architektům, analytikům zabezpečení informací a správcům cloudů, jejichž společnosti hodlají začít používat Azure Security Center.

## <a name="planning-guide"></a>Průvodce plánováním
Tato příručka pojednává o krocích a úkolech, které můžete provést, abyste přizpůsobili použití služby Security Center požadavkům na zabezpečení a modelu správy cloudu ve vaší organizaci. Pokud chcete využívat všech výhod služby Security Center, je důležité pochopit, jak různí jednotlivci nebo týmy ve vaší organizaci používají tuto službu k tomu, aby vyhověli požadavkům na zabezpečený vývoj a provoz, sledování, řízení a reakce na incidenty. Při plánování použití služby Security Center je potřeba zvážit tyto klíčové oblasti:

* Role zabezpečení a řízení přístupu
* Zásady a doporučení zabezpečení
* Shromažďování dat a úložiště
* Průběžné sledování zabezpečení
* Reakce na incidenty

V další části se dozvíte, jak každou z těchto oblastí naplánovat a jak tato doporučení uplatnit v kombinaci s vašimi požadavky.

> [!NOTE]
> V tématu [Azure Security Center – nejčastější dotazy](security-center-faq.md) najdete seznam běžných dotazů, které vám můžou pomoct i ve fázi návrhu a plánování.
> 
> 

## <a name="security-roles-and-access-controls"></a>Role zabezpečení a řízení přístupu
V závislosti na velikosti a struktuře vaší organizace můžou službu Security Center používat různí jednotlivci a týmy a provádět v ní různé úkony týkající se zabezpečení. Následující diagram ukazuje příklad zahrnující fiktivní osoby a jejich příslušné role a povinnosti v oblasti zabezpečení:

![Role](./media/security-center-planning-and-operations-guide/security-center-planning-and-operations-guide-fig01-ga.png)

Security Center umožňuje těmto osobám tyto různé povinnosti plnit. Například:

**Jeff (vlastník úloh v cloudu)**

* Spravuje úlohy v cloudu a související prostředky
* Odpovídá za implementace a správu bezpečnostních mechanismů v souladu se zásadami zabezpečení společnosti

**Ellen (ředitelka zabezpečení informací / ředitelka IT)**

* Odpovídá za všechny aspekty zabezpečení společnosti
* Odpovídá za jednotný postoj společnosti k zabezpečení napříč všemi úlohami v cloudu
* Musí být informována o hlavních útocích a rizicích

**David (zabezpečení IT)**

* Nastavuje zásady zabezpečení společnosti, aby zajistil funkčnost požadovaných bezpečnostních mechanismů
* Dohlíží na dodržování zásad
* Generuje sestavy pro vedení a auditory

**Judy (pracovnice oddělení zabezpečení)**

* Nepřetržitě sleduje výstrahy zabezpečení a reaguje na ně
* Problémy předává vlastníkovi úloh v cloudu nebo analytikovi zabezpečení IT

**Sam (analytik zabezpečení)**

* Zkoumá útoky
* Napravuje výstrahy nebo pracuje na nápravě s vlastníkem úlohy v cloudu 

Security Center používá [řízení přístupu na základě rolí (RBAC)](../active-directory/role-based-access-control-configure.md). To poskytuje [předdefinované role](../active-directory/role-based-access-built-in-roles.md), které se dají v Azure přiřadit uživatelům, skupinám a službám. Když uživatel otevře službu Security Center, uvidí jenom informace související s prostředky, ke kterým má přístup. To znamená, že uživateli je přiřazena role Vlastníka, Přispěvatele nebo Čtenáře předplatného nebo skupiny prostředků, do které prostředek patří. 

> [!NOTE]
> Aby uživatel mohl zobrazit Security Center v Azure, musí být alespoň vlastníkem nebo přispěvatelem skupiny prostředků předplatného.
> 
> 

Použití osob uvedených v předchozím diagramu vyžaduje následující nastavení RBAC:

**Jeff (vlastník úloh v cloudu)**

* Vlastník/spolupracovník skupiny prostředků

**David (zabezpečení IT)**

* Vlastník/spolupracovník předplatného

**Judy (pracovnice oddělení zabezpečení)**

* Čtenář předplatného s oprávněním zobrazovat výstrahy
* Vlastník/spolupracovník předplatného potřebný k rušení výstrah

**Sam (analytik zabezpečení)**

* Čtenář předplatného s oprávněním zobrazovat výstrahy
* Vlastník/spolupracovník předplatného potřebný k nápravě nebo rušení výstrah
* Může potřebovat přístup k úložišti

Další informace, které je potřeba vzít v úvahu:

* Zásady zabezpečení můžou upravovat jenom vlastníci a přispěvatelé odběru.
* Doporučení zabezpečení pro určitý prostředek můžou uplatňovat jenom vlastníci a přispěvatelé předplatného a skupiny prostředků.

Při plánování řízení přístupu pomocí RBAC pro Security Center je potřeba si dobře rozmyslet, kdo z vaší organizace bude Security Center používat. Ke správné konfiguraci RBAC je také důležité, jaké typy úloh budou uživatelé provádět.

> [!NOTE]
> Doporučujeme přiřadit uživatelům tu nejvíc omezenou roli, kterou ke své práci potřebují. Třeba uživatelům, kteří potřebují jenom zobrazovat informace o stavu zabezpečení prostředků, ale nemusí provádět žádné kroky, třeba uplatňovat doporučení nebo upravovat zásady, by se měla přiřadit role Čtenář.
> 
> 

## <a name="security-policies-and-recommendations"></a>Zásady a doporučení zabezpečení
Zásady zabezpečení definují sadu ovládacích prvků doporučenou pro prostředky v rámci daného předplatného nebo skupiny prostředků. V Security Center určíte zásady na základě toho, jaké má vaše společnost požadavky na zabezpečení, a podle typu aplikací nebo citlivosti dat.

Zásady povolené na úrovni předplatného se automaticky šíří do všech skupin prostředků v rámci tohoto předplatného, jak ukazuje následující diagram:

![Zásady zabezpečení](./media/security-center-planning-and-operations-guide/security-center-planning-and-operations-guide-fig2-ga.png)

Jak ukazuje předchozí obrázek, zásady zabezpečení pro skupiny prostředků je možné zdědit z úrovně předplatného.

V některých scénářích, kdy můžete mít ve skupině prostředků prostředky, které vyžadují jinou sadu zásad, můžete dědičnost zásad zakázat a u určité skupiny prostředků použít vlastní zásady.

Pokud potřebujete mít v určitých skupinách prostředků vlastní zásady, zakažte v těchto skupinách prostředků dědičnost a změňte zásady zabezpečení. Pokud třeba máte některé úlohy, které nevyžadují zásadu transparentního šifrování dat SQL, vypněte tuto zásadu na úrovni předplatného a povolte ji jenom ve skupinách prostředků, ve kterých je potřeba.

Když začnete pro různé skupiny prostředků vytvářet vlastní zásady, měli byste nasazení zásad naplánovat s vědomím, že v případě konfliktu zásad (mezi zásadami předplatného a zásadami skupiny prostředků) mají přednost zásady skupiny prostředků.

> [!NOTE]
> Pokud potřebujete zkontrolovat, které zásady jste změnily, můžete použít [protokoly auditu Azure](https://blogs.msdn.microsoft.com/cloud_solution_architect/2015/03/10/audit-logs-for-azure-events/). V protokolech auditu jsou zaznamenané všechny změny zásad.
> 
> 

### <a name="security-recommendations"></a>Doporučení zabezpečení
Než začnete konfigurovat zásady zabezpečení, prostudujte si všechna [doporučení zabezpečení](security-center-recommendations.md) a zkontrolujte, jestli jsou dané zásady vhodné pro vaše předplatné a skupiny prostředků. Je také důležité pochopit, jaké kroky je potřeba k uplatnění doporučení zabezpečení provést.

**Endpoint Protection** (Ochrana koncových bodů): Pokud virtuální počítač nemá povolené žádné řešení ochrany koncových bodů, Security Center vám doporučí, abyste nějaké nainstalovali. Pokud upřednostňujete určité řešení ochrany koncových bodů, které už jste na místním pracovišti zavedli, budete se muset rozhodnout, jestli stejný antimalwarový software použijete i pro virtuální počítače Azure. Security Center nabízí několik možností ochrany koncových bodů.  Můžete použít bezplatnou službu Microsoft Antimalware nebo si vybrat ze seznamu řešení ochrany koncových bodů od integrovaných partnerů. Další informace o tom, jak pomocí služby Security Center nasadit antimalwarové řešení, najdete v tématu [Instalace ochrany koncových bodů v Azure Security Center](security-center-install-endpoint-protection.md).

**System Updates** (Aktualizace systému): Azure Security Center vyhledá virtuální počítače IaaS a služby Cloud Services (PaaS), kterým chybí aktualizace zabezpečení nebo kritické aktualizace operačního systému. Zamyslete se nad tím, kdo bude zodpovídat za instalaci potřebných aktualizací a jak se budou aktualizace instalovat. Řada organizací používá službu WSUS, Windows Update nebo jiný nástroj.

**Baseline Configurations** (Standardní hodnoty konfigurace): Pokud konfigurace operačního systému virtuálních počítačů neodpovídají doporučeným standardním hodnotám, zobrazí se doporučení. [Zkontrolujte sadu standardních hodnot](https://gallery.technet.microsoft.com/Azure-Security-Center-a789e335) a zvažte, jak se budou konfigurace operačních systémů uplatňovat.

**Disk Encryption** (Šifrování disku): Pokud máte virtuální počítače s nešifrovanými disky, Security Center doporučí, abyste použili službu Azure Disk Encryption. Tato služba za pomoci nástroje BitLocker pro Windows a DM-Crypt pro Linux zajišťuje šifrování disků s operačním systémem a daty. Toto doporučení vás přesměruje na [podrobného průvodce](security-center-disk-encryption.md), který obsahuje pokyny k zašifrování.

Upozorňujeme, že existuje několik různých scénářů, ve kterých šifrování probíhá. Při plánování budete muset vzít v úvahu jedinečné požadavky každého z těchto scénářů:

* Šifrování nových virtuálních počítačů Azure z virtuálních pevných disků, které jste zašifrovali pomocí vlastních šifrovacích klíčů
* Šifrování nových virtuálních počítačů Azure, které jste vytvořili z galerie Azure
* Šifrování virtuálních počítačů Azure, které už v Azure běží

Požadavky na plánování se u každého z těchto scénářů liší. Podrobnosti o každém z těchto scénářů najdete v [dokumentu white paper služby Azure Disk Encryption](https://gallery.technet.microsoft.com/Azure-Disk-Encryption-for-a0018eb0).

**Web Application Firewall** (Firewall webových aplikací): Security Center vyhledá virtuální počítače, na kterých běží webové aplikace, a doporučí vám instalaci firewallu webových aplikací. Vyhodnoťte, které z dostupných partnerských řešení vaší organizaci nejvíc vyhovuje, a určete, jak k tomuto řešení získáte licenci (partneři můžou podporovat model použití vlastní licence nebo model průběžných plateb). Další informace o tom, jak pomocí Security Center nasadit na virtuální počítače Azure bránu firewall webových aplikací, najdete v tématu [Přidání brány firewall webových aplikací v Azure Security Center](security-center-add-web-application-firewall.md).

**Next Generation Firewall** (Brána firewall příští generace): Umožňuje poskytovat virtuální zařízení od předních dodavatelů včetně řešení Check Point a brzy taky společností Cisco a Fortinet. Tato zásada rozšiřuje ochranu sítě nad rámec skupin zabezpečení sítě, které jsou integrované v Azure. Služba Security Center bude zjišťovat nasazení, pro která se doporučuje brána firewall příští generace, a umožní vám zřídit virtuální zařízení.

**Virtual Networking** (Virtuální sítě): Security Center vyhodnotí infrastrukturu a konfiguraci [Azure Virtual Network](https://azure.microsoft.com/documentation/services/virtual-network/) a zkontroluje, jestli se používají [skupiny zabezpečení sítě](../virtual-network/virtual-networks-nsg.md) a jestli mají tyto skupiny správně nakonfigurovaná pravidla pro příchozí přenosy dat. Měli byste zvážit, jaká pravidla pro přenosy dat je potřeba určit, a tyto požadavky sdělit osobám, které budou uplatňovat příslušná doporučení zabezpečení.

Security Center vám doporučí, abyste ve svém předplatném Azure uvedli podrobnosti o kontaktu zabezpečení. Prostřednictvím tohoto kontaktu se na vás společnost Microsoft obrátí, pokud středisko Microsoft Security Response Center (MSRC) zjistí, že k datům zákazníka nezákonně nebo neoprávněně přistupovala třetí strana. Další informace o povolení tohoto doporučení najdete v článku [Zadání podrobností o kontaktu zabezpečení do Azure Security Center](security-center-provide-security-contact-details.md).

## <a name="data-collection-and-storage"></a>Shromažďování dat a úložiště
Důrazně doporučujeme zapnout pro každé z vašich předplatných shromažďování dat, tím totiž zajistíte dostupnost sledování zabezpečení pro všechny virtuální počítače. Shromažďování dat se povoluje pomocí nástroje Azure Monitoring Agent (ASMAgentLauncher.exe) a rozšíření Azure Security Monitoring (ASMMonitoringAgent.exe).

Rozšíření Azure Security Monitoring prochází různé položky konfigurace v oblasti zabezpečení a shromažďuje protokoly zabezpečení z daného virtuálního počítače. Tato data se odesílají na účet úložiště, který zadáte. Na virtuální počítač se nainstaluje taky správce procházení (ASMSoftwareScanner.exe), který bude vyhledávat opravy.

Agent Azure Security Monitoring (ASM) má následující základní nároky:

* Většinou 3 MB paměti s možnými výkyvy na 10 MB, když se scénář spustí každých 12 hodin  
* Nepatrné zatížení procesoru z trvalého procesu i vyhledávačů 
* Nepatrné využití disku

Agent ASM má sled procesů, které můžou představovat dohromady přibližně 30 MB v paměti.  Každá instance agenta sledování může využívat až 3 GB místa na disku. Každá instance je limitovaná 20 % zatížení procesoru, i když v praxi to je mnohem méně. 

Když v zásadě zabezpečení povolíte shromažďování dat, agent sledování a rozšíření se automaticky nainstalují na všechny stávající a nově podporované virtuální počítače zřízené v Azure.  Proces agenta je navržen tak, aby nenarušoval běžný chod a měl zcela minimální dopad na výkon virtuálního počítače.

> [!NOTE]
> Další informace o odstraňování potíží souvisejících s nástrojem Azure Security Monitoring Agent najdete v článku [Průvodce odstraňováním potíží pro službu Azure Security Center](security-center-troubleshooting-guide.md).
> 
> 

Pokud někdy budete chtít shromažďování dat vypnout, můžete to udělat v zásadě zabezpečení. Pokud chcete odstranit dřív nasazené agenty sledování, vyberte v nabídce možnost Delete Agents (Odstranit agenty).

> [!NOTE]
> V tématu [Azure Security Center – nejčastější dotazy](security-center-faq.md) najdete seznam podporovaných virtuálních počítačů.
> 
> 

Pro každou oblast, ve které máte spuštěné virtuální počítače, si zvolíte účet úložiště, na který se mají ukládat data shromážděná z daných virtuálních počítačů. Pokud pro každou oblast nevyberete účet úložiště, vytvoří se vám automaticky. Pro každou oblast můžete vybrat umístění úložiště nebo uložit všechny informace do centrálního umístění. Zatímco zásady zabezpečení se dají nastavit na úrovni předplatného Azure a na úrovni skupiny prostředků, oblast pro účet úložiště můžete nastavit jenom na úrovni předplatného.

Pokud používáte účet úložiště, který je sdílený mezi různými prostředky Azure, nezapomeňte si přečíst článek [Škálovatelnost a cíle výkonnosti služby Azure Storage](../storage/storage-scalability-targets.md). V něm najdete další informace o limitech velikosti a dalších omezeních. Vaše předplatné má také nastavené limity účtu úložiště. Jejich podrobnější vysvětlení najdete v článku [Omezení a kvóty předplatného a služeb Azure](../azure-subscription-service-limits.md).

Náklady spojené s tímto úložištěm nejsou součástí ceny služby Security Center a budou se účtovat zvlášť podle běžného [ceníku úložiště Azure](https://azure.microsoft.com/pricing/details/storage/). Z pohledu plánování byste měli vědět, že Security Center bude přidávat data do vašeho úložiště Azure, což může zvýšit roční náklady o 1 až 3 dolary.

Je taky potřeba naplánovat aspekty výkonu a škálovatelnosti na základě velikosti vašeho prostředí Azure a prostředků, které váš účet úložiště využívají. Další informace najdete v článku [Kontrolní seznam výkonu a škálovatelnosti služby Microsoft Azure Storage](../storage/storage-performance-checklist.md).

> [!NOTE]
> Microsoft se pevně zavazuje, že soukromí a bezpečnost těchto dat bude chránit. Společnost Microsoft dodržuje přísné pokyny pro dodržování předpisů a zabezpečení – od psaní kódu po provoz služeb. Další informace o zpracování dat a ochraně osobních údajů najdete v článku [Zabezpečení dat ve službě Azure Security Center](security-center-data-security.md).
> 
> 

## <a name="ongoing-security-monitoring"></a>Průběžné sledování zabezpečení
Po počáteční konfiguraci a uplatnění doporučení služby Security Center je dalším krokem zvážení provozních procesů Security Center.

Na portálu Azure můžete ke službě Security Center získat přístup tak, že kliknete na **Procházet** a do pole **Filtrovat** zadáte **Security Center**. Zobrazí se položky na základě filtrů, které uživatel vybere.

Security Center nebude ovlivňovat vaše běžné provozní postupy, bude jenom pasivně sledovat vaše nasazení a poskytovat doporučení na základě zásad zabezpečení, které povolíte.

Řídicí panel Security Center je rozdělený na dvě hlavní části:

* Prevention (Prevence)
* Detection (Detekce)

Když ve službě Security Center poprvé povolíte shromažďování dat ze svého stávajícího prostředí Azure, nezapomeňte si projít všechna doporučení. To můžete udělat v okně **Recommendations** (Doporučení) nebo u jednotlivých prostředků – **Virtual Machine** (Virtuální počítač), **Networking** (Sítě), **SQL** a **Application** (Aplikace).

Až vyřešíte všechna doporučení, v části **Prevention** (Prevence) by měly být všechny vyřešené prostředky označené zeleně. Průběžné sledování je od této chvíle jednodušší, protože budete provádět kroky jenom na základě změn stavu zabezpečení prostředků a dlaždic doporučení.

Část **Detection** (Detekce) vyžaduje víc reakcí, obsahuje totiž výstrahy týkající se problémů, které jsou aktuální nebo nastaly v minulosti, kdy je zjistily ovládací prvky služby Security Center a systémy jiných výrobců. Na dlaždici Security Alerts (Výstrahy zabezpečení) se zobrazí pruhové grafy znázorňující počet výstrah o zjištěných hrozbách, které v určitém dni našly, a o jejich přiřazení do různých kategorií závažnosti (nízká, střední, vysoká). Další informace o výstrahách zabezpečení najdete v tématu [Správa a zpracování výstrah zabezpečení v Azure Security Center](security-center-managing-and-responding-alerts.md).

> [!NOTE]
> Také můžete využít řešení Microsoft Power BI, které umožňuje znázornění dat ze služby Security Center. Přečtěte si článek [Přehledné znázornění dat z Azure Security Center v řešení Power BI](security-center-powerbi.md).
> 
> 

### <a name="monitoring-for-new-or-changed-resources"></a>Sledování nových nebo změněných prostředků
Většina prostředí Azure je dynamická a pravidelně v nich probíhá přidávání a odebírání prostředků, konfigurace a další změny. Security Center pomáhá zajistit přehled o stavu zabezpečení těchto nových prostředků.

Když do prostředí Azure přidáte nové prostředky (virtuální počítače, databáze SQL), Security Center tyto prostředky automaticky zjistí a začne sledovat jejich zabezpečení. To zahrnuje také webové role a role pracovního procesu PaaS. Pokud je v [zásadě zabezpečení](security-center-policies.md) povolené shromažďování dat, pro vaše virtuální počítače se automaticky povolí další funkce sledování.

![Klíčové oblasti](./media/security-center-planning-and-operations-guide/security-center-planning-and-operations-guide-fig3-ga.png)

1. K dlaždici **Resource security health** (Stav zabezpečení prostředků) pro virtuální počítače získáte přístup kliknutím na **Virtual Machines** (Virtuální počítače). Případné problémy s povolováním shromažďování dat nebo související doporučení se zobrazí v části **Monitoring Recommendations** (Doporučení pro sledování).
2. V části **Recommendations** (Doporučení) zjistíte, jestli se u nového prostředku zjistila nějaká rizika zabezpečení.
3. Je velmi běžné, že když se do vašeho prostředí přidají nové virtuální počítače, nejdřív mají nainstalovaný jen operační systém. Vlastníkovi prostředku může nějakou dobu trvat, než nasadí další aplikace, které budou tyto virtuální počítače používat.  V ideálním případě byste měli znát konečný záměr této úlohy. Bude se jednat o aplikační server? Podle toho, co k čemu bude tato nová úloha sloužit, můžete povolit vhodnou **zásadu zabezpečení**, což je třetí krok v tomto pracovním postupu.
4. Během přidávání nových prostředků do prostředí Azure je možné, že se na dlaždici **Security Alerts** (Výstrahy zabezpečení) objeví nové výstrahy. Vždycky zkontrolujte, jestli na této dlaždici nejsou nové výstrahy, a proveďte příslušné kroky na základě doporučení služby Security Center.

Taky je dobré pravidelně sledovat stav stávajících prostředků, abyste zjistili, které změny konfigurace vyvolaly bezpečnostní rizika, odlišily se od doporučených standardních hodnot nebo způsobily vydání výstrah zabezpečení. Začněte na řídicím panelu Security Center. Jsou tři hlavní oblasti, které byste měli pravidelně kontrolovat.

![Provoz](./media/security-center-planning-and-operations-guide/security-center-planning-and-operations-guide-fig4.png)

1. Panel **Resource security health** (Stav zabezpečení prostředků) nabízí rychlý přístup ke klíčovým prostředkům. Pomocí této možnost můžete sledovat virtuální počítače, sítě, SQL a aplikace.
2. Panel **Recommendations** (Doporučení) umožňuje kontrolovat doporučení služby Security Center. Při průběžném sledování můžete zjistit, že se vám doporučení nezobrazují každý den, což je normální, protože jste všechna doporučení zjistili při počátečním nastavování služby Security Center. Z toho důvodu se vám v této části nemusí každý den zobrazovat nové informace, takže ji musíte otevírat, jenom když je to nutné.
3. Panel **Detection** (Detekce) se může měnit velmi často i velmi zřídka. Vždy zkontrolujte výstrahy zabezpečení a proveďte akce na základě doporučení služby Security Center.

## <a name="incident-response"></a>Reakce na incidenty
Security Center vyhledává nové hrozby a upozorňuje vás na ně. Organizace by měly výstrahy zabezpečení aktivně sledovat a provádět potřebné kroky, aby útok podrobněji prošetřily nebo napravily. Další informace o tom, jak detekce hrozeb služby Security Center pracuje, najdete v článku [Funkce detekce ve službě Azure Security Center](security-center-detection-capabilities.md).

Cílem tohoto článku není pomoct vám s vytvořením vlastního plánu reakcí na incidenty, postup uvedený v dokumentu Microsoft Azure Security Response in the Cloud (Reakce zabezpečení Microsoft Azure v cloudu) ale použijeme jako základ pro fáze reakcí na incidenty. Jednotlivé fáze jsou uvedeny v následujícím diagramu:

![Podezřelá aktivita](./media/security-center-planning-and-operations-guide/security-center-planning-and-operations-guide-fig5-1.png)

> [!NOTE]
> Jako referenci při sestavování vlastního plánu můžete využít příručku [Computer Security Incident Handling Guide](http://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-61r2.pdf) organizace National Institute of Standards and Technology (NIST).
> 
> 

Výstrahy služby Security Center můžete použít během následujících fází:

* **Detekce**: Identifikace podezřelé aktivity v jednom nebo několika prostředcích. 
* **Posouzení**: Počáteční zhodnocení a získávání dalších informací o podezřelé aktivitě.
* **Diagnóza**: Technické vyřešení problému podle nápravných kroků.

Každá výstraha zabezpečení poskytuje informace, které umožňují lepší pochopení povahy útoku a navrhují možné kroky ke zmírnění dopadu. Některé výstrahy obsahují i odkazy na další informace nebo další zdroje informací v Azure. Poskytnuté informace můžete využít k dalšímu výzkumu a nápravným krokům.

Následující příklad ukazuje probíhající podezřelé aktivity protokolu RDP:

![Podezřelá aktivita](./media/security-center-planning-and-operations-guide/security-center-planning-and-operations-guide-fig5-ga.png)

Jak vidíte, toto okno obsahuje podrobné informace o čase útoku, zdrojovému názvu hostitele a cílovém virtuálním počítači a obsahuje také doporučené kroky. Za některých okolností může být sekce informací o zdroji útoku prázdná. Další informace o tomto typu chování najdete v článku [Chybějící informace o zdroji ve výstrahách služby Azure Security Center](https://blogs.msdn.microsoft.com/azuresecurity/2016/03/25/missing-source-information-in-azure-security-center-alerts/).

Ve videu [How to Leverage the Azure Security Center & Microsoft Operations Management Suite for an Incident Response](https://channel9.msdn.com/Blogs/Taste-of-Premier/ToP1703) (Jak reagovat na incidenty pomocí služeb Azure Security Center a Microsoft Operations Management Suite) najdete ukázky, které vám pomůžou pochopit, jak je možné v jednotlivých fázích využít službu Security Center.

> [!NOTE]
> Přečtěte si téma [Využití Azure Security Center při reakci na incidenty](security-center-incident-response.md), abyste se dozvěděli informace o použití funkcí Security Center během procesu Reakce na incidenty. 
> 
> 

## <a name="see-also"></a>Viz také
V tomto dokumentu jste zjistili, jak naplánovat přechod na službu Security Center. Pokud se o službě Security Center chcete dozvědět víc, pročtěte si tato témata:

* [Správa a zpracování výstrah zabezpečení ve službě Azure Security Center](security-center-managing-and-responding-alerts.md)
* [Sledování stavu zabezpečení v Azure Security Center](security-center-monitoring.md) – Naučte se sledovat stav svých prostředků Azure.
* [Sledování partnerských řešení pomocí Azure Security Center](security-center-partner-solutions.md) – Zjistěte, jak pomocí Azure Security Center sledovat stav vašich partnerských řešení.
* [Azure Security Center – nejčastější dotazy](security-center-faq.md) – Přečtěte si nejčastější dotazy o použití této služby.
* [Blog o zabezpečení Azure](http://blogs.msdn.com/b/azuresecurity/) – Přečtěte si příspěvky o zabezpečení Azure a dodržování předpisů.


