---
title: "Správa výstrah zabezpečení ve službě Azure Security Center | Dokumentace Microsoftu"
description: "Tento dokument vám pomůže používat funkce služby Azure Security Center ke správě výstrah zabezpečení a reagování na ně."
services: security-center
documentationcenter: na
author: YuriDio
manager: mbaldwin
editor: 
ms.assetid: b88a8df7-6979-479b-8039-04da1b8737a7
ms.service: security-center
ms.topic: hero-article
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/06/2017
ms.author: yurid
translationtype: Human Translation
ms.sourcegitcommit: 094729399070a64abc1aa05a9f585a0782142cbf
ms.openlocfilehash: bf3aa51b4b1224840ae6897ff7df86cf3d2b3477
ms.lasthandoff: 03/07/2017


---
# <a name="managing-and-responding-to-security-alerts-in-azure-security-center"></a>Správa a zpracování výstrah zabezpečení v Azure Security Center
Tento dokument vám pomůže používat Azure Security Center ke správě výstrah zabezpečení a reagování na ně.

> [!NOTE]
> Pokud chcete povolit rozšířené detekce, upgradujte na Azure Security Center Standard. K dispozici je bezplatná 60denní zkušební verze. Pokud chcete provést upgrade, vyberte v [zásadách zabezpečení](security-center-policies.md) cenovou úroveň. Další informace najdete v článku o [cenách Azure Security Center](security-center-pricing.md).
>
>

## <a name="what-are-security-alerts"></a>Co jsou výstrahy zabezpečení?
Security Center automaticky shromažďuje, analyzuje a integruje data protokolu z vašich prostředků Azure, sítě a připojených partnerských řešení, jako jsou brány firewall a řešení ochrany koncových bodů, aby se zjistily skutečné hrozby a snížil počet falešných poplachů. Seznam upřednostňovaných výstrah zabezpečení se zobrazí ve službě Security Center spolu s informacemi, které potřebujete k rychlému prozkoumání problému, a doporučeními týkajícími se řešení útoku.


> [!NOTE]
> Další informace o tom, jak detekce služby Security Center pracuje, najdete v článku [Funkce detekce ve službě Azure Security Center](security-center-detection-capabilities.md).
>
>

## <a name="managing-security-alerts"></a>Správa výstrah zabezpečení
Aktuální výstrahy můžete zkontrolovat jejich prohlížením na dlaždici **Výstrahy zabezpečení**. Přejděte na Azure Portal a podle následujících pokynů zobrazte podrobnosti o jednotlivých výstrahách:

1. Na řídicím panelu Security Center uvidíte dlaždici **Výstrahy zabezpečení**.

    ![Dlaždice Výstrahy zabezpečení ve službě Security Center](./media/security-center-managing-and-responding-alerts/security-center-managing-and-responding-alerts-fig1-ga.png)

2. Kliknutím na dlaždici **Výstrahy zabezpečení** otevřete okno, které obsahuje podrobnosti o výstrahách, jak je uvedeno dále.

   ![Okno Výstrahy zabezpečení ve službě Security Center](./media/security-center-managing-and-responding-alerts/security-center-managing-and-responding-alerts-fig2-ga.png)

V dolní části tohoto okna jsou uvedené podrobnosti pro každou výstrahu. Chcete-li je seřadit, klikněte na sloupec, podle kterého chcete řadit. Definice pro každý sloupec je uvedená dále:

* **Description** (Popis): Stručné vysvětlení výstrahy.
* **Count** (Počet): Seznam všech výstrah tohoto konkrétního typu, které byly zjištěny v určitý den.
* **Detected by** (Zjistil): Služba, která je zodpovědná za aktivaci výstrahy.
* **Date** (Datum): Datum, kdy došlo k události.
* **State** (Stav): Aktuální stav výstrahy. Existují dva typy stavů:
  * **Active** (Aktivní): Výstraha zabezpečení byla zjištěna.
* **Severity** (Závažnost): Úroveň závažnosti, tj. vysoká, střední nebo nízká.

### <a name="filtering-alerts"></a>Filtrování výstrah
Výstrahy můžete filtrovat podle data, stavu nebo závažnosti. Filtrování výstrah může být užitečné v případech, kdy potřebujete zúžit obor zobrazených výstrah zabezpečení. Například můžete chtít zabývat se výstrahami zabezpečení, k nimž došlo v posledních 24 hodinách, protože zjišťujete případný průnik do systému.

1. Klikněte v okně **Výstrahy zabezpečení** na **Filtr**. Otevře se okno **Filtr** a vy vyberte hodnoty pro datum, stav a závažnost výstrah, které chcete vidět.

    ![Filtrování výstrah ve službě Security Center](./media/security-center-managing-and-responding-alerts/security-center-managing-and-responding-alerts-fig3-2017.png)

### <a name="respond-to-security-alerts"></a>Reakce na výstrahy zabezpečení
Vyberte výstrahu zabezpečení, abyste se dozvěděli další informace o událostech, které výstrahu vygenerovaly a kroky, pokud existují, které je třeba provést k nápravě útoku. Výstrahy zabezpečení jsou seskupené podle typu a data. Kliknutím na výstrahu zabezpečení se otevře okno obsahující seznam seskupených výstrah.

![Reakce na výstrahy zabezpečení v Azure Security Center](./media/security-center-managing-and-responding-alerts/security-center-managing-and-responding-alerts-fig5-ga.png)

V tomto případě odkazují vygenerované výstrahy na podezřelé aktivity protokolu RDP (Remote Desktop Protocol). První sloupec zobrazuje, které prostředky byly napadeny, druhý zobrazuje počet útoků, třetí čas útoku, čtvrtý stav výstrahy a pátý závažnost útoku. Po zkontrolování těchto informací klikněte na prostředek, který byl napaden. Otevře se nové okno.

![Návrhy postupu při výstrahách zabezpečení v Azure Security Center](./media/security-center-managing-and-responding-alerts/security-center-managing-and-responding-alerts-fig6-ga.png)

V poli **Popis** tohoto okna najdete další podrobnosti o této události. Tyto další podrobnosti nabízejí získání náhledu na příčinu výstrahy zabezpečení, cílový prostředek, případnou zdrojovou IP adresu a doporučení, jak provést nápravu.  V některých případech bude zdrojová IP adresa prázdná (nedostupná), protože ne všechny protokoly událostí zabezpečení Windows obsahující IP adresu.

Náprava navrhovaná službou Security Center se bude lišit podle výstrahy zabezpečení. V některých případech budete muset k implementaci doporučené nápravy použít další možnosti Azure. Například nápravou pro tento útok je uvedení IP adresy, která generuje tento útok, v seznamu zakázaných serverů, a to pomocí [seznamu ACL sítě](../virtual-network/virtual-networks-acl.md) nebo pravidla [skupiny zabezpečení sítě](../virtual-network/virtual-networks-nsg.md).

> [!NOTE]
> Další informace o různých typech výstrah najdete v tématu [Výstrahy zabezpečení podle typu v Azure Security Center](security-center-alerts-type.md).
>
>

## <a name="see-also"></a>Viz také
V tomto dokumentu jste zjistili, jak ve službě Security Center konfigurovat zásady zabezpečení. Pokud se o službě Security Center chcete dozvědět víc, pročtěte si tato témata:

* [Řešení bezpečnostních incidentů v Azure Security Center](security-center-incident.md)
* [Funkce detekce ve službě Azure Security Center](security-center-detection-capabilities.md)
* [Průvodce plánováním a provozem služby Azure Security Center](security-center-planning-and-operations-guide.md)
* [Azure Security Center – nejčastější dotazy](security-center-faq.md) – Přečtěte si nejčastější dotazy o použití této služby.
* [Blog o zabezpečení Azure](http://blogs.msdn.com/b/azuresecurity/) – Přečtěte si příspěvky o zabezpečení Azure a dodržování předpisů.

