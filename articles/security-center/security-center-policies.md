---
title: "Nastavení zásad zabezpečení ve službě Azure Security Center | Dokumentace Microsoftu"
description: "Tento dokument vám pomůže s konfigurací zásad zabezpečení ve službě Azure Security Center."
services: security-center
documentationcenter: na
author: YuriDio
manager: swadhwa
editor: 
ms.assetid: 3b9e1c15-3cdb-4820-b678-157e455ceeba
ms.service: security-center
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/03/2017
ms.author: yurid
translationtype: Human Translation
ms.sourcegitcommit: 2f03ba60d81e97c7da9a9fe61ecd419096248763
ms.openlocfilehash: bd2291129a1a61f69e83cb76748d00b9ede6eb6f
ms.lasthandoff: 04/18/2017


---
# <a name="set-security-policies-in-azure-security-center"></a>Nastavení zásad zabezpečení ve službě Azure Security Center
Tento dokument vám pomůže nakonfigurovat zásady zabezpečení v Security Center a provede vás potřebnými kroky.

## <a name="what-are-security-policies"></a>Co jsou zásady zabezpečení?
Zásady zabezpečení definují sadu ovládacích prvků doporučenou pro prostředky v rámci daného předplatného nebo skupiny prostředků. V Security Center určíte zásady pro vaše předplatná Azure nebo skupinu prostředků na základě toho, jaké má vaše společnost požadavky na zabezpečení, a podle typu aplikací nebo citlivosti dat v každém předplatném.

Například prostředky používané pro vývoj nebo testování mohou mít jiné požadavky na zabezpečení než prostředky, které se používají v aplikacích v produkčním prostředí. Aplikace pracující s regulovanými daty, třeba s osobními údaji, zase mohou vyžadovat vyšší úroveň zabezpečení. Zásady zabezpečení povolené ve službě Azure Security Center mají vliv na doporučení zabezpečení a monitorování a pomohou vám najít potenciální nedostatky zabezpečení a zmírnit hrozby. Pro další informace o určení, která možnost je pro vás vhodná, si přečtěte [Průvodce plánováním a provozem služby Azure Security Center](security-center-planning-and-operations-guide.md).

## <a name="set-security-policies-for-subscriptions"></a>Nastavení zásad zabezpečení pro předplatné
Zásady zabezpečení můžete nakonfigurovat pro každé předplatné nebo skupinu prostředků. Pokud chcete určitou zásadu zabezpečení upravit, musíte mít roli vlastníka nebo přispěvatele daného předplatného. Přihlaste se na webu Azure Portal a při konfiguraci zásad zabezpečení ve službě Security Center postupujte podle následujících kroků:

1. Na řídicím panelu Security Center klikněte na dlaždici **Zásady**.
2. V okně **Zásady zabezpečení – Určete zásadu pro předplatné nebo skupinu prostředků**, které se otevře, vyberte předplatné, pro které chcete zásadu zabezpečení povolit. Pokud chcete zásadu zabezpečení povolit pouze pro určitou skupinu prostředků, ne pro celé předplatné, přejděte dolů na další část, která se věnuje nastavení zásad zabezpečení pro skupiny prostředků.

    ![Určení zásady](./media/security-center-policies/security-center-policies-fig1-ga.png)
3. Otevře se okno **Zásady zabezpečení** pro dané předplatné, které obsahuje podobné možnosti jako následující snímek obrazovky:

    ![Povolení shromažďování dat](./media/security-center-policies/security-center-policies-fig2-ga.png)

    V tomto okně jsou k dispozici následující možnosti:

   * **Zásady prevence**: Tuto možnost použijte, chcete-li konfigurovat zásady pro konkrétní předplatné nebo skupinu prostředků.  
   * **E-mailové oznámení**: Tuto možnost použijte, pokud chcete nakonfigurovat odesílání e-mailového oznámení při prvním výskytu výstrahy během dne a u výstrah s vysokou závažností. Předvolby e-mailu lze konfigurovat pouze pro zásady předplatného. Další informace o postupu při konfigurování e-mailových oznámení najdete v tématu [Zadání podrobností o kontaktu zabezpečení do služby Azure Security Center](security-center-provide-security-contact-details.md).
   * **Cenová úroveň**: Tuto možnost použijte pro upgrade výběru cenové úrovně. Další informace o možnostech cen najdete na [stránce cen služby Security Center](security-center-pricing.md).
4. Ujistěte se, že je u položky **Shromažďovat data z virtuálních počítačů** vybraná možnost **Zapnuto**. Tato možnost povolí automatické shromažďování protokolů u stávajících a nových prostředků.

   > [!NOTE]
   > Doporučujeme zapnout pro každé z vašich předplatných shromažďování dat, aby se zajistila dostupnost sledování zabezpečení pro všechny stávající i nové virtuální počítače. Pokud povolíte shromažďování dat, nainstaluje se agent sledování. Pokud teď na tomto místě nechcete shromažďování dat zapnout, můžete to udělat později v zobrazení **Stav** a **Doporučení**. Shromažďování dat můžete také povolit pouze pro předplatné nebo pro vybrané virtuální počítače. Další informace o podporovaných virtuálních počítačích najdete v článku [Azure Security Center – nejčastější dotazy](security-center-faq.md).
   >
   >
5. Pokud ještě nemáte nakonfigurovaný účet úložiště, může se vám při otevření okna **Zásady zabezpečení** zobrazit upozornění podobné následujícímu snímku obrazovky. Pokud nevyberete účet úložiště pro každou oblast, vytvoří se automaticky.

    ![Výběr úložiště](./media/security-center-policies/security-center-policies-fig2.png)
6. Pokud se vám zobrazí toto upozornění, klikněte na tuto možnost a vyberte oblast, jak je znázorněno na následujícím snímku obrazovky:

    ![Výběr úložiště](./media/security-center-policies/security-center-policies-fig3-ga.png)
7. Pro každou oblast, ve které máte spuštěné virtuální počítače, zvolte účet úložiště, na který se mají ukládat data shromážděná z daných virtuálních počítačů. To usnadňuje uchování dat ve stejné geografické oblasti za účelem ochrany osobních údajů a suverenity dat. Jakmile se rozhodnete, jakou oblast budete používat, vyberte ji a poté zvolte účet úložiště.
8. V okně **Vyberte účty úložiště** klikněte na **OK**.

   > [!NOTE]
   > Pokud chcete, můžete v jednom centrálním účtu úložiště shromažďovat data z virtuálních počítačů v různých oblastech. Další informace najdete v článku [Azure Security Center – nejčastější dotazy](security-center-faq.md).
   >
   >
9. V okně **Zásady zabezpečení** kliknutím na **Zapnout** povolte doporučení zabezpečení, která chcete v tomto předplatném používat. Po kliknutí na **Zásady prevence** se zobrazí možnosti jako na následujícím snímku obrazovky:

    ![Výběr zásad zabezpečení](./media/security-center-policies/security-center-policies-fig4-ga-new.png)

Následující tabulku použijte jako referenci pro pochopení jednotlivých možností:

| Zásada | Pokud je nastavená možnost Zapnuto |
| --- | --- |
| Aktualizace systému |Načte denní seznam dostupných aktualizací zabezpečení a důležitých aktualizací z webu Windows Update nebo ze služby Windows Server Update Services. Načtený seznam závisí na službě nakonfigurované pro daný virtuální počítač a doporučuje použití chybějících aktualizací. V systémech Linux využívá zásada k určení balíčků, pro které jsou dostupné aktualizace, systém správy balíčků, který je součástí dané distribuce. Také kontroluje aktualizace zabezpečení a důležité aktualizace z virtuálních počítačů služby [Azure Cloud Services](../cloud-services/cloud-services-how-to-configure.md). |
| Ohrožení zabezpečení operačního systému |Denně analyzuje konfigurace operačního systému k určení problémů, které by mohly způsobit zranitelnost virtuálních počítačů vůči útoku. Zásada také doporučuje změny v konfiguraci pro odstranění těchto zranitelností. Další informace o tom, jaké konkrétní konfigurace se monitorují, najdete v [seznamu doporučených standardních hodnot](https://gallery.technet.microsoft.com/Azure-Security-Center-a789e335). (V tomto okamžiku není Windows Server 2016 plně podporovaný.) |
| Ochrana koncových bodů |Doporučuje zřízení ochrany koncových bodů pro všechny virtuální počítače s Windows, aby se mohly identifikovat a odstraňovat viry, spyware a další škodlivý software. |
| Šifrování disku |Doporučuje povolit šifrování disku ve všech virtuálních počítačích pro zvýšení ochrany dat při nečinnosti. |
| Skupiny zabezpečení sítě |Doporučuje konfiguraci [skupin zabezpečení sítě](../virtual-network/virtual-networks-nsg.md), které řídí příchozí a odchozí přenosy dat do virtuálních počítačů s veřejnými koncovými body. Pokud neurčíte jinak, skupiny zabezpečení sítě nakonfigurované pro určitou podsíť se budou dědit do všech síťových rozhraní virtuálního počítače. Kromě kontroly toho, jestli je nakonfigurovaná skupina zabezpečení sítě, tato zásada také vyhodnocuje pravidla zabezpečení příchozích dat a zjišťuje pravidla, která povolují příchozí přenosy dat. |
| Brána firewall webových aplikací |Doporučuje zřízení brány firewall webových aplikací na virtuálních počítačích, pokud je splněna jedna z následujících podmínek:</br></br>Používá se [veřejná IP adresa na úrovni instance](../virtual-network/virtual-networks-instance-level-public-ip.md) (LPIP)a pravidla zabezpečení příchozích dat pro přidruženou skupinu zabezpečení sítě jsou nastavena, aby povolovala přístup k portu 80 a 443.</br></br>Používá se IP adresa s vyrovnáváním zatížení a přidružené vyrovnávání zatížení i pravidla překladu adres příchozích dat jsou nastavené, aby povolovaly přístup k portu 80 a 443. Další informace najdete v tématu [Podpora nástroje pro vyrovnávání zatížení v Azure Resource Manageru](../load-balancer/load-balancer-arm.md). |
| Brána firewall příští generace |Rozšiřuje ochranu sítě nad rámec skupin zabezpečení sítě, které jsou integrované v Azure. Security Center bude zjišťovat nasazení, pro která se doporučuje brána firewall příští generace, a umožní vám zřídit virtuální zařízení. |
| Auditování SQL a zjišťováním hrozeb |Doporučuje, abyste povolili auditování přístupu ke službě Azure Database za účelem vyšetřování dodržování předpisů a také rozšířeného zjišťování hrozeb. |
| Transparentní šifrování dat SQL |Doporučuje povolení neuplatněného šifrování pro služby Azure SQL Database, přidružené zálohy a soubory protokolů transakcí. I v případě, že dojde k porušení zabezpečení vašich dat, nebudou čitelná. |
| Posouzení ohrožení zabezpečení |Doporučuje, abyste na vašem virtuálním počítači nainstalovali řešení posouzení ohrožení zabezpečení. |

Až nakonfigurujete všechny možnosti, klikněte na **OK** v okně **Zásady zabezpečení**, ve kterém se zobrazují doporučení, a poté v okně **Zásady zabezpečení**, které obsahuje počáteční nastavení, klikněte na **Uložit**.

## <a name="set-security-policies-for-resource-groups"></a>Nastavení zásad zabezpečení pro skupiny prostředků
Pokud raději chcete nakonfigurovat zásady zabezpečení pro různé skupiny prostředků, použijete podobný postup jako při nastavování zásad pro předplatné. Hlavní rozdíl spočívá v tom, že budete muset rozbalit název předplatného a vybrat skupinu prostředků, pro kterou chcete nakonfigurovat jedinečnou zásadu zabezpečení:

![Výběr skupiny prostředků](./media/security-center-policies/security-center-policies-fig5-ga.png)

Až vyberete skupinu prostředků, otevře se okno **Zásady zabezpečení**. Ve výchozím nastavení je možnost **Dědičnost** povolená. To znamená, že se všechny zásady zabezpečení pro tuto skupinu prostředků dědí z úrovně předplatného. Pokud chcete pro určitou skupinu prostředků nastavit vlastní zásady zabezpečení, můžete tuto konfiguraci změnit. Pokud to chcete udělat, vyberte možnost **Jedinečné** a u položky **Zásada prevence** proveďte změny.

![Zásada zabezpečení pro skupinu prostředků](./media/security-center-policies/security-center-policies-fig6-ga.png)

> [!NOTE]
> V případě konfliktu mezi zásadou na úrovni předplatného a zásadou na úrovni skupiny prostředků má přednost zásada na úrovni prostředků.
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

