---
title: "Integrace zásad zabezpečení ve službě Azure Security Center se službou Azure Policy | Dokumentace Microsoftu"
description: "Tento dokument vám pomůže s konfigurací integrace zásad zabezpečení ve službě Azure Security Center se službou Azure Policy."
services: security-center
documentationcenter: na
author: YuriDio
manager: mbaldwin
editor: 
ms.assetid: cd906856-f4f9-4ddc-9249-c998386f4085
ms.service: security-center
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/13/2017
ms.author: yurid
ms.openlocfilehash: 5e07cd6891a5ab04012f819b5f6b9379312e530d
ms.sourcegitcommit: 5d772f6c5fd066b38396a7eb179751132c22b681
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/13/2017
---
# <a name="set-security-policies-in-security-center-powered-by-azure-policy"></a>Nastavení zásad zabezpečení, které používají službu Azure Policy, ve službě Security Center
Tento dokument vám pomůže nakonfigurovat zásady zabezpečení, které používají službu Azure Policy, ve službě Security Center a provede vás potřebnými kroky. 


## <a name="how-security-policies-work"></a>Jak fungují zásady zabezpečení?
Security Center automaticky vytváří výchozí zásady zabezpečení pro každé z vašich předplatných Azure. Zásady můžete upravovat ve službě Security Center nebo můžete pomocí služby [Azure Policy](http://docs.microsoft.com/azure/azure-policy/azure-policy-introduction) vytvářet nové definice zásad, přiřazovat zásady napříč skupinami pro správu (ty mohou představovat celou organizaci, obchodní jednotku atd.) a monitorovat dodržování předpisů těchto zásad.

> [!NOTE]
> Zásady Azure jsou ve verzi Limited Preview. Pokud se chcete připojit, klikněte [sem](https://aka.ms/getpolicy). Další informace o zásadách Azure najdete v tématu [Vytvoření a správa zásad pro vynucování dodržování předpisů](http://docs.microsoft.com/en-us/azure/azure-policy/create-manage-policy).

## <a name="edit-security-policies"></a>Upravení zásad zabezpečení
Ve službě Security Center můžete upravit výchozí zásady zabezpečení pro každé z vašich předplatných Azure. Pokud chcete upravit zásady zabezpečení, musíte být vlastníkem, přispěvatelem nebo správcem zabezpečení daného předplatného nebo skupiny pro správu, která je obsahuje. Přihlaste se na webu Azure Portal a postupujte podle následujících kroků pro zobrazení zásad zabezpečení ve službě Security Center:

1. Na řídicím panelu **Security Center** v části **Obecné** klikněte na **Zásady zabezpečení**.
2. Vyberte předplatné, pro které chcete zásady zabezpečení povolit.

    ![Správa zásad](./media/security-center-policies/security-center-policies-fig10.png)

3. V části **SOUČÁSTI ZÁSAD** klikněte na **Zásady zabezpečení**.

    ![Součásti zásad](./media/security-center-policies/security-center-policies-fig12.png)

4. Toto jsou výchozí zásady přiřazené službě Security Center prostřednictvím zásad Azure. Můžete odstranit položky, které jsou v části **ZÁSADY A PARAMETRY**, nebo přidat další definice zásad, které jsou v části **DOSTUPNÉ MOŽNOSTI**. Pokud to chcete udělat, stačí kliknout na symbol plus vedle názvu definice.

    ![Definice zásad](./media/security-center-policies/security-center-policies-fig11.png)

5. Pokud chcete podrobnější vysvětlení nějaké zásady, klikněte na ni a otevře se další stránka s podrobnostmi a kódem JSON, který obsahuje strukturu [definice zásady](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-policy/#policy-definition-structure):

    ![JSON](./media/security-center-policies/security-center-policies-fig14.png)

6. Jakmile budete s úpravami hotovi, klikněte na **Uložit**.


## <a name="available-security-policy-definitions"></a>Dostupné definice zásad zabezpečení

Následující tabulku můžete použít jako referenci pro pochopení definic zásad, které jsou k dispozici ve výchozích zásadách zabezpečení: 

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


## <a name="next-step"></a>Další krok
V tomto dokumentu jste zjistili, jak ve službě Security Center konfigurovat zásady zabezpečení. Pokud se o službě Security Center chcete dozvědět víc, pročtěte si tato témata:

* [Průvodce plánováním a provozem služby Azure Security Center](security-center-planning-and-operations-guide.md). Zjistěte, jak naplánovat a pochopit aspekty návrhu, abyste mohli přejít na Azure Security Center.
* [Monitorování stavu zabezpečení ve službě Azure Security Center](security-center-monitoring.md). Zjistěte, jak monitorovat stav svých prostředků Azure.
* [Správa a zpracování výstrah zabezpečení ve službě Azure Security Center](security-center-managing-and-responding-alerts.md). Zjistěte, jak spravovat a zpracovávat výstrahy zabezpečení.
* [Monitorování partnerských řešení pomocí služby Azure Security Center](security-center-partner-solutions.md). Zjistěte, jak monitorovat stav partnerských řešení.
* [Azure Security Center – nejčastější dotazy](security-center-faq.md). Přečtěte si nejčastější dotazy o použití této služby.
* [Blog o zabezpečení Azure](http://blogs.msdn.com/b/azuresecurity/). Přečtěte si příspěvky o zabezpečení Azure a dodržování předpisů.
