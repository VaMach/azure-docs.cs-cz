---
title: "Průvodce plánováním a provozem služby Security Center | Dokumentace Microsoftu"
description: "Tento dokument vám pomůže s plánováním před přijetím řešení Azure Security Center a s vyřešením aspektů každodenního provozu."
services: security-center
documentationcenter: na
author: YuriDio
manager: mbaldwin
editor: 
ms.assetid: f984e4a2-ac97-40bf-b281-2f7f473494c4
ms.service: security-center
ms.topic: hero-article
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/16/2017
ms.author: yurid
ms.translationtype: Human Translation
ms.sourcegitcommit: ff2fb126905d2a68c5888514262212010e108a3d
ms.openlocfilehash: c502e4363dbaa37455d1aad90d1e9fa855fd09b0
ms.contentlocale: cs-cz
ms.lasthandoff: 06/17/2017


---
# <a name="azure-security-center-planning-and-operations-guide"></a>Průvodce plánováním a provozem služby Azure Security Center
Tento průvodce je určený odborníkům na informační technologie (IT), IT architektům, analytikům zabezpečení informací a správcům cloudů, jejichž společnosti hodlají začít používat Azure Security Center.

>[!NOTE] 
>Od začátku června 2017 bude Security Center používat ke shromažďování a ukládání dat agenta Microsoft Monitoring Agent. Další informace najdete v článku o [migraci platformy pro Azure Security Center](security-center-platform-migration.md). Informace v tomto článku představují funkce služby Security Center po přechodu na agenta Microsoft Monitoring Agent.
>

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

## <a name="security-roles-and-access-controls"></a>Role zabezpečení a řízení přístupu
V závislosti na velikosti a struktuře vaší organizace můžou službu Security Center používat různí jednotlivci a týmy a provádět v ní různé úkony týkající se zabezpečení. Následující diagram ukazuje příklad zahrnující fiktivní osoby a jejich příslušné role a povinnosti v oblasti zabezpečení:

![Role](./media/security-center-planning-and-operations-guide/security-center-planning-and-operations-guide-fig01-new.png)

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
* Pracuje na nápravě s vlastníkem úlohy v cloudu 

Security Center používá [řízení přístupu na základě rolí (RBAC)](../active-directory/role-based-access-control-configure.md). To poskytuje [předdefinované role](../active-directory/role-based-access-built-in-roles.md), které se dají v Azure přiřadit uživatelům, skupinám a službám. Když uživatel otevře službu Security Center, uvidí jenom informace související s prostředky, ke kterým má přístup. To znamená, že uživateli je přiřazena role Vlastníka, Přispěvatele nebo Čtenáře předplatného nebo skupiny prostředků, do které prostředek patří. Kromě těchto rolí existují ve službě Security Center dvě specifické role:

- **Čtenář zabezpečení:** uživatel patřící do této role může zobrazovat práva ke službě Security Center, což zahrnuje doporučení, výstrahy, zásady a stav, ale nemůže provádět změny.
- **Správce zabezpečení:** stejné jako čtenář zabezpečení, ale uživatel může také aktualizovat zásady zabezpečení a rušit doporučení a výstrahy.

Výše popsané role služby Security Center nemají přístup k jiným oblastem služeb Azure, jako je služba Storage, web a mobilní zařízení nebo Internet věcí.  

> [!NOTE]
> Aby uživatel mohl zobrazit Security Center v Azure, musí být alespoň vlastníkem nebo přispěvatelem skupiny prostředků předplatného. 
> 
> 

Použití osob uvedených v předchozím diagramu vyžaduje následující nastavení RBAC:

**Jeff (vlastník úloh v cloudu)**

* Vlastník/spolupracovník skupiny prostředků

**David (zabezpečení IT)**

* Vlastník/spolupracovník předplatného nebo správce zabezpečení

**Judy (pracovnice oddělení zabezpečení)**

* Čtenář předplatného nebo čtenář zabezpečení s oprávněním zobrazovat výstrahy
* Vlastník/spolupracovník předplatného nebo správce zabezpečení potřebný k rušení výstrah

**Sam (analytik zabezpečení)**

* Čtenář předplatného s oprávněním zobrazovat výstrahy
* Vlastník/spolupracovník předplatného potřebný k rušení výstrah
* Může být vyžadován přístup k pracovnímu prostoru

Další informace, které je potřeba vzít v úvahu:

* Zásady zabezpečení můžou upravovat jenom vlastníci/přispěvatelé předplatného a správci zabezpečení.
* Doporučení zabezpečení pro určitý prostředek můžou uplatňovat jenom vlastníci a přispěvatelé předplatného a skupiny prostředků.

Při plánování řízení přístupu pomocí RBAC pro Security Center je potřeba si dobře rozmyslet, kdo z vaší organizace bude Security Center používat. Ke správné konfiguraci RBAC je také důležité, jaké typy úloh budou uživatelé provádět.

> [!NOTE]
> Doporučujeme přiřadit uživatelům tu nejvíc omezenou roli, kterou ke své práci potřebují. Třeba uživatelům, kteří potřebují jenom zobrazovat informace o stavu zabezpečení prostředků, ale nemusí provádět žádné kroky, třeba uplatňovat doporučení nebo upravovat zásady, by se měla přiřadit role Čtenář.
> 
> 

## <a name="security-policies-and-recommendations"></a>Zásady a doporučení zabezpečení
Zásady zabezpečení definují sadu ovládacích prvků doporučenou pro prostředky v rámci daného předplatného. V Security Center určíte zásady na základě toho, jaké má vaše společnost požadavky na zabezpečení, a podle typu aplikací nebo citlivosti dat.

Zásady povolené na úrovni předplatného se automaticky šíří do všech skupin prostředků v rámci tohoto předplatného, jak ukazuje následující diagram:

![Zásady zabezpečení](./media/security-center-planning-and-operations-guide/security-center-planning-and-operations-guide-fig2-newUI.png)

> [!NOTE]
> Pokud potřebujete zkontrolovat, které zásady jste změnily, můžete použít [protokoly auditu Azure](https://blogs.msdn.microsoft.com/cloud_solution_architect/2015/03/10/audit-logs-for-azure-events/). V protokolech auditu jsou zaznamenané všechny změny zásad.
> 
> 

### <a name="security-recommendations"></a>Doporučení zabezpečení
Než začnete konfigurovat zásady zabezpečení, prostudujte si všechna [doporučení zabezpečení](security-center-recommendations.md) a zkontrolujte, jestli jsou dané zásady vhodné pro vaše předplatné a skupiny prostředků. Je také důležité pochopit, jaká akce by se měla provést pro řešení [Doporučení zabezpečení](https://docs.microsoft.com/en-us/azure/security-center/security-center-recommendations) a kdo z organizace bude zodpovědný za monitorování nových doporučení a provádění potřebných kroků.

Security Center vám doporučí, abyste ve svém předplatném Azure uvedli podrobnosti o kontaktu zabezpečení. Prostřednictvím tohoto kontaktu se na vás společnost Microsoft obrátí, pokud středisko Microsoft Security Response Center (MSRC) zjistí, že k datům zákazníka nezákonně nebo neoprávněně přistupovala třetí strana. Další informace o povolení tohoto doporučení najdete v článku [Zadání podrobností o kontaktu zabezpečení do Azure Security Center](security-center-provide-security-contact-details.md).

## <a name="data-collection-and-storage"></a>Shromažďování dat a úložiště
Azure Security Center používá agenta Microsoft Monitoring Agent – to je stejný agent, kterého používá Operations Management Suite a služba Log Analytics – ke shromažďování dat zabezpečení z virtuálních počítačů. Data shromážděná z tohoto agenta se budou ukládat v pracovních prostorech Log Analytics.

### <a name="agent"></a>Agent

Po povolení shromažďování dat v zásadách zabezpečení se agent Microsoft Monitoring Agent (pro [Windows](https://docs.microsoft.com/azure/log-analytics/log-analytics-windows-agents) nebo [Linux](https://docs.microsoft.com/azure/log-analytics/log-analytics-linux-agents)) nainstaluje na všech podporovaných a nově vytvořených virtuálních počítačích Azure.  Pokud je na virtuálním počítači agent Microsoft Monitoring Agent již nainstalovaný, Azure Security Center bude využívat stávajícího nainstalovaného agenta. Proces agenta je navržen tak, aby nenarušoval běžný chod a měl zcela minimální dopad na výkon virtuálního počítače.

Microsoft Monitoring Agent pro Windows vyžaduje použití portu TCP 443. Další podrobnosti najdete v [článku Řešení problémů](security-center-troubleshooting-guide.md).

Pokud někdy budete chtít shromažďování dat vypnout, můžete to udělat v zásadě zabezpečení. Protože však agenta Microsoft Monitoring Agent můžou využívat další služby pro správu a monitorování Azure, po vypnutí shromažďování dat ve službě Security Center se agent neodinstaluje automaticky. V případě potřeby můžete agenta odinstalovat ručně.

> [!NOTE]
> V tématu [Azure Security Center – nejčastější dotazy](security-center-faq.md) najdete seznam podporovaných virtuálních počítačů.
> 

### <a name="workspace"></a>Pracovní prostor

Data shromážděná z agenta Microsoft Monitoring Agent (jménem služby Azure Security Center) budou uložena v existujících pracovních prostorech Log Analytics přidružených k vašemu předplatnému Azure nebo v nových pracovních prostorech s ohledem na zeměpisné umístění virtuálního počítače. 

Na webu Azure Portal můžete procházením zobrazit seznam pracovních prostorů Log Analytics, včetně těch vytvořených službou Azure Security Center. Pro nové pracovní prostory se vytvoří související skupina prostředků. Budou se řídit těmito zásadami vytváření názvů: 

* Pracovní prostor: *DefaultWorkspace-[ID_předplatného]-[zeměpisné umístění]*
* Skupina prostředků: *DefaultResouceGroup-[zeměpisné_umístění]*

U pracovních prostorů vytvořených službou Azure Security Center se data uchovávají po dobu 30 dnů. U existujících pracovních prostorů uchovávání závisí na cenové úrovni pracovního prostoru.

> [!NOTE]
> Microsoft se pevně zavazuje, že soukromí a bezpečnost těchto dat bude chránit. Společnost Microsoft dodržuje přísné pokyny pro dodržování předpisů a zabezpečení – od psaní kódu po provoz služeb. Další informace o zpracování dat a ochraně osobních údajů najdete v článku [Zabezpečení dat ve službě Azure Security Center](security-center-data-security.md).
> 

## <a name="ongoing-security-monitoring"></a>Průběžné sledování zabezpečení
Po počáteční konfiguraci a uplatnění doporučení služby Security Center je dalším krokem zvážení provozních procesů Security Center.

Na portálu Azure můžete ke službě Security Center získat přístup tak, že kliknete na **Procházet** a do pole **Filtrovat** zadáte **Security Center**. Zobrazení, která uživatel získá, podléhají těmto použitým filtrům. Následující příklad ukazuje prostředí s mnoha problémy, které je potřeba řešit:

![řídicí panel](./media/security-center-planning-and-operations-guide/security-center-planning-and-operations-guide-fig6.png)

> [!NOTE]
> Security Center nebude ovlivňovat vaše běžné provozní postupy, bude jenom pasivně sledovat vaše nasazení a poskytovat doporučení na základě zásad zabezpečení, které povolíte.

Když se poprvé rozhodnete pro používání služby Security Center pro stávající prostředí Azure, nezapomeňte si projít všechna doporučení. To můžete udělat na dlaždici **Doporučení** nebo u jednotlivých prostředků (**Compute**, **Sítě**, **Úložiště a data**, **Aplikace**).

Až vyřešíte všechna doporučení, v části **Prevention** (Prevence) by měly být všechny vyřešené prostředky označené zeleně. Průběžné sledování je od této chvíle jednodušší, protože budete provádět kroky jenom na základě změn stavu zabezpečení prostředků a dlaždic doporučení.

Část **Detection** (Detekce) vyžaduje víc reakcí, obsahuje totiž výstrahy týkající se problémů, které jsou aktuální nebo nastaly v minulosti, kdy je zjistily ovládací prvky služby Security Center a systémy jiných výrobců. Na dlaždici Security Alerts (Výstrahy zabezpečení) se zobrazí pruhové grafy znázorňující počet výstrah o zjištěných hrozbách, které v určitém dni našly, a o jejich přiřazení do různých kategorií závažnosti (nízká, střední, vysoká). Další informace o výstrahách zabezpečení najdete v tématu [Správa a zpracování výstrah zabezpečení v Azure Security Center](security-center-managing-and-responding-alerts.md).

> [!NOTE]
> Také můžete využít řešení Microsoft Power BI, které umožňuje znázornění dat ze služby Security Center. Přečtěte si článek [Přehledné znázornění dat z Azure Security Center v řešení Power BI](security-center-powerbi.md).
> 
> 

### <a name="monitoring-for-new-or-changed-resources"></a>Sledování nových nebo změněných prostředků
Většina prostředí Azure je dynamická a pravidelně v nich probíhá přidávání a odebírání prostředků, konfigurace a další změny. Security Center pomáhá zajistit přehled o stavu zabezpečení těchto nových prostředků.

Když do prostředí Azure přidáte nové prostředky (virtuální počítače, databáze SQL), Security Center tyto prostředky automaticky zjistí a začne sledovat jejich zabezpečení. To zahrnuje také webové role a role pracovního procesu PaaS. Pokud je v [zásadě zabezpečení](security-center-policies.md) povolené shromažďování dat, pro vaše virtuální počítače se automaticky povolí další funkce sledování.

![Klíčové oblasti](./media/security-center-planning-and-operations-guide/security-center-planning-and-operations-guide-fig3-newUI.png)

1. Pro virtuální počítače klikněte na **Compute** v části **Prevence**. Případné problémy s povolováním shromažďování dat nebo související doporučení se zobrazí na kartě **Přehled** a v části **Doporučení pro monitorování**.
2. V části **Recommendations** (Doporučení) zjistíte, jestli se u nového prostředku zjistila nějaká rizika zabezpečení.
3. Je velmi běžné, že když se do vašeho prostředí přidají nové virtuální počítače, nejdřív mají nainstalovaný jen operační systém. Vlastníkovi prostředku může nějakou dobu trvat, než nasadí další aplikace, které budou tyto virtuální počítače používat.  V ideálním případě byste měli znát konečný záměr této úlohy. Bude se jednat o aplikační server? Podle toho, co k čemu bude tato nová úloha sloužit, můžete povolit vhodnou **zásadu zabezpečení**, což je třetí krok v tomto pracovním postupu.
4. Během přidávání nových prostředků do prostředí Azure je možné, že se na dlaždici **Security Alerts** (Výstrahy zabezpečení) objeví nové výstrahy. Vždycky zkontrolujte, jestli na této dlaždici nejsou nové výstrahy, a proveďte příslušné kroky na základě doporučení služby Security Center.

Taky je dobré pravidelně sledovat stav stávajících prostředků, abyste zjistili, které změny konfigurace vyvolaly bezpečnostní rizika, odlišily se od doporučených standardních hodnot nebo způsobily vydání výstrah zabezpečení. Začněte na řídicím panelu Security Center. Jsou tři hlavní oblasti, které byste měli pravidelně kontrolovat.

![Operace](./media/security-center-planning-and-operations-guide/security-center-planning-and-operations-guide-fig4-newUI.png)

1. Část **Prevence** nabízí rychlý přístup ke klíčovým prostředkům. Pomocí této možnosti můžete monitorovat službu Compute, Sítě, Úložiště a data a Aplikace.
2. Panel **Recommendations** (Doporučení) umožňuje kontrolovat doporučení služby Security Center. Při průběžném monitorování můžete zjistit, že se vám doporučení nezobrazují každý den, což je normální, protože jste všechna doporučení zjistili při počátečním nastavování služby Security Center. Z toho důvodu se vám v této části nemusí každý den zobrazovat nové informace, takže ji musíte otevírat, jenom když je to nutné.
3. Část **Detekce** se může měnit velmi často i velmi zřídka. Vždy zkontrolujte výstrahy zabezpečení a proveďte akce na základě doporučení služby Security Center.

## <a name="incident-response"></a>Reakce na incidenty
Security Center vyhledává nové hrozby a upozorňuje vás na ně. Organizace by měly výstrahy zabezpečení aktivně sledovat a provádět potřebné kroky, aby útok podrobněji prošetřily nebo napravily. Další informace o tom, jak detekce hrozeb služby Security Center pracuje, najdete v článku [Funkce detekce ve službě Azure Security Center](security-center-detection-capabilities.md).

Cílem tohoto článku není pomoct vám s vytvořením vlastního plánu reakcí na incidenty, postup uvedený v dokumentu Microsoft Azure Security Response in the Cloud (Reakce zabezpečení Microsoft Azure v cloudu) ale použijeme jako základ pro fáze reakcí na incidenty. Jednotlivé fáze jsou uvedeny v následujícím diagramu:

![Podezřelá aktivita](./media/security-center-planning-and-operations-guide/security-center-planning-and-operations-guide-fig5-1.png)

> [!NOTE]
> Jako referenci při sestavování vlastního plánu můžete využít příručku [Computer Security Incident Handling Guide](http://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-61r2.pdf) organizace National Institute of Standards and Technology (NIST).
> 

Výstrahy služby Security Center můžete použít během následujících fází:

* **Detekce**: Identifikace podezřelé aktivity v jednom nebo několika prostředcích. 
* **Posouzení**: Počáteční zhodnocení a získávání dalších informací o podezřelé aktivitě.
* **Diagnóza**: Technické vyřešení problému podle nápravných kroků.

Každá výstraha zabezpečení poskytuje informace, které umožňují lepší pochopení povahy útoku a navrhují možné kroky ke zmírnění dopadu. Některé výstrahy obsahují i odkazy na další informace nebo další zdroje informací v Azure. Poskytnuté informace můžete využít k dalšímu výzkumu a nápravným krokům. Můžete také prohledávat data související se zabezpečením, která jsou uložena ve vašem pracovním prostoru.

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


