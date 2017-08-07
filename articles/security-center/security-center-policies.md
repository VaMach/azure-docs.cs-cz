---
title: "Nastavení zásad zabezpečení ve službě Azure Security Center | Dokumentace Microsoftu"
description: "Tento dokument vám pomůže s konfigurací zásad zabezpečení ve službě Azure Security Center."
services: security-center
documentationcenter: na
author: YuriDio
manager: mbaldwin
editor: 
ms.assetid: 3b9e1c15-3cdb-4820-b678-157e455ceeba
ms.service: security-center
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/27/2017
ms.author: yurid
ms.translationtype: HT
ms.sourcegitcommit: 54774252780bd4c7627681d805f498909f171857
ms.openlocfilehash: f4e3f74ce3f342eecf633cd748e2b7b21b2ccdd2
ms.contentlocale: cs-cz
ms.lasthandoff: 07/27/2017

---
# <a name="set-security-policies-in-azure-security-center"></a>Nastavení zásad zabezpečení ve službě Azure Security Center
Tento dokument vám pomůže nakonfigurovat zásady zabezpečení v Security Center a provede vás potřebnými kroky.

>[!NOTE] 
>Od začátku června 2017 používá Security Center ke shromažďování a ukládání dat agenta Microsoft Monitoring Agent. Další informace najdete v článku o [migraci platformy pro Azure Security Center](security-center-platform-migration.md). Informace v tomto článku představují funkce služby Security Center po přechodu na agenta Microsoft Monitoring Agent.
>

## <a name="what-are-security-policies"></a>Co jsou zásady zabezpečení?
Zásady zabezpečení definují sadu ovládacích prvků doporučenou pro prostředky v rámci daného předplatného. V Security Center určíte zásady pro svoje předplatná Azure na základě toho, jaké má vaše společnost požadavky na zabezpečení, a podle typu aplikací nebo citlivosti dat v každém předplatném.

Například prostředky používané pro vývoj nebo testování mohou mít jiné požadavky na zabezpečení než prostředky, které se používají v aplikacích v produkčním prostředí. Aplikace pracující s regulovanými daty, třeba s osobními údaji, zase mohou vyžadovat vyšší úroveň zabezpečení. Zásady zabezpečení povolené ve službě Azure Security Center mají vliv na doporučení zabezpečení a monitorování a pomohou vám najít potenciální nedostatky zabezpečení a zmírnit hrozby. Pro další informace o určení, která možnost je pro vás vhodná, si přečtěte [Průvodce plánováním a provozem služby Azure Security Center](security-center-planning-and-operations-guide.md).

## <a name="set-security-policies"></a>Nastavení zásad zabezpečení
Zásady zabezpečení můžete nakonfigurovat pro každé předplatné. Pokud chcete určitou zásadu zabezpečení upravit, musíte mít roli vlastníka nebo přispěvatele daného předplatného. Přihlaste se na webu Azure Portal a při konfiguraci zásad zabezpečení ve službě Security Center postupujte podle následujících kroků:

1. Na řídicím panelu Security Center klikněte na dlaždici **Zásady**.
2. V okně Zásady zabezpečení, které se otevře, vyberte předplatné, pro které chcete zásadu zabezpečení povolit.

    ![Určení zásady](./media/security-center-policies/security-center-policies-fig1-ga.png)
3. Pro vybrané předplatné se otevře okno **Zásady zabezpečení**, které obsahuje sadu možností. V tomto okně jsou k dispozici následující možnosti:

   * **Zásady prevence:** Tuto možnost použijte, pokud chcete konfigurovat zásady pro konkrétní předplatné.  
   * **E-mailové oznámení**: Tuto možnost použijte, pokud chcete nakonfigurovat odesílání e-mailového oznámení při prvním výskytu výstrahy během dne a u výstrah s vysokou závažností. Předvolby e-mailu lze konfigurovat pouze pro zásady předplatného. Další informace o postupu při konfigurování e-mailových oznámení najdete v tématu [Zadání podrobností o kontaktu zabezpečení do služby Azure Security Center](security-center-provide-security-contact-details.md).
   * **Cenová úroveň**: Tuto možnost použijte pro upgrade výběru cenové úrovně. Další informace o možnostech cen najdete na [stránce cen služby Security Center](security-center-pricing.md).
4. Ujistěte se, že je u položky **Shromažďovat data z virtuálních počítačů** vybraná možnost **Zapnuto**. Tato možnost umožňuje automatické shromažďování protokolů u existujících i nových prostředků pomocí agenta Microsoft Monitoring Agent – to je stejný agent, kterého používá Operations Management Suite a služba Log Analytics. Data shromážděná z tohoto agenta se ukládají v existujících pracovních prostorech Log Analytics přidružených k vašemu předplatnému Azure nebo v nových pracovních prostorech s ohledem na geografickou polohu virtuálního počítače.

5. V okně **Zásady zabezpečení** kliknutím na **Zásady prevence** zobrazte dostupné možnosti. Kliknutím na **Zapnuto** povolte doporučení pro zabezpečení, která jsou pro toto předplatné relevantní.

    ![Výběr zásad zabezpečení](./media/security-center-policies/security-center-policies-fig7.png)

Následující tabulku použijte jako referenci pro pochopení jednotlivých možností:

| Zásada | Pokud je nastavená možnost Zapnuto |
| --- | --- |
| Aktualizace systému |Načte denní seznam dostupných aktualizací zabezpečení a důležitých aktualizací z webu Windows Update nebo ze služby Windows Server Update Services. Načtený seznam závisí na službě nakonfigurované pro daný virtuální počítač a doporučuje použití chybějících aktualizací. V systémech Linux využívá zásada k určení balíčků, pro které jsou dostupné aktualizace, systém správy balíčků, který je součástí dané distribuce. Také kontroluje aktualizace zabezpečení a důležité aktualizace z virtuálních počítačů služby [Azure Cloud Services](../cloud-services/cloud-services-how-to-configure.md). |
| Ohrožení zabezpečení operačního systému |Denně analyzuje konfigurace operačního systému k určení problémů, které by mohly způsobit zranitelnost virtuálních počítačů vůči útoku. Zásada také doporučuje změny v konfiguraci pro odstranění těchto ohrožení zabezpečení. Další informace o tom, jaké konkrétní konfigurace se monitorují, najdete v [seznamu doporučených standardních hodnot](https://gallery.technet.microsoft.com/Azure-Security-Center-a789e335). (V tomto okamžiku není Windows Server 2016 plně podporovaný.) |
| Ochrana koncových bodů |Doporučuje zřízení ochrany koncových bodů pro všechny virtuální počítače s Windows, aby se mohly identifikovat a odstraňovat viry, spyware a další škodlivý software. |
| Šifrování disku |Doporučuje povolit šifrování disku ve všech virtuálních počítačích pro zvýšení ochrany dat při nečinnosti. |
| Skupiny zabezpečení sítě |Doporučuje konfiguraci [skupin zabezpečení sítě](../virtual-network/virtual-networks-nsg.md), které řídí příchozí a odchozí přenosy dat do virtuálních počítačů s veřejnými koncovými body. Pokud neurčíte jinak, skupiny zabezpečení sítě nakonfigurované pro určitou podsíť se dědí do všech síťových rozhraní virtuálních počítačů. Kromě kontroly toho, jestli je nakonfigurovaná skupina zabezpečení sítě, tato zásada také vyhodnocuje pravidla zabezpečení příchozích dat a zjišťuje pravidla, která povolují příchozí přenosy dat. |
| Brána firewall webových aplikací |Doporučuje zřízení brány firewall webových aplikací na virtuálních počítačích, pokud je splněna jedna z následujících podmínek: </br></br>Používá se [veřejná IP adresa na úrovni instance](../virtual-network/virtual-networks-instance-level-public-ip.md) (LPIP)a pravidla zabezpečení příchozích dat pro přidruženou skupinu zabezpečení sítě jsou nastavena, aby povolovala přístup k portu 80 a 443.</br></br>Používá se IP adresa s vyrovnáváním zatížení a přidružené vyrovnávání zatížení i pravidla překladu adres příchozích dat jsou nastavené, aby povolovaly přístup k portu 80 a 443. Další informace najdete v tématu [Podpora nástroje pro vyrovnávání zatížení v Azure Resource Manageru](../load-balancer/load-balancer-arm.md). |
| Brána firewall příští generace |Rozšiřuje ochranu sítě nad rámec skupin zabezpečení sítě, které jsou integrované v Azure. Security Center bude zjišťovat nasazení, pro která se doporučuje brána firewall příští generace, a umožní vám zřídit virtuální zařízení. |
| Auditování SQL a zjišťováním hrozeb |Doporučuje, abyste povolili auditování přístupu ke službě Azure Database za účelem vyšetřování dodržování předpisů a také rozšířeného zjišťování hrozeb. |
| Šifrování SQL |Doporučuje povolení neuplatněného šifrování pro služby Azure SQL Database, přidružené zálohy a soubory protokolů transakcí. I v případě, že dojde k porušení zabezpečení vašich dat, nebudou čitelná. |
| Posouzení ohrožení zabezpečení |Doporučuje, abyste na vašem virtuálním počítači nainstalovali řešení posouzení ohrožení zabezpečení. |
| Šifrování služby Storage |Tato funkce je aktuálně dostupná pro Soubory a objekty blob Azure. Po povolení šifrování služby Storage budou šifrována pouze nová data a veškeré stávající soubory v účtu úložiště zůstanou nezašifrované. |
| Síťový přístup JIT |Pokud je povolený přístup JIT (právě včas), Security Center uzamkne příchozí provoz do vašich virtuálních počítačů Azure vytvořením pravidla NSG. Na virtuálním počítači vyberete porty, na které má být příchozí provoz uzamčen. Další informace najdete v tématu popisujícím [správu přístupu k virtuálním počítačům pomocí metody právě včas](https://docs.microsoft.com/azure/security-center/security-center-just-in-time). |

Až nakonfigurujete všechny možnosti, klikněte na **OK** v okně **Zásady zabezpečení**, ve kterém se zobrazují doporučení, a poté v okně **Zásady zabezpečení**, které obsahuje počáteční nastavení, klikněte na **Uložit**.

> [!NOTE]
> Cenová úroveň se pro úroveň skupiny prostředků i nadále použitelná. Další informace najdete na [stránce s cenami](https://azure.microsoft.com/pricing/details/security-center/).
>
>

## <a name="see-also"></a>Viz také
V tomto dokumentu jste zjistili, jak ve službě Azure Security Center konfigurovat zásady zabezpečení. Pokud se o službě Azure Security Center chcete dozvědět víc, pročtěte si tato témata:

* [Průvodce plánováním a provozem služby Azure Security Center](security-center-planning-and-operations-guide.md). Zjistěte, jak naplánovat a pochopit aspekty návrhu, abyste mohli přejít na Azure Security Center.
* [Monitorování stavu zabezpečení ve službě Azure Security Center](security-center-monitoring.md). Zjistěte, jak monitorovat stav svých prostředků Azure.
* [Správa a zpracování výstrah zabezpečení ve službě Azure Security Center](security-center-managing-and-responding-alerts.md). Zjistěte, jak spravovat a zpracovávat výstrahy zabezpečení.
* [Monitorování partnerských řešení pomocí služby Azure Security Center](security-center-partner-solutions.md). Zjistěte, jak monitorovat stav partnerských řešení.
* [Azure Security Center – nejčastější dotazy](security-center-faq.md). Přečtěte si nejčastější dotazy o použití této služby.
* [Blog o zabezpečení Azure](http://blogs.msdn.com/b/azuresecurity/). Přečtěte si příspěvky o zabezpečení Azure a dodržování předpisů.

