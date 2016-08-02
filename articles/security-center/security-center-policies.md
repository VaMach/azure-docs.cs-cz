<properties
   pageTitle="Začínáme s Azure Security Center | Microsoft Azure"
   description="Tento dokument vám pomůže s konfigurací zásad zabezpečení ve službě Azure Security Center."
   services="security-center"
   documentationCenter="na"
   authors="YuriDio"
   manager="swadhwa"
   editor=""/>

<tags
   ms.service="security-center"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="06/03/2016"
   ms.author="yurid"/>

# Nastavení zásad zabezpečení v Azure Security Center
Tento dokument vám pomůže nakonfigurovat zásady zabezpečení v Azure Security Center a provede vás potřebnými kroky.

> [AZURE.NOTE] Informace v tomto dokumentu se týkají verze Preview služby Azure Security Center.

## Co je Azure Security Center?
Azure Security Center pomáhá předcházet hrozbám, zjišťovat je a reagovat na ně a nabízí lepší přehled o zabezpečení prostředků Azure a kontrolu nad nimi. Poskytuje integrované bezpečnostní sledování a správu zásad ve vašich předplatných, pomáhá zjišťovat hrozby, kterých byste si jinak nevšimli, a spolupracuje s řadou řešení zabezpečení.

## Co jsou zásady zabezpečení?
Zásady zabezpečení definují sadu ovládacích prvků doporučenou pro prostředky v rámci daného předplatného nebo skupiny prostředků. V Azure Security Center určíte zásady pro vaše předplatná Azure nebo skupinu prostředků na základě toho, jaké má vaše společnost požadavky na zabezpečení, a podle typu aplikací nebo citlivosti dat v každém předplatném.

Třeba prostředky používané pro vývoj nebo testování můžou mít jiné požadavky na zabezpečení než ty, které se používají v aplikacích v produkčním prostředí. Aplikace pracující s regulovanými daty, třeba s osobními údaji, zase můžou vyžadovat jinou úroveň zabezpečení. Zásady zabezpečení povolené ve službě Azure Security Center budou mít vliv na doporučení zabezpečení a monitorování a pomůžou vám najít potenciální nedostatky zabezpečení a zmírnit hrozby.

## Nastavení zásad zabezpečení pro předplatné

Zásady zabezpečení se dají nakonfigurovat pro každé předplatné nebo skupinu prostředků. Pokud chcete určitou zásadu zabezpečení upravit, musíte mít roli vlastníka nebo přispěvatele daného předplatného. Ke konfiguraci zásad zabezpečení v Azure Security Center použijte následující postup:

1. Na řídicím panelu Azure Security Center klikněte na dlaždici **Security Policy** (Zásada zabezpečení).

2. V okně **Security Policy - Define policy per subscription or resource group** (Zásada zabezpečení – určete zásadu pro předplatné nebo skupinu prostředků), které se otevře napravo, vyberte předplatné, pro které chcete zásadu zabezpečení povolit. Pokud chcete zásadu zabezpečení povolit jenom pro určitou skupinu prostředků, ne pro celé předplatné, přejděte dolů na další část, která se věnuje nastavení zásad zabezpečení pro skupiny prostředků.

    ![Určení zásady](./media/security-center-policies/security-center-policies-fig01.png)

3. Otevře se okno **Security policy** (Zásada zabezpečení) pro dané předplatné, které obsahuje podobné možnosti jako následující okno:

    ![Povolení shromažďování dat](./media/security-center-policies/security-center-policies-fig1-new.png)

4. Ujistěte se, že je u položky **Collect data from virtual machines** (Shromažďovat data z virtuálních počítačů) vybraná možnost **On** (Zapnuto). Tato možnost povolí automatické shromažďování protokolů u stávajících a nových prostředků. 

    >[AZURE.NOTE] Důrazně doporučujeme zapnout pro každé z vašich předplatných shromažďování dat, tím totiž zajistíte dostupnost sledování zabezpečení pro všechny stávající i nové virtuální počítače. Pokud povolíte shromažďování dat, nainstaluje se agent sledování. Pokud teď na tomto místě nechcete shromažďování dat zapnout, můžete to udělat později v zobrazení stavu a doporučení. Shromažďování dat můžete také povolit pro celé předplatné nebo pro vybrané virtuální počítače. Další informace o podporovaných virtuálních počítačích najdete v článku [Azure Security Center – nejčastější dotazy](security-center-faq.md).

5. Pokud ještě nemáte nakonfigurovaný účet úložiště, může se vám při otevření okna **Security Policy** (Zásada zabezpečení) zobrazit upozornění podobné tomuto obrázku:

    ![Výběr úložiště](./media/security-center-policies/security-center-policies-fig2.png)

6. Pokud se vám zobrazí toto upozornění, klikněte na tuto možnost a vyberte oblast, jak znázorňuje následující obrázek:

    ![Výběr úložiště](./media/security-center-policies/security-center-policies-fig3.png)

7. Pro každou oblast, ve které máte spuštěné virtuální počítače, si zvolte účet úložiště, na který se mají ukládat data shromážděná z daných virtuálních počítačů. To usnadňuje uchování dat ve stejné geografické oblasti za účelem ochrany osobních údajů a suverenity dat. Jakmile se rozhodnete, jakou oblast budete používat, vyberte ji a potom zvolte účet úložiště.

8. V okně **Choose storage accounts** (Vyberte účty úložiště) klikněte na **OK**.

    > [AZURE.NOTE] Pokud chcete, můžete v jednom centrálním účtu úložiště shromažďovat data z virtuálních počítačů v různých oblastech. Další informace najdete v článku [Azure Security Center – nejčastější dotazy](security-center-faq.md).

9. V okně **Security Policy** (Zásady zabezpečení) kliknutím na **On** (Zapnout) povolte doporučení zabezpečení, která chcete v tomto předplatném používat. Klikněte na možnost **Prevention policy** (Zásada prevence). Otevře se okno **Security Policy** (Zásada zabezpečení), jak znázorňuje následující obrázek. 

    ![Výběr zásad zabezpečení](./media/security-center-policies/security-center-policies-fig1-1-new2.png)

Použijte následující tabulku pro referenci, abyste pochopili, jakou mají jednotlivé možnosti funkci:

| Zásada | Pokud je nastavená možnost On (Zapnuto) |
|----- |-----|
| System Updates (Aktualizace systému) | Denně načítá seznam dostupných aktualizací zabezpečení a kritických aktualizací ze služby Windows Update nebo WSUS, podle toho, která služba je pro daný virtuální počítač nakonfigurovaná, a doporučuje instalaci chybějících aktualizací. |
| Baseline Rules (Pravidla standardních hodnot) | Denně analyzuje konfigurace operačních systémů, které by mohly způsobit větší zranitelnost virtuálního počítače vůči útokům, a doporučuje změny konfigurace, které tyto nedostatky zabezpečení řeší. Další informace o tom, jaké konkrétní konfigurace se sledují, najdete v [seznamu doporučených standardních hodnot](https://gallery.technet.microsoft.com/Azure-Security-Center-a789e335). |
| Endpoint Protection (Ochrana koncových bodů) | Doporučuje zřízení ochrany koncových bodů pro všechny virtuální počítače s Windows, aby se mohly identifikovat a odstraňovat viry, spyware a další škodlivý software. 
| Network Security Groups (Skupiny zabezpečení sítě) | Doporučuje konfiguraci [skupin zabezpečení sítě](../virtual-network/virtual-networks-nsg.md), které řídí příchozí a odchozí přenosy dat do podsítí a síťových rozhraní. Pokud neurčíte jinak, skupiny zabezpečení sítě nakonfigurované pro určitou podsíť se budou dědit do všech síťových rozhraní virtuálního počítače. Kromě kontroly toho, jestli je nakonfigurovaná skupina zabezpečení sítě, tato možnost také vyhodnocuje pravidla zabezpečení příchozích dat a zjišťuje, jestli existují pravidla, která povolují příchozí přenosy dat. |
| Web Application Firewall (Brána firewall webových aplikací) | Při splnění následujících podmínek doporučuje zřídit na virtuálních počítačích brány firewall webových aplikací: Používá se [veřejná IP adresa na úrovni instance](../virtual-network/virtual-networks-instance-level-public-ip.md) a příslušná pravidla zabezpečení příchozích dat ve skupině zabezpečení sítě jsou nastavená tak, aby umožňovala přístup k portu 80/443. Používá se IP adresa s vyrovnáváním zátěže a příslušná pravidla pro vyrovnávání zátěže a překlad příchozích adres jsou nastavená tak, aby umožňovala přístup k portu 80/443 (další informace najdete v článku [Podpora služby Load Balancer v Azure Resource Manageru](../load-balancer/load-balancer-arm.md)). |
| Next Generation Firewall (Brána firewall příští generace) | Tato zásada rozšiřuje ochranu sítě nad rámec skupin zabezpečení sítě, které jsou integrované v Azure. Služba Security Center bude zjišťovat nasazení, pro která se doporučuje brána firewall příští generace, a umožní vám zřídit virtuální zařízení. |
| SQL Auditing (Auditování SQL) | Doporučuje, abyste povolili auditování přístupu k SQL serverům a databázím Azure za účelem dodržování předpisů, rozšířeného zjišťování a vyšetřování. |
| SQL Transparent Data Encryption (Transparentní šifrování dat SQL) | Doporučí povolení neuplatněného šifrování pro všechny SQL databáze Azure, přidružené zálohy a soubory protokolů transakcí, aby data v případě porušení zabezpečení nebyla čitelná. |

11. Až nakonfigurujete všechny možnosti, klikněte na **OK** v okně **Security Policy** (Zásada zabezpečení), ve kterém se zobrazují doporučení, a v okně **Security Policy** (Zásada zabezpečení), které má počáteční nastavení, klikněte na **Save** (Uložit).

## Nastavení zásad zabezpečení pro skupiny prostředků

Pokud raději chcete nakonfigurovat zásady zabezpečení pro různé skupiny prostředků, použijete podobný postup jako při nastavování zásad pro předplatné. Hlavní rozdíl spočívá v tom, že budete muset rozbalit název předplatného a vybrat skupinu prostředků, pro kterou chcete nakonfigurovat jedinečnou zásadu:

![Výběr skupiny prostředků](./media/security-center-policies/security-center-policies-fig4.png)

Až vyberete skupinu prostředků, otevře se okno **Security Policy** (Zásada zabezpečení). Ve výchozím nastavení je povolená možnost **Inheritance** (Dědičnost), což znamená, že se všechny zásady zabezpečení pro tuto skupinu prostředků dědí z úrovně předplatného. Pokud chcete pro určitou skupinu prostředků nastavit vlastní zásady zabezpečení, můžete tuto konfiguraci změnit. Pokud to chcete udělat, vyberte možnost **Unique** (Jedinečné) a u položky **Prevention policy** (Zásada prevence) proveďte změny.

![Zásada zabezpečení pro skupinu prostředků](./media/security-center-policies/security-center-policies-fig5-new.png)

> [AZURE.NOTE] V případě konfliktu mezi zásadou na úrovni předplatného a zásadou na úrovni skupiny prostředků má přednost zásada na úrovni prostředků.


## Další kroky

V tomto dokumentu jste zjistili, jak ve službě Azure Security Center konfigurovat zásady zabezpečení. Pokud se o službě Azure Security Center chcete dozvědět víc, pročtěte si tato témata:

- [Průvodce plánováním a provozem služby Azure Security Center](security-center-planning-and-operations-guide.md) – Zjistěte, jak naplánovat a pochopit aspekty návrhu, abyste mohli přejít na Azure Security Center.
- [Sledování stavu zabezpečení v Azure Security Center](security-center-monitoring.md) – Naučte se monitorovat stav svých prostředků Azure.
- [Správa a zpracování výstrah zabezpečení v Azure Security Center](security-center-managing-and-responding-alerts.md) – Zjistěte, jak spravovat výstrahy zabezpečení a reagovat na ně.
- [Sledování partnerských řešení pomocí Azure Security Center](security-center-partner-solutions.md) – Zjistěte, jak pomocí Azure Security Center sledovat stav vašich partnerských řešení.
- [Azure Security Center – nejčastější dotazy](security-center-faq.md) – Přečtěte si nejčastější dotazy o použití této služby.
- [Blog o zabezpečení Azure](http://blogs.msdn.com/b/azuresecurity/) – Přečtěte si příspěvky o zabezpečení Azure a dodržování předpisů.



<!--HONumber=Jun16_HO2-->


