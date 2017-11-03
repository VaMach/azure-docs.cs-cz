---
title: "Oprava chyb zabezpečení operačního systému v Azure Security Center | Microsoft Docs"
description: "Tento dokument se dozvíte, jak provést doporučení Azure Security Center ** ohrožení zabezpečení operačního systému opravit **."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 991d41f5-1d17-468d-a66d-83ec1308ab79
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/11/2017
ms.author: terrylan
ms.openlocfilehash: 39879c22278a55f841e294cda5a89bec2bdf6988
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="remediate-os-vulnerabilities-in-azure-security-center"></a>Oprava chyb zabezpečení operačního systému v Azure Security Center
Azure Security Center analyzuje denně operační systém (OS) virtuálních počítačů (VM) a počítačů pro konfiguraci, které by mohly znamenat virtuálních počítačů a počítačů zranitelnější vůči útokům. Security Center doporučuje, když vaše konfigurace operačního systému neodpovídá pravidla doporučenou konfiguraci a doporučuje změny konfigurace, které tyto nedostatky řeší chyby zabezpečení.

> [!NOTE]
> Další informace o konkrétní konfigurace se sledují, najdete v článku [seznam doporučenou konfiguraci pravidel](https://gallery.technet.microsoft.com/Azure-Security-Center-a789e335).
>
>

## <a name="implement-the-recommendation"></a>Implementace doporučení
Oprava operačního systému ohrožení zabezpečení se zobrazí jako doporučení ve službě Security Center. Toto doporučení se zobrazí v části **doporučení** a v části **výpočetní**.

V tomto příkladu se podíváme **ohrožení zabezpečení operačního systému opravit (společností Microsoft)** doporučení v části **výpočetní**.
1. Vyberte **výpočetní** v hlavní nabídce Security Center.

   ![Napravit ohrožení zabezpečení operačního systému][1]

2. V části **výpočetní**, vyberte **ohrožení zabezpečení operačního systému opravit (společností Microsoft)**. **Ohrožení zabezpečení operačního systému (společností Microsoft) neshoda** otevře řídicí panel.

   ![Napravit ohrožení zabezpečení operačního systému][2]

  Horní části řídicí panel poskytuje:

  - Celkový počet pravidel podle závažnosti, která konfigurace operačního systému se nezdařilo napříč Virtuálního počítače a počítače.
  - Celkový počet pravidel pomocí typu, který konfigurace operačního systému se nezdařilo napříč Virtuálního počítače a počítače.
  - Celkový počet pravidel se nezdařilo, podle konfigurace vašeho operačního systému Windows a vaše konfigurace operačního systému Linux.

  Dolní části řídicí panel obsahuje seznam všech neúspěšných pravidel rámci virtuálních počítačů a počítačů a závažnost chybějící aktualizace. Seznam obsahuje:

  - **CCEID**: CCE jedinečný identifikátor pro pravidlo. Security Center používá společné konfigurace – výčet (CCE) k přiřazení jedinečné identifikátory pro konfigurační pravidla.
  - **NÁZEV**: název pravidla se nezdařila
  - **Typ pravidla**: klíč registru, zásady zabezpečení nebo zásady auditu
  - **POČET Virtuální počítače a počítače**: Celkový počet virtuálních počítačů a počítačů, které služby při selhání rozhodl, platí pro
  - **PRAVIDLO ZÁVAŽNOST**: hodnota závažnosti CCE kritické, důležité nebo upozornění
  - **STAV**: Aktuální stav doporučení:

    - **Open** (Otevřené): Doporučení dosud nebylo řešeno.
    - **V průběhu**: doporučení se aktuálně na tyto prostředky a není třeba žádné akce
    - **Vyřešeno**: Doporučení už je dokončené. (Při byl problém vyřešen, položka je zobrazena šedě)

3. Vyberte v seznamu zobrazíte podrobnosti neúspěšné pravidlo.

   ![Pravidla konfigurace, které selhaly][3]

  V tomto okně se poskytuje následující informace:

  - NÁZEV – Název pravidla
  - CCIED – CCE jedinečný identifikátor pro pravidlo
  - Verze operačního systému – verze operačního systému virtuálního počítače nebo počítače
  - PRAVIDLO ZÁVAŽNOST – Hodnota závažnosti CCE kritická, důležité nebo upozornění
  - Úplný popis – Popis pravidla
  - Ohrožení zabezpečení – Vysvětlení ohrožení zabezpečení nebo riziko, pokud není použita pravidla
  - POTENCIÁLNÍ dopad – Dopad na chod firmy při použití pravidla
  - PROTIOPATŘENÍM – postup nápravy
  - OČEKÁVANÁ hodnota – Hodnota očekávané při Security Center analyzuje konfiguraci operačního systému virtuálního počítače podle pravidla
  - Skutečná hodnota – Hodnota vrácena po dokončení analýzy konfigurace operačního systému virtuálního počítače podle pravidla
  - – PRAVIDLO pravidlo operace použije pomocí služby Security Center při analýze konfigurace operačního systému virtuálního počítače podle pravidla

4. Vyberte **vyhledávání** ikona na pásu karet nejvyšší. Hledání otevře výpis pracovní prostory, které mají virtuální počítače a počítače s vybranou ohrožení zabezpečení operačního systému. Toto okno Výběr pracovního prostoru se zobrazí pouze pokud vybrané pravidlo platí pro víc virtuálních počítačů, které jsou připojené k jiné pracovní prostory.

  ![Uvedené pracovních prostorů][4]

5. Vyberte pracovní prostor. Vyhledávací dotaz analýzy protokolů otevře filtrované do pracovního prostoru s ohrožení zabezpečení operačního systému.

  ![Pracovní prostor se ohrožení zabezpečení operačního systému][5]

6. Vyberte počítač ze seznamu pro další informace. Další výsledek hledání otevře s informacemi, které jsou filtrovány pouze pro tento počítač.

  ![Filtrovaný pro tento počítač][6]

## <a name="next-steps"></a>Další kroky
Tento článek vám ukázal, jak provést doporučení Security Center "Napravit OS ohrožení zabezpečení." Můžete zkontrolovat sadu pravidel, konfigurace [zde](https://gallery.technet.microsoft.com/Azure-Security-Center-a789e335). Security Center používá CCE (Common Configuration výčtu) k přiřazení jedinečné identifikátory pro konfigurační pravidla. Přejděte [CCE](https://nvd.nist.gov/cce/index.cfm) lokality pro další informace.

Další informace o službě Security Center, najdete v následujících zdrojích informací:

* [Podporované platformy v Azure Security Center](security-center-os-coverage.md) -obsahuje seznam podporovaných Windows a virtuální počítače s Linuxem.
* [Nastavení zásad zabezpečení v Azure Security Center](security-center-policies.md) – zjistěte, jak nakonfigurovat zásady zabezpečení pro skupiny prostředků a předplatná Azure.
* [Správa doporučení zabezpečení v Azure Security Center](security-center-recommendations.md) – zjistěte, jak vám doporučení pomáhají chránit prostředky v Azure.
* [Sledování stavu zabezpečení v Azure Security Center](security-center-monitoring.md) – Naučte se monitorovat stav svých prostředků Azure.
* [Správa a zpracování výstrah zabezpečení v Azure Security Center](security-center-managing-and-responding-alerts.md) – zjistěte, jak spravovat a reakce na výstrahy zabezpečení.
* [Sledování partnerských řešení pomocí Azure Security Center](security-center-partner-solutions.md) – zjistěte, jak sledovat stav vašich partnerských řešení.
* [Nejčastější dotazy k Azure Security Center](security-center-faq.md) – přečtěte si nejčastější dotazy o použití této služby.
* [Blog o bezpečnosti Azure](http://blogs.msdn.com/b/azuresecurity/) – přečtěte si příspěvky o zabezpečení Azure a dodržování předpisů.

<!--Image references-->
[1]: ./media/security-center-remediate-os-vulnerabilities/compute-blade.png
[2]:./media/security-center-remediate-os-vulnerabilities/os-vulnerabilities.png
[3]: ./media/security-center-remediate-os-vulnerabilities/vulnerability-details.png
[4]: ./media/security-center-remediate-os-vulnerabilities/search.png
[5]: ./media/security-center-remediate-os-vulnerabilities/log-search.png
[6]: ./media/security-center-remediate-os-vulnerabilities/search-results.png
