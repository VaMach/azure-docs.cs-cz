---
title: Co je Azure Security Center? | Microsoft Docs
description: "Můžete se dozvědět o službě Azure Security Center, jejích klíčových funkcích a způsobu práce."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 45b9756b-6449-49ec-950b-5ed1e7c56daa
ms.service: security-center
ms.devlang: na
ms.topic: overview
ms.custom: mvc
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/15/2018
ms.author: terrylan
ms.openlocfilehash: 08102ce4caead003925aa600f4f7f005b1c336e0
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/21/2018
---
# <a name="what-is-azure-security-center"></a>Co je Azure Security Center?
Azure Security Center zajišťuje jednotnou správu zabezpečení a pokročilou ochranu před hrozbami napříč hybridními cloudovými úlohami. Se službou Security Center můžete používat zásady zabezpečení napříč úlohami, omezit vystavení hrozbám a detekovat útoky a reagovat na ně.

Proč používat Security Center?

- **Centralizovaná správa zásad** – Zajistěte dodržování předpisů společnosti nebo soulad se zákonnými požadavky na zabezpečení díky centrální správě zásad zabezpečení napříč všemi hybridními cloudovými úlohami.
- **Nepřetržité posuzování zabezpečení** – Monitorujte zabezpečení počítačů, sítí, služeb úložiště, datových služeb a aplikací za účelem zjišťování potenciálních problémů se zabezpečením.
- **Užitečná doporučení** – Napravujte ohrožení zabezpečení dříve, než je budou moci zneužít útočníci, díky užitečným doporučením zabezpečení s určenou prioritou.
- **Pokročilá cloudová ochrana** – Omezte hrozby díky přístupu k portům pro správu podle potřeby a řízení aplikací spuštěných na virtuálních počítačích jejich přidáváním na seznam povolených.
- **Výstrahy a incidenty s určenou prioritou** – Zaměřte se nejprve na nejdůležitější hrozby díky výstrahám a incidentům zabezpečení.
- **Integrovaná řešení zabezpečení** – Shromažďujte, prohledávejte a analyzujte data o zabezpečení z nejrůznějších zdrojů, včetně propojených partnerských řešení.

Stránka **Security Center – Přehled** poskytuje rychlý přehled stavu zabezpečení úloh v Azure i mimo Azure a umožňuje tak zkoumat a posuzovat zabezpečení úloh a identifikaci a omezení rizika. Integrovaný řídicí panel poskytuje okamžitý přehled o výstrahách a ohroženích zabezpečení vyžadujících pozornost.

![Přehled][1]

## <a name="centralized-policy-management"></a>Centralizovaná správa zásad
Zásady zabezpečení definují požadovanou konfiguraci úloh a pomáhají zajišťovat dodržování předpisů společnosti nebo soulad se zákonnými požadavky na zabezpečení. Ve službě Security Center definujete zásady a upravujete je podle typu úlohy nebo citlivosti dat.

Zásady Security Center obsahují následující součásti:

- **Shromažďování dat:** Určuje zřizování agentů a nastavení [shromažďování dat](security-center-enable-data-collection.md) zabezpečení.
- **Zásady zabezpečení:** Upravením [zásad zabezpečení](security-center-policies.md) můžete určit, které ovládací prvky Security Center monitoruje a doporučuje.
- **E-mailová oznámení:** Určuje kontakty zabezpečení a nastavení [e-mailových oznámení](security-center-provide-security-contact-details.md).
- **Cenová úroveň:** Definujete [výběr cen](security-center-pricing.md) úrovně Free nebo Standard. Zvolená úroveň určí, které funkce Security Center budou dostupné pro prostředky v daném rozsahu.

![Zásady zabezpečení][2]

Další informace najdete v tématu [Přehled zásad zabezpečení](security-center-policies-overview.md).

## <a name="continuous-security-assessment"></a>Nepřetržité posuzování zabezpečení
Security Center analyzuje stav zabezpečení výpočetních prostředků, virtuálních sítí, služeb úložiště a datových služeb a aplikací. Nepřetržité posuzování pomáhá zjišťovat potenciální problémy se zabezpečením, jako jsou systémy s chybějícími aktualizacemi zabezpečení nebo zveřejněné síťové porty. Výběrem dlaždice v části Prevence zobrazíte další informace, včetně seznamu prostředků a všech identifikovaných ohrožení zabezpečení.

![Monitorování stavu zabezpečení][3]

Další informace najdete v tématu [Monitorování stavu zabezpečení](security-center-monitoring.md).

## <a name="actionable-recommendations"></a>Užitečná doporučení
Security Center analyzuje stav zabezpečení vašich prostředků Azure i mimo Azure a identifikuje potenciální ohrožení zabezpečení. Seznam doporučení zabezpečení s určenou prioritou vás provede procesem řešení problémů se zabezpečením.

![Doporučení][4]

Další informace najdete v tématu [Správa doporučení zabezpečení](security-center-recommendations.md).

## <a name="just-in-time-vm-access"></a>Přístup k virtuálním počítačům podle potřeby
Omezte plochu vystavenou síťovým útokům díky řízenému přístupu k portům pro správu virtuálních počítačů Azure podle potřeby a výrazně tak omezte vystavení útokům hrubou silou a jiným síťovým útokům.

![Přístup k virtuálním počítačům podle potřeby][5]

Určete pravidla způsobu, jakým se uživatelé můžou připojovat k virtuálním počítačům. V případě potřeby je o přístup možné zažádat ze služby Security Center nebo přes PowerShell. Pokud je žádost v souladu s pravidly, automaticky se udělí přístup po požadovanou dobu.

Další informace najdete v tématu [Správa přístupu k virtuálním počítačům pomocí metody Just-in-Time](security-center-just-in-time.md).

## <a name="adaptive-application-controls"></a>Adaptivní řízení aplikací
Blokujte malware a další nechtěné aplikace použitím doporučení pro přidání na seznam povolených upravených pro vaše konkrétní úlohy Azure a využívajících strojové učení.

![Adaptivní řízení aplikací][6]

Kontrolujte a kliknutím používejte doporučená pravidla přidávání aplikací na seznam povolených generovaná službou Security Center nebo upravujte již nakonfigurovaná pravidla.

Další informace najdete v tématu [Adaptivní řízení aplikací](security-center-adaptive-application.md).

## <a name="prioritized-alerts-and-incidents"></a>Výstrahy a incidenty s určenou prioritou
Security Center s využitím pokročilých analýz a globální analýzy hrozeb detekuje příchozí útoky a aktivitu po porušení zabezpečení. U výstrah se určuje priorita a seskupují se do incidentů, které pomáhají soustředit se nejprve na nejdůležitější hrozby. Můžete také vytvářet vlastní výstrahy zabezpečení.

![Výstrahy a incidenty s určenou prioritou][7]

Můžete rychle posoudit rozsah a dopad útoku s využitím vizuálního a interaktivního prostředí pro šetření a blíže zkoumat data zabezpečení s využitím předdefinovaných nebo ad hoc dotazů.

Další informace najdete v tématu [Správa a zpracování výstrah zabezpečení](security-center-managing-and-responding-alerts.md).

## <a name="integrate-your-security-solutions"></a>Integrace řešení zabezpečení
V Security Center můžete shromažďovat, prohledávat a analyzovat data zabezpečení z nejrůznějších zdrojů, včetně propojených partnerských řešení, jako jsou síťové brány firewall a další služby Microsoftu.

![Integrace řešení zabezpečení][8]

Další informace najdete v tématu [Integrace řešení zabezpečení](security-center-partner-integration.md).

## <a name="next-steps"></a>Další kroky

- Pokud chcete začít využívat Security Center, musíte mít předplatné Microsoft Azure. Pokud předplatné nemáte, můžete si zaregistrovat [bezplatnou zkušební verzi](https://azure.microsoft.com/free/).
- Cenová úroveň Free služby Security Center je povolená s vaším předplatným Azure. Pokud chcete využívat pokročilé možnosti správy zabezpečení a detekce hrozeb, musíte upgradovat na cenovou úroveň Standard. Úroveň Standard je po dobu prvních 60 dní zdarma. Další informace najdete na [stránce s cenami za Security Center](https://azure.microsoft.com/pricing/details/security-center/).
- Pokud už jste připraveni povolit Security Center, článek [Rychlý Start: Onboarding předplatného Azure na Security Center Standard](security-center-get-started.md) vás provede postupem.


<!--Image references-->
[1]: ./media/security-center-intro/overview.png
[2]: ./media/security-center-intro/security-policy.png
[3]: ./media/security-center-intro/compute.png
[4]: ./media/security-center-intro/recommendations.png
[5]: ./media/security-center-intro/just-in-time-vm-access.png
[6]: ./media/security-center-intro/adaptive-app-controls.png
[7]: ./media/security-center-intro/security-alerts.png
[8]: ./media/security-center-intro/security-solutions.png
