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
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/08/2018
ms.author: terrylan
ms.openlocfilehash: 50a54b8d2a73807aa9a0217f7ccf971b8c516494
ms.sourcegitcommit: 9a8b9a24d67ba7b779fa34e67d7f2b45c941785e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/08/2018
---
# <a name="what-is-azure-security-center"></a>Co je Azure Security Center?
Azure Security Center poskytuje správu jednotná zabezpečení a ochrana před internetovými útoky pokročilé v rámci úlohy hybridní cloud. S Centrem zabezpečení můžete použít zásady zabezpečení nad úlohy, omezit vaše ohrožení a zjistit a reagovat na útoky.

Proč používat Security Center?

- **Centralizovaná správa zásad** – zajistit zabezpečení zákonné požadavky nebo dodržování předpisů ve společnosti tím, že centrálně spravovat zásady zabezpečení přes všechny úlohy hybridní cloud.
- **Vyhodnocení průběžné zabezpečení** – monitorování zabezpečení počítače, sítě, úložiště a data služby a aplikace chcete zjistit potenciální potíže se zabezpečením.
- **Řešitelné doporučení** – napravit ohrožení zabezpečení, než může zneužít útočníkům doporučení zabezpečení seřazený podle priority a možné použít.
- **Rozšířené cloudu obrany** – snížit ohrožení s jenom při přístup k časovému porty pro správu a povolených pro řízení aplikací běžících na virtuálních počítačů.
- **Prioritu výstrah a incidentů** -zaměřit se na nejdůležitější hrozeb nejprve s nastavovat výstrahy zabezpečení a incidentů.
- **Integrované řešení zabezpečení** – shromažďování, vyhledávání a analyzovat data zabezpečení z různých zdrojů, včetně připojených partnerských řešení.

**Security Center – přehled** poskytuje rychlý pohled na postavení zabezpečení Azure a úlohy mimo Azure, umožňuje zjistit a vyhodnotit zabezpečení vašich úloh a identifikovat a zmírnění rizik. Předdefinovaný řídicí panel poskytuje okamžité přehledy o výstrahy zabezpečení a ohrožení zabezpečení, které vyžadují pozornost.

![Přehled][1]

## <a name="centralized-policy-management"></a>Centralizovaná správa zásad
Zásady zabezpečení definují požadovanou konfiguraci úloh a pomáhají zajišťovat dodržování předpisů společnosti nebo soulad se zákonnými požadavky na zabezpečení. V Centru zabezpečení definovat zásady a přizpůsobit pro váš typ úlohy, nebo citlivosti vašich data.

Zásady Security Center obsahují následující součásti:

- **Shromažďování dat**: Určuje agentem zřizování a zabezpečení [shromažďování dat](security-center-enable-data-collection.md) nastavení.
- **Zásady zabezpečení**: určit, který řídí monitorování Security Center a doporučuje úpravou [zásady zabezpečení](security-center-policies.md).
- **E-mailová oznámení**: Určuje zabezpečení kontakty a [e-mailová oznámení](security-center-provide-security-contact-details.md) nastavení.
- **Cenová úroveň**: Standard nebo definuje volné [ceny výběr](security-center-pricing.md). Vrstvy, které zvolíte určuje funkce Security Center, které jsou k dispozici pro prostředky v oboru.

![Zásady zabezpečení][2]

V tématu [přehled zásad zabezpečení](security-center-policies-overview.md) Další informace.

## <a name="continuous-security-assessment"></a>Vyhodnocení průběžné zabezpečení
Security Center analyzuje stav zabezpečení výpočetní prostředky, virtuální sítě, úložiště a datové služby a aplikace. Průběžné assessment vám umožní zjistit potenciální potíže se zabezpečením, jako je například systémy s chybějící aktualizací zabezpečení nebo zveřejněné síťové porty. Vyberte dlaždici v části prevence zobrazíte další informace, včetně seznamu prostředků a všechny chyby zabezpečení, které byly identifikovány.

![Sledování stavu zabezpečení][3]

V tématu [sledování stavu zabezpečení](security-center-monitoring.md) Další informace.

## <a name="actionable-recommendations"></a>Řešitelné doporučení
Security Center analyzuje stav zabezpečení svým Azure a prostředky mimo Azure, aby identifikovala potenciální ohrožení zabezpečení. Seznam seřazený podle priority zabezpečení doporučení vás provede procesem adresování problémy se zabezpečením.

![Doporučení][4]

V tématu [Správa doporučení zabezpečení](security-center-recommendations.md) Další informace.

## <a name="just-in-time-vm-access"></a>Přístup k virtuálnímu počítači podle potřeby
Omezte prostor pro útoky sítě se jenom na dobu, řízený přístup k portům správy na virtuálních počítačích Azure, zásadně snižuje vystavení hrubou silou a další síťovým útokům.

![Přístup k virtuálnímu počítači podle potřeby][5]

Určete pravidla, jak uživatelé se mohou připojit k virtuálním počítačům. V případě potřeby můžete ze služby Security Center nebo pomocí prostředí PowerShell požadován přístup. Tak dlouho, dokud žádost v souladu s pravidly, je automaticky udělí přístup k pro požadovaný čas.

V tématu [spravovat přístup k virtuálním počítačům pomocí jenom na dobu](security-center-just-in-time.md) Další informace.

## <a name="adaptive-application-controls"></a>Adaptivní řízení aplikací
Blokování malwaru a další nežádoucí aplikace s použitím povolených doporučení přizpůsobit pro konkrétní úlohy Azure a používá technologii strojové učení.

![Adaptivní řízení aplikací][6]

Zkontrolujte a klikněte na tlačítko použít pravidla povolených doporučené aplikace generované Security Center nebo upravit pravidla, které jsou již nakonfigurována.

V tématu [ovládací prvky adaptivní aplikace](security-center-adaptive-application.md) Další informace.

## <a name="prioritized-alerts-and-incidents"></a>Seřazený podle priority výstrah a incidentů
Security Center používá pokročilou analýzu a globální analýzou hrozeb ke zjišťování příchozí útoky a po porušení aktivit. Výstrahy jsou nastavovat a seskupeny do incidenty, tedy zvýšení nejprve zaměřit na nejdůležitější hrozeb. Můžete vytvořit také vlastní zabezpečení vlastní výstrahy.

![Seřazený podle priority výstrah a incidentů][7]

Můžete rychle vyhodnocovat na rozsah a dopad útoku, možnosti interaktivního šetření a použít předdefinované nebo ad hoc dotazy pro podrobnější zkoumání dat zabezpečení.

V tématu [Správa a zpracování výstrah zabezpečení](security-center-managing-and-responding-alerts.md) Další informace.

## <a name="integrate-your-security-solutions"></a>Integrovat řešení zabezpečení
Můžete shromažďovat, vyhledávání a analyzovat data zabezpečení z různých zdrojů, včetně připojených partnerských řešení, jako jsou brány firewall sítě a dalším službám Microsoft ve službě Security Center.

![Integrovat řešení pro zabezpečení][8]

V tématu [integrovat řešení zabezpečení](security-center-partner-integration.md) Další informace.

## <a name="next-steps"></a>Další postup

- Chcete-li začít využívat Security Center, potřebujete předplatné služby Microsoft Azure. Pokud nemáte předplatné, můžete si zaregistrovat [bezplatnou zkušební verzi](https://azure.microsoft.com/free/).
- Cenové úrovně Free Security Center je povolená s vaším předplatným Azure. Abyste mohli využívat pokročilé zabezpečení správy a možností detekce hrozeb, musíte provést upgrade na cenová úroveň Standard. Úroveň Standard je po dobu prvních 60 dní zdarma. Najdete v článku [Security Center stránce s cenami](https://azure.microsoft.com/pricing/details/security-center/) Další informace.
- Pokud budete chtít povolit Security Center standardní teď [rychlý start: zařadit předplatného Azure Security Center standardní](security-center-get-started.md) vás provede kroky.


<!--Image references-->
[1]: ./media/security-center-intro/overview.png
[2]: ./media/security-center-intro/security-policy.png
[3]: ./media/security-center-intro/compute.png
[4]: ./media/security-center-intro/recommendations.png
[5]: ./media/security-center-intro/just-in-time-vm-access.png
[6]: ./media/security-center-intro/adaptive-app-controls.png
[7]: ./media/security-center-intro/security-alerts.png
[8]: ./media/security-center-intro/security-solutions.png
