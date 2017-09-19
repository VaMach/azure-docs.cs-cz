---
title: "Monitorování identity a přístupu v Azure Security Center | Dokumentace Microsoftu"
description: "Tento dokument vám pomůže používat funkce identity a přístupu v Azure Security Center k monitorování aktivit přístupu uživatelů a problémů souvisejících s identitou."
services: security-center
documentationcenter: na
author: YuriDio
manager: mbaldwin
editor: 
ms.assetid: 9f04e730-4cfa-4078-8eec-905a443133da
ms.service: security-center
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/13/2017
ms.author: yurid
ms.translationtype: HT
ms.sourcegitcommit: fda37c1cb0b66a8adb989473f627405ede36ab76
ms.openlocfilehash: 2e351cd38e7baefc09fa36ceabffec92de22433b
ms.contentlocale: cs-cz
ms.lasthandoff: 09/14/2017

---
# <a name="monitoring-identity-and-access-in-azure-security-center"></a>Monitorování identity a přístupu v Azure Security Center
Tento dokument vám pomůže používat Azure Security Center k monitorování identity uživatele a aktivit přístupu.

## <a name="why-monitor-identity-and-access"></a>Proč monitorovat identitu a přístup?
Identita by měla být základní „řídicí plochou“ vaší organizace a její ochrana je tou nejvyšší prioritou. V minulosti byly kolem organizací vytyčeny perimetry, které byly hlavní obrannou linií. V současné době se ale čím dál víc dat a aplikací přesouvá do cloudu a novým perimetrem se proto stává identita.

Monitorování aktivit kolem vaší identity budete moci proaktivně jednat ještě než k problematické události dojde nebo provést reaktivní akci k zastavení pokusu o útok. Řídicí panel Identita a přístup poskytuje přehled stavu vaší identity, včetně počtu neúspěšných pokusů o přihlášení, uživatelského účtu použitého při těchto pokusech, uzamčených účtů, účtů se změněným nebo resetovaným heslem a počtu účtů, jejichž uživatelé jsou právě přihlášení.

## <a name="how-to-monitor-identity-and-access-activities"></a>Jak monitorovat identitu a aktivity přístupu?
Pomocí následujícího postupu můžete vizualizovat identitu a přístup související s aktuálními aktivitami. Budete k tomu potřebovat přístup k řídicímu panelu **Identita a přístup**:

1.  Otevřete řídicí panel **Security Center**.
2.  V levém podokně v části **Prevence** klikněte na **Identita a přístup**. Pokud máte více pracovních prostorů, zobrazí se selektor pracovního prostoru.

    ![výběr pracovního prostoru](./media/security-center-identity-access\security-center-identity-access-fig1.png)

    > [!NOTE]
    > Pokud se v posledním sloupci zobrazí **UPGRADUJTE PLÁN**, je to proto, že pracovní prostor používá bezplatné předplatné a pro použití této funkce je potřeba upgradovat na úroveň Standard. Pokud se v něm zobrazí VYŽADUJE UPGRADE, je to proto, že pro použití této funkce je potřeba upgradovat [Azure Log Analytics](https://docs.microsoft.com/azure/log-analytics/log-analytics-overview). Další informace o cenovém plánu najdete v článku o cenách Azure Security Center. 
    > 
3. Pokud máte k prozkoumání více než jeden pracovní prostor, můžete určit prioritu prozkoumání podle sloupce **NEÚSPĚŠNÁ PŘIHLÁŠENÍ**, ve kterém se zobrazuje aktuální počet neúspěšných pokusů o přihlášení v tomto pracovním prostoru. Vyberte pracovní prostor, který chcete použít, a pak se zobrazí řídicí panel **Identita a přístup**.

    ![Identita a přístup](./media/security-center-identity-access\security-center-identity-access-fig2.png)

4. Informace, které jsou k dispozici na tomto řídicím panelu, vám pomůžou okamžitě identifikovat potenciálně podezřelé aktivity. Tento řídicí panel je rozdělen na tři hlavní oblasti:
    * **Stav identity:** Obsahuje souhrn aktivit souvisejících s identitou, které probíhají v tomto pracovním prostoru.
    * **Neúspěšná přihlášení:** Pomáhá rychle identifikovat hlavní příčinu neúspěšného pokusu o přihlášení a zobrazuje seznam deseti účtů s největší neúspěšností pokusů o přihlášení.
    * **Přesčas přihlášení:** Pomáhá rychle identifikovat celkový přesčas přihlášení a zobrazuje seznam nejdelších pokusů o přihlášení k účtu počítače.
    
Řídicí panel, který se zobrazí po výběru jakékoli dlaždice, vychází z [dotazu prohledávání protokolu](https://docs.microsoft.com/azure/security-center/security-center-search), jediným rozdílem je typ dotazu a výsledek. Stále můžete vybrat položku, například počítač, a kliknutím na ni zobrazit související data. 

## <a name="see-also"></a>Viz také
V tomto dokumentu jste zjistili, jak monitorovat identitu a přístup v Azure Security Center. Pokud se o službě Azure Security Center chcete dozvědět víc, pročtěte si tato témata:

* [Správa a zpracování výstrah zabezpečení ve službě Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-managing-and-responding-alerts). Zjistěte, jak spravovat výstrahy a reagovat na incidenty zabezpečení ve službě Security Center.
* [Monitorování stavu zabezpečení ve službě Azure Security Center](security-center-monitoring.md). Zjistěte, jak monitorovat stav svých prostředků Azure.
* [Principy výstrah zabezpečení ve službě Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-alerts-type). Seznamte se s dalšími typy výstrah zabezpečení.
* [Průvodce odstraňováním potíží pro službu Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-troubleshooting-guide). Zjistěte, jak řešit běžné problémy ve službě Security Center. 
* [Azure Security Center – nejčastější dotazy](security-center-faq.md). Přečtěte si nejčastější dotazy o použití této služby.
* [Blog o zabezpečení Azure](http://blogs.msdn.com/b/azuresecurity/). Přečtěte si příspěvky o zabezpečení Azure a dodržování předpisů.


