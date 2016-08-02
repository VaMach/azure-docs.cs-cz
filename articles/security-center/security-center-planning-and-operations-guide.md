<properties
   pageTitle="Průvodce plánováním a provozem služby Security Center | Microsoft Azure"
   description="Tento dokument vám pomůže s plánováním před přijetím řešení Azure Security Center a s vyřešením aspektů každodenního provozu."
   services="security-center"
   documentationCenter="na"
   authors="YuriDio"
   manager="swadhwa"
   editor=""/>

<tags
   ms.service="security-center"
   ms.topic="hero-article" 
   ms.devlang="na"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="04/25/2016"
   ms.author="yurid"/>
 
# Průvodce plánováním a provozem služby Azure Security Center
Tento průvodce je určený odborníkům na informační technologie (IT), IT architektům, analytikům zabezpečení informací a správcům cloudů, jejichž společnosti hodlají začít používat Azure Security Center.

> [AZURE.NOTE] Informace v tomto dokumentu se týkají verze Preview služby Azure Security Center.

## Přehled Azure Security Center
Azure Security Center pomáhá předcházet hrozbám, zjišťovat je a reagovat na ně a nabízí lepší přehled o zabezpečení prostředků Azure a kontrolu nad nimi. Poskytuje integrované bezpečnostní sledování a správu zásad ve vašich předplatných, pomáhá zjišťovat hrozby, kterých byste si jinak nevšimli, a spolupracuje s řadou řešení zabezpečení. 

V tématu [Azure Security Center – nejčastější dotazy](security-center-faq.md) najdete seznam běžných dotazů, které vám můžou pomoct i ve fázi návrhu a plánování.

## Průvodce plánováním
Tato příručka pojednává o krocích a úkolech, které můžete provést, abyste přizpůsobili použití služby Azure Security Center požadavkům na zabezpečení a modelu správy cloudu ve vaší organizaci. Pokud chcete využívat všech výhod služby Azure Security Center, je důležité pochopit, jak budou různí jednotlivci nebo týmy ve vaší organizaci službu používat k tomu, aby vyhověli požadavkům na zabezpečený vývoj a provoz, sledování, řízení a reakce na incidenty. Při plánování použití Azure Security Center je potřeba zvážit tyto klíčové oblasti:

- Role zabezpečení a řízení přístupu
- Zásady a doporučení zabezpečení
- Shromažďování dat a úložiště
- Průběžné sledování zabezpečení 
- Reakce na incidenty

V další části se dozvíte, jak každou z těchto oblastí naplánovat a jak tato doporučení uplatnit v kombinaci s vašimi požadavky. 

## Role zabezpečení a řízení přístupu 

V závislosti na velikosti a struktuře vaší organizace můžou službu Security Center používat různí jednotlivci a týmy a provádět v ní různé úkony týkající se zabezpečení. Následuje příklad zahrnující fiktivní osoby a jejich příslušné role a povinnosti v oblasti zabezpečení:

![Role](./media/security-center-planning-and-operations-guide/security-center-planning-and-operations-guide-fig01.png)

Azure Security Center umožňuje těmto osobám tyto různé povinnosti plnit. Příklad:

**Jeff (vlastník úloh v cloudu)**

- Prohlíží a provádí doporučení služby Security Center na portálu Azure. 
- Může taky sledovat změny pomocí systému lístků (zadává údaje do doporučení pomocí rozhraní API).

**Rex (ředitel zabezpečení informací / ředitel IT)**

- Prohlíží sestavy služby Security Center z Power BI nebo Excelu.

**David (zabezpečení IT)**

- Nastavuje zásady zabezpečení a kontroluje stav zabezpečení na portálu Azure.
- Analyzuje data a generuje sestavy v Power BI. 

**Sam (operace zabezpečení)**

- Kontroluje a třídí výstrahy Security Center na portálu Azure. 
- Může používat existující řídicí panel (zadává údaje do výstrah pomocí rozhraní API).

**Sherlock (analytik zabezpečení)**

- Kontroluje výstrahy Security Center na portálu Azure. 
- Může používat existující řídicí panel (zadává údaje do výstrah pomocí rozhraní API).
- Analyzuje trendy výstrah v Power BI. 
- Kontroluje protokoly událostí z úložiště.

Azure Security Center používá [řízení přístupu na základě rolí (RBAC)](../active-directory/role-based-access-control-configure.md). To poskytuje [předdefinované role](../active-directory/role-based-access-built-in-roles.md), které se dají v Azure přiřadit uživatelům, skupinám a službám. Když uživatel otevře Azure Security Center, uvidí jenom informace týkající se prostředků, ke kterým má přístup, což znamená, že má uživatel přiřazenou roli Vlastník, Přispěvatel nebo Čtenář pro předplatné nebo skupinu prostředků, do které daný prostředek patří. Pokud vezmeme v úvahu výše uvedené osoby, byly by potřeba tyto role RBAC:

**Jeff (vlastník úloh v cloudu)**

- Vlastník/spolupracovník skupiny prostředků

**David (zabezpečení IT)**

- Vlastník/spolupracovník předplatného

**Sam (operace zabezpečení)**

- Čtenář předplatného s oprávněním zobrazovat výstrahy
- Vlastník/spolupracovník předplatného potřebný k rušení výstrah

**Sherlock (analytik zabezpečení)**

- Čtenář předplatného s oprávněním zobrazovat výstrahy
- Vlastník/spolupracovník předplatného potřebný k nápravě nebo rušení výstrah
- Může potřebovat přístup k úložišti

Další informace, které je potřeba vzít v úvahu:

- Zásady zabezpečení můžou upravovat jenom vlastníci a přispěvatelé odběru.
- Doporučení zabezpečení pro určitý prostředek můžou uplatňovat jenom vlastníci a přispěvatelé předplatného a skupiny prostředků.

Při plánování řízení přístupu pomocí RBAC pro Azure Security Center je potřeba si dobře rozmyslet, kdo z vaší organizace bude Azure Security Center používat jaké druhy úkonů bude provádět. Potom nakonfigurujte RBAC odpovídajícím způsobem. 

> [AZURE.NOTE] Doporučujeme přiřadit uživatelům tu nejvíc omezenou roli, kterou ke své práci potřebují. Třeba uživatelům, kteří potřebují jenom zobrazovat informace o stavu zabezpečení prostředků, ale nemusí provádět žádné kroky, třeba uplatňovat doporučení nebo upravovat zásady, by se měla přiřadit role Čtenář.

## Zásady a doporučení zabezpečení
Zásady zabezpečení definují sadu ovládacích prvků doporučenou pro prostředky v rámci daného předplatného nebo skupiny prostředků. V Azure Security Center určíte zásady na základě toho, jaké má vaše společnost požadavky na zabezpečení, a podle typu aplikací nebo citlivosti dat. 

Zásady povolené na úrovni předplatného se automaticky šíří do všech skupin prostředků v rámci tohoto předplatného, jak ukazuje následující diagram: 

![Zásady zabezpečení](./media/security-center-planning-and-operations-guide/security-center-planning-and-operations-guide-fig2.png) 

Jak ukazuje předchozí obrázek, zásady zabezpečení pro skupiny prostředků je možné zdědit z úrovně předplatného. 

V některých scénářích, kdy můžete mít ve skupině prostředků prostředky, které vyžadují jinou sadu zásad, můžete dědičnost zásad zakázat a u určité skupiny prostředků použít vlastní zásady. 

Pokud potřebujete mít v určitých skupinách prostředků vlastní zásady, zakažte v těchto skupinách prostředků dědičnost a změňte zásady zabezpečení. Pokud třeba máte některé úlohy, které nevyžadují zásadu transparentního šifrování dat SQL, vypněte tuto zásadu na úrovni předplatného a povolte ji jenom ve skupinách prostředků, ve kterých je potřeba.
 
Když začnete pro různé skupiny prostředků vytvářet vlastní zásady, měli byste nasazení zásad naplánovat s vědomím, že v případě konfliktu zásad (mezi zásadami předplatného a zásadami skupiny prostředků) mají přednost zásady skupiny prostředků. 

> [AZURE.NOTE] Pokud potřebujete zkontrolovat, které zásady jste změnily, můžete použít [protokoly auditu Azure](https://blogs.msdn.microsoft.com/cloud_solution_architect/2015/03/10/audit-logs-for-azure-events/). V protokolech auditu jsou zaznamenané všechny změny zásad.

### Doporučení zabezpečení

Než začnete konfigurovat zásady zabezpečení, měli byste zkontrolovat všechna [doporučení zabezpečení](security-center-recommendations.md) a zjistit, jestli jsou dané zásady vhodné pro vaše předplatné a skupiny prostředků. Je také důležité pochopit, jaké kroky je potřeba k uplatnění doporučení zabezpečení provést.

**Endpoint Protection** (Ochrana koncových bodů): Pokud virtuální počítač nemá povolené žádné řešení ochrany koncových bodů, Azure Security Center vám doporučí, abyste nějaké nainstalovali. Pokud upřednostňujete určité řešení ochrany koncových bodů, které už jste na místním pracovišti zavedli, budete se muset rozhodnout, jestli stejný antimalwarový software použijete i pro virtuální počítače Azure. Azure Security Center nabízí několik možností ochrany koncových bodů.  Můžete použít bezplatnou službu Microsoft Antimalware nebo si vybrat ze seznamu řešení ochrany koncových bodů od integrovaných partnerů. Další informace o tom, jak pomocí Azure Security Center nasadit antimalwarové řešení, najdete v tématu [Povolení antimalwarového řešení v Azure Security Center](security-center-enable-antimalware.md).

**System Updates** (Aktualizace systému): Azure Security Center vyhledá virtuální počítače, kterým chybí aktualizace zabezpečení nebo kritické aktualizace operačního systému. Zamyslete se nad tím, kdo bude zodpovídat za instalaci potřebných aktualizací a jak se budou aktualizace instalovat. Řada organizací používá službu WSUS, Windows Update nebo jiný nástroj.

**Baseline Configurations** (Standardní hodnoty konfigurace): Pokud konfigurace operačního systému virtuálních počítačů neodpovídají doporučeným standardním hodnotám, zobrazí se doporučení. Sadu standardních hodnot si můžete projít [tady](https://gallery.technet.microsoft.com/Azure-Security-Center-a789e335). Zvažte, jak se budou konfigurace operačních systémů uplatňovat. 

**Disk Encryption** (Šifrování disku): Pokud máte virtuální počítače s nešifrovanými disky, Azure Security Center doporučí, abyste použili službu Azure Disk Encryption, která s využitím nástroje BitLocker pro Windows a DM-Crypt pro Linux zajišťuje šifrování disků s operačním systémem a daty. Toto doporučení vás přesměruje na [podrobného průvodce](security-center-disk-encryption.md), který obsahuje pokyny k zašifrování. 

Upozorňujeme, že existuje několik různých scénářů, ve kterých šifrování probíhá. Při plánování budete muset vzít v úvahu jedinečné požadavky každého z těchto scénářů:

- Šifrování nových virtuálních počítačů Azure z virtuálních pevných disků, které jste zašifrovali pomocí vlastních šifrovacích klíčů
- Šifrování nových virtuálních počítačů Azure, které jste vytvořili z galerie Azure
- Šifrování virtuálních počítačů Azure, které už v Azure běží

Požadavky na plánování se u každého z těchto scénářů liší. Podrobnosti o každém z těchto scénářů najdete v [dokumentu white paper služby Azure Disk Encryption](https://gallery.technet.microsoft.com/Azure-Disk-Encryption-for-a0018eb0).

**Web Application Firewall** (Firewall webových aplikací): Azure Security Center vyhledá virtuální počítače, na kterých běží webové aplikace, a doporučí vám instalaci firewallu webových aplikací. Vyhodnoťte, které z dostupných partnerských řešení vaší organizaci nejvíc vyhovuje, a určete, jak k tomuto řešení získáte licenci (partneři můžou podporovat model použití vlastní licence nebo model průběžných plateb). Další informace o tom, jak pomocí Azure Security Center nasadit na virtuální počítače Azure bránu firewall webových aplikací, najdete v tématu [Přidání brány firewall webových aplikací v Azure Security Center](security-center-add-web-application-firewall.md).

**Virtual Networking** (Virtuální sítě): Azure Security Center vyhodnotí infrastrukturu a konfiguraci [Azure Virtual Network](https://azure.microsoft.com/documentation/services/virtual-network/) a zkontroluje, jestli je se používá [skupina zabezpečení sítě](../virtual-network/virtual-networks-nsg.md) a jestli má správně nakonfigurovaná pravidla pro příchozí přenosy dat. Měli byste zvážit, jaká pravidla pro přenosy dat je potřeba určit, a tyto požadavky sdělit osobám, které budou uplatňovat příslušná doporučení zabezpečení.

## Shromažďování dat a úložiště

Důrazně doporučujeme zapnout pro každé z vašich předplatných shromažďování dat, tím totiž zajistíte dostupnost sledování zabezpečení pro všechny virtuální počítače. Shromažďování dat se povoluje pomocí nástroje Azure Monitoring Agent (ASMAgentLauncher.exe) a rozšíření Azure Security Monitoring (ASMMonitoringAgent.exe). 

Rozšíření Azure Security Monitoring prochází různé položky konfigurace v oblasti zabezpečení a shromažďuje protokoly zabezpečení z daného virtuálního počítače. Tato data se odesílají na účet úložiště, který zadáte. Na virtuální počítač se nainstaluje taky správce procházení (ASMSoftwareScanner.exe), který bude vyhledávat opravy.

Když v zásadě zabezpečení povolíte shromažďování dat, agent sledování a rozšíření se automaticky nainstalují na všechny stávající a nově podporované virtuální počítače zřízené v Azure.  Proces agenta je neinvazivní a nemá vliv na výkon virtuálního počítače.
 
Pokud někdy budete chtít shromažďování dat vypnout, můžete to udělat v zásadě zabezpečení. Pokud chcete odstranit dřív nasazené agenty sledování, vyberte v nabídce možnost Delete Agents (Odstranit agenty).

> [AZURE.NOTE] V tématu [Azure Security Center – nejčastější dotazy](security-center-faq.md) najdete seznam podporovaných virtuálních počítačů.

Pro každou oblast, ve které máte spuštěné virtuální počítače, si zvolíte účet úložiště, na který se mají ukládat data shromážděná z daných virtuálních počítačů. Pokud pro každou oblast nevyberete účet úložiště, vytvoří se vám automaticky. Pro každou oblast můžete vybrat umístění úložiště nebo uložit všechny informace do centrálního umístění. Zatímco zásady zabezpečení se dají nastavit na úrovni předplatného Azure a na úrovni skupiny prostředků, oblast pro účet úložiště můžete nastavit jenom na úrovni předplatného.

Pokud používáte účet úložiště, který je sdílený mezi různými prostředky Azure, nezapomeňte si přečíst článek [Škálovatelnost a cíle výkonnosti služby Azure Storage](../storage/storage-scalability-targets.md). V něm najdete další informace o limitech velikosti a dalších omezeních. Vaše předplatné má také nastavené limity účtu úložiště. Jejich podrobnější vysvětlení najdete v článku [Omezení a kvóty předplatného a služeb Azure](../azure-subscription-service-limits).

> [AZURE.NOTE] Náklady spojené s tímto úložištěm nejsou součástí ceny služby Azure Security Center a budou se účtovat zvlášť podle běžného [ceníku úložiště Azure](https://azure.microsoft.com/pricing/details/storage/). 

Je taky potřeba naplánovat aspekty výkonu a škálovatelnosti na základě velikosti vašeho prostředí Azure a prostředků, které váš účet úložiště využívají. Další informace najdete v článku [Kontrolní seznam výkonu a škálovatelnosti služby Microsoft Azure Storage](../storage/storage-performance-checklist.md).

## Průběžné sledování zabezpečení

Po počáteční konfiguraci a uplatnění doporučení služby Azure Security Center je dalším krokem zvážení provozních procesů Azure Security Center. 

Na portálu Azure můžete ke službě Azure Security Center získat přístup tak, že kliknete na **Procházet** a do pole **Filtrovat** zadáte **Security Center**. Zobrazí se položky na základě filtrů, které uživatel vybere.

Azure Security Center nebude ovlivňovat vaše běžné provozní postupy, bude jenom pasivně sledovat vaše nasazení a poskytovat doporučení na základě zásad zabezpečení, které povolíte.

Řídicí panel Azure Security Center je rozdělený na dvě hlavní části: 

- Prevention (Prevence) 
- Detection (Detekce) 

Když ve službě Azure Security Center poprvé povolíte shromažďování dat ze svého stávajícího prostředí Azure, nezapomeňte si projít všechna doporučení. To můžete udělat v okně **Recommendations** (Doporučení) nebo u jednotlivých prostředků – **Virtual Machine** (Virtuální počítač), **Networking** (Sítě), **SQL** a **Application** (Aplikace). 

Až vyřešíte všechna doporučení, v části **Prevention** (Prevence) by měly být všechny vyřešené prostředky označené zeleně. Průběžné sledování je od této chvíle jednodušší, protože budete provádět kroky jenom na základě změn stavu zabezpečení prostředků a dlaždic doporučení.

Část **Detection** (Detekce) vyžaduje víc reakcí, obsahuje totiž výstrahy týkající se problémů, které jsou aktuální nebo nastaly v minulosti, kdy je zjistily ovládací prvky služby Azure Security Center a systémy jiných výrobců. Na dlaždici Security Alerts (Výstrahy zabezpečení) se zobrazí pruhové grafy znázorňující počet výstrah o zjištěných hrozbách, které v určitém dni našly, a o jejich přiřazení do různých kategorií závažnosti (nízká, střední, vysoká). Další informace o výstrahách zabezpečení najdete v tématu [Správa a zpracování výstrah zabezpečení v Azure Security Center](security-center-managing-and-responding-alerts.md). 

> [AZURE.NOTE] Také můžete využít řešení Microsoft Power BI, které umožňuje znázornění dat ze služby Azure Security Center. Přečtěte si článek [Přehledné znázornění dat z Azure Security Center v řešení Power BI](security-center-powerbi.md).

### Sledování nových nebo změněných prostředků

Většina prostředí Azure je dynamická a pravidelně v nich probíhá přidávání a odebírání prostředků, konfigurace a další změny. Azure Security Center pomáhá zajistit přehled o stavu zabezpečení těchto nových prostředků.

Když do prostředí Azure přidáte nové prostředky (virtuální počítače, databáze SQL), Azure Security Center tyto prostředky automaticky zjistí a začne sledovat jejich zabezpečení. Pokud je v zásadě zabezpečení povolené shromažďování dat, pro vaše virtuální počítače se automaticky povolí další funkce sledování. 

![Klíčové oblasti](./media/security-center-planning-and-operations-guide/security-center-planning-and-operations-guide-fig3.png) 

1.  K dlaždici **Resource security health** (Stav zabezpečení prostředků) pro virtuální počítače získáte přístup kliknutím na **Virtual Machines** (Virtuální počítače). Případné problémy s povolováním shromažďování dat nebo související doporučení se zobrazí v části **Monitoring Recommendations** (Doporučení pro sledování).
2.  V části **Recommendations** (Doporučení) zjistíte, jestli se u nového prostředku zjistila nějaká rizika zabezpečení.
3.  Je velmi běžné, že když se do vašeho prostředí přidají nové virtuální počítače, nejdřív mají nainstalovaný jen operační systém. Vlastníkovi prostředku může nějakou dobu trvat, než nasadí další aplikace, které budou tyto virtuální počítače používat.  V ideálním případě byste měli znát konečný záměr této úlohy. Bude se jednat o aplikační server? Podle toho, co k čemu bude tato nová úloha sloužit, můžete povolit vhodnou **zásadu zabezpečení**, což je třetí krok v tomto pracovním postupu.
4.  Během přidávání nových prostředků do prostředí Azure je možné, že se na dlaždici **Security Alerts** (Výstrahy zabezpečení) objeví nové výstrahy. Vždycky zkontrolujte, jestli na této dlaždici nejsou nové výstrahy, a proveďte příslušné kroky na základě doporučení služby Azure Security Center.

Taky je dobré pravidelně sledovat stav stávajících prostředků, abyste zjistili, které změny konfigurace vyvolaly bezpečnostní rizika, odlišily se od doporučených standardních hodnot nebo způsobily vydání výstrah zabezpečení. Začněte na řídicím panelu Azure Security Center. Jsou tři hlavní oblasti, které byste měli pravidelně kontrolovat.

![Provoz](./media/security-center-planning-and-operations-guide/security-center-planning-and-operations-guide-fig4.png) 

1.  Panel **Resource security health** (Stav zabezpečení prostředků) nabízí rychlý přístup ke klíčovým prostředkům. Pomocí této možnost můžete sledovat virtuální počítače, sítě, SQL a aplikace. 
2.  Panel **Recommendations** (Doporučení) umožňuje kontrolovat doporučení služby Azure Security Center. Při průběžném sledování můžete zjistit, že se vám doporučení nezobrazují každý den, což je normální, protože jste všechna doporučení zjistili při počátečním nastavování služby Azure Security Center. Z toho důvodu se vám v této části nemusí každý den zobrazovat nové informace, takže ji musíte otevírat, jenom když je to nutné.
3.  Panel **Detection** (Detekce) se může měnit velmi často i velmi zřídka. Vždy zkontrolujte výstrahy zabezpečení a proveďte akce na základě doporučení služby Azure Security Center. 

## Reakce na incidenty

Azure Security Center vyhledává nové hrozby a upozorňuje vás na ně. Organizace by měly výstrahy zabezpečení aktivně sledovat a provádět potřebné kroky, aby útok podrobněji prošetřily nebo napravily.

> [AZURE.NOTE] Cílem tohoto článku není pomoct vám s vytvořením plánu reakcí na incidenty. Jako referenci při sestavování vlastního plánu můžete využít příručku [Computer Security Incident Handling Guide](http://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-61r2.pdf) organizace National Institute of Standards and Technology (NIST).

Každá výstraha zabezpečení poskytuje informace, které umožňují lepší pochopení povahy útoku a navrhují možné kroky ke zmírnění dopadu. Některé výstrahy obsahují i odkazy na další informace nebo další zdroje informací v Azure. Poskytnuté informace můžete využít k dalšímu výzkumu a nápravným krokům.

Následující příklad ukazuje probíhající podezřelé aktivity protokolu RDP:

![Podezřelá aktivita](./media/security-center-planning-and-operations-guide/security-center-planning-and-operations-guide-fig5.png)

Jak vidíte, toto okno obsahuje podrobné informace o čase útoku, zdrojovému názvu hostitele a cílovém virtuálním počítači a obsahuje také doporučené kroky. Za některých okolností může být sekce informací o zdroji útoku prázdná. Další informace o tomto typu chování najdete v článku [Chybějící informace o zdroji ve výstrahách služby Azure Security Center](https://blogs.msdn.microsoft.com/azuresecurity/2016/03/25/missing-source-information-in-azure-security-center-alerts/).

| **Úroveň**        | **Detekce útoku**                     | **Navrhované kroky ke zmírnění**                                                                       |
|------------------|------------------------------------------|-----------------------------------------------------------------------------------------------|
| Síť          | Detekce útoku hrubou silou v protokolu RDP                | Omezte prostor pro útoky odebráním nepotřebných otevřených koncových bodů, konfigurací seznamů řízení přístupu, použitím silného hesla. |
| Virtuální počítače | Spuštění SVCHOST z nesprávného adresáře | Přesuňte virtuální počítač do jiné podsítě (izolovaný režim), zkontrolujte ho a znovu ho vytvořte.                        |
| Aplikace      | Injektáž SQL v databázi Azure          | Posilte konfiguraci databáze.                                                                |



## Další kroky
V tomto dokumentu jste zjistili, jak ve službě Security Center konfigurovat zásady zabezpečení. Pokud se o službě Security Center chcete dozvědět víc, pročtěte si tato témata:

- [Sledování stavu zabezpečení v Azure Security Center](security-center-monitoring.md) – Naučte se monitorovat stav svých prostředků Azure.
- [Azure Security Center – nejčastější dotazy](security-center-faq.md) – Přečtěte si nejčastější dotazy o použití této služby.
- [Blog o zabezpečení Azure](http://blogs.msdn.com/b/azuresecurity/) – Přečtěte si příspěvky o zabezpečení Azure a dodržování předpisů.



<!--HONumber=Jun16_HO2-->


