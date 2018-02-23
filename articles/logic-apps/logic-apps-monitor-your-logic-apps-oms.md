---
title: "Monitorování a získání přehledy o aplikaci logiky spouští pomocí OMS - Azure Logic Apps | Microsoft Docs"
description: "Monitorování vaší logiku aplikace běží s analýzy protokolů a Operations Management Suite (OMS), chcete-li získat přehledy a bohatší ladění podrobnosti pro řešení potíží a diagnostiku"
author: divyaswarnkar
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: 
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/9/2017
ms.author: LADocs; divswa
ms.openlocfilehash: 2f9f27dc74348909b89941c2bb17ccdf610dba33
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/21/2018
---
# <a name="monitor-and-get-insights-about-logic-app-runs-with-operations-management-suite-oms-and-log-analytics"></a>Monitorovat a získat přehled o logiku aplikace běží s Operations Management Suite (OMS) a analýzy protokolů

Pro monitorování a bohatší informace o ladění můžete zapnout analýzy protokolů ve stejnou dobu, po vytvoření aplikace logiky. Log Analytics poskytuje možnosti pro diagnostiku protokolování a monitorování pro svou aplikaci logiky spouští prostřednictvím portálu Operations Management Suite (OMS). Když přidáte do řešení pro správu aplikací logiky na OMS, zobrazí agregovaný stav pro logiku aplikace běží a konkrétní podrobnosti, jako je stav, čas spuštění, opakované odeslání stavu a ID korelace.

Toto téma ukazuje, jak zapnout analýzy protokolů nebo nainstalujte řešení pro správu aplikace logiky v OMS, můžete zobrazit události modulu runtime a data pro svou aplikaci logiky spustit.

 > [!TIP]
 > Pokud chcete sledovat existující aplikace logiky, použijte následující postup [zapnout protokolování diagnostiky a posílat data runtime aplikace logiky OMS](../logic-apps/logic-apps-monitor-your-logic-apps.md#azure-diagnostics).

## <a name="requirements"></a>Požadavky

Než začnete, musíte mít pracovním prostorem OMS. Další informace [jak vytvořit pracovní prostor služby OMS](../log-analytics/log-analytics-get-started.md). 

## <a name="turn-on-diagnostics-logging-when-creating-logic-apps"></a>Zapnutí protokolování diagnostiky při vytváření aplikací logiky

1. V [portál Azure](https://portal.azure.com), vytvoření aplikace logiky. Zvolte **vytvořit prostředek** > **Enterprise integrace** > **aplikace logiky**.

   ![Vytvoření aplikace logiky](media/logic-apps-monitor-your-logic-apps-oms/find-logic-apps-azure.png)

2. V **vytvořit aplikaci logiky** proveďte tyto úlohy, jak je znázorněno:

   1. Zadejte název pro svou aplikaci logiky a vyberte předplatné Azure. 
   2. Vytvořte nebo vyberte skupinu prostředků Azure.
   3. Nastavit **protokolu analýzy** k **na**. 
   Vyberte pracovní prostor OMS, kde chcete odeslat data pro svou aplikaci logiky spouští. 
   4. Až budete připraveni, zvolte **připnout na řídicí panel** > **vytvořit**.

      ![Vytvoření aplikace logiky](./media/logic-apps-monitor-your-logic-apps-oms/create-logic-app.png)

      Po dokončení tohoto kroku, Azure vytvoří aplikace logiky, která je teď spojené s vaším pracovním prostorem OMS. 
      Tento krok navíc také automaticky nainstaluje řešení pro správu aplikace logiky v pracovním prostoru OMS.

3. Chcete-li zobrazit logiky aplikace běží v OMS, [pokračovat v těchto krocích](#view-logic-app-runs-oms).

## <a name="install-the-logic-apps-management-solution-in-oms"></a>Nainstalujte do řešení pro správu aplikace logiky v OMS

Pokud jste již zapnuli analýzy protokolů při vytvoření aplikace logiky, tento krok přeskočte. Už máte nainstalovaný v OMS řešení pro správu aplikace logiky.

1. V [portál Azure](https://portal.azure.com), zvolte **více služeb**. Vyhledejte "analýzy protokolů" jako filtr a vyberte **analýzy protokolů** znázorněné:

   ![Zvolte "Analýzy protokolů"](media/logic-apps-monitor-your-logic-apps-oms/find-log-analytics.png)

2. V části **analýzy protokolů**, najděte a vyberte pracovní prostor OMS. 

   ![Vyberte pracovní prostor OMS](media/logic-apps-monitor-your-logic-apps-oms/select-logic-app.png)

3. V části **správy**, zvolte **portálu OMS**.

   ![Zvolte "Portál OMS"](media/logic-apps-monitor-your-logic-apps-oms/oms-portal-page.png)

4. Na domovské stránce OMS Pokud se zobrazí Banner informující o upgradu, zvolte Banner informující o tak, aby nejprve upgradovat vaším pracovním prostorem OMS. Zvolte **řešení Galerie**.

   ![Zvolte "Galerie řešení"](media/logic-apps-monitor-your-logic-apps-oms/solutions-gallery.png)

5. V části **všechna řešení**, najít a vybrat na dlaždici **správy Logic Apps** řešení.

   ![Zvolte "Správa aplikace logiky"](media/logic-apps-monitor-your-logic-apps-oms/logic-apps-management-tile2.png)

6. Chcete-li nainstalovat řešení v pracovním prostoru OMS, zvolte **přidat**.

   ![Zvolte "Přidat" pro "Správa aplikace logiky"](media/logic-apps-monitor-your-logic-apps-oms/add-logic-apps-management-solution.png)

<a name="view-logic-app-runs-oms"></a>

## <a name="view-your-logic-app-runs-in-your-oms-workspace"></a>Zobrazení, které běží vaše aplikace logiky v pracovním prostoru OMS

1. Chcete-li zobrazit počet a stav pro vaše aplikace běží logiku, přejděte na stránku přehled pro pracovní prostor OMS. Zkontrolujte podrobnosti v **správy Logic Apps** dlaždici.

   ![Dlaždice přehledu se zobrazuje počet logiku aplikace spustit a stav](media/logic-apps-monitor-your-logic-apps-oms/overview.png)

   > [!Note]
   > Pokud se zobrazí banner upgradu místo dlaždici správy Logic Apps, zvolte Banner informující o tak, aby nejprve upgradovat vaším pracovním prostorem OMS.
  
   > ![Upgrade "Pracovním prostorem OMS"](media/logic-apps-monitor-your-logic-apps-oms/oms-upgrade-banner.png)

2. Chcete-li zobrazit souhrn s dalšími podrobnostmi o vašem logiku aplikace běží, vyberte **správy Logic Apps** dlaždici.

   Zde že logiku aplikace běží jsou seskupené podle názvu nebo stav spuštění. Můžete také zjistit podrobnosti o selhání v akce nebo aktivačních událostí pro logiku aplikace běží.

   ![Souhrn stavu pro svou aplikaci logiky běží](media/logic-apps-monitor-your-logic-apps-oms/logic-apps-runs-summary.png)
   
3. Chcete-li zobrazit všechny spuštění pro určitou logiku aplikace nebo stav, vyberte řádek pro aplikace logiky nebo stav.

   Tady je příklad, který zobrazuje všechny spuštění pro určitou logiku aplikace:

   ![Spuštění zobrazení pro aplikaci logiky nebo ve stavu](media/logic-apps-monitor-your-logic-apps-oms/logic-app-run-details.png)

   Existují dvě pokročilé možnosti na této stránce:
   * **Sledovat vlastnosti:** tomto sloupci se zobrazuje sledovaných vlastnosti, které jsou seskupené podle akce pro aplikaci logiky. Chcete-li zobrazit vlastnosti sledovaných, zvolte **zobrazení**. Pomocí filtru sloupec může hledat sledovaných vlastnosti.
   
     ![Zobrazit sledovaných vlastnosti pro aplikace logiky](media/logic-apps-monitor-your-logic-apps-oms/logic-app-tracked-properties.png)

     Všechny nově přidané vlastnosti sledovaných může trvat 10 až 15 minut, než se objeví poprvé. Další informace [jak přidat do aplikace logiky sledovaných vlastnosti](logic-apps-monitor-your-logic-apps.md#azure-diagnostics-event-settings-and-details).

   * **Odešlete:** můžete znovu odeslat jeden nebo více spustí logiku aplikace, které selhaly, byly úspěšné, nebo jsou stále běží. Zaškrtněte políčka pro spuštění, které chcete znovu odeslat a vyberte **odešlete znovu**. 

     ![Odešlete znovu logiku aplikace běží](media/logic-apps-monitor-your-logic-apps-oms/logic-app-resubmit.png)

4. Chcete-li filtrovat tyto výsledky, můžete provádět klientské a serverové filtrování.

   * Filtr na straně klienta: pro každý sloupec vyberte filtry, které chcete. 
   Zde je několik příkladů:

     ![Příklad filtry sloupců](media/logic-apps-monitor-your-logic-apps-oms/filters.png)

   * Filtr na straně serveru: Zvolte konkrétního časového okna nebo omezit počet spuštění, které se zobrazují, pomocí ovládacího prvku oboru v horní části stránky. 
   Ve výchozím nastavení zobrazí pouze 1000 záznamů najednou. 
   
     ![Změnit časové okno](media/logic-apps-monitor-your-logic-apps-oms/change-interval.png)
 
5. Chcete-li zobrazit všechny akce a jejich podrobnosti o konkrétní spustit, vyberte řádek pro spuštění aplikace logiky.

   Tady je příklad, který zobrazuje všechny akce pro určitou logiku aplikaci spustit:

   ![Zobrazení akce pro spuštění aplikace logiky](media/logic-apps-monitor-your-logic-apps-oms/logic-app-action-details.png)
   
6. Na všechny stránky s výsledky, chcete-li zobrazit dotaz za výsledky nebo chcete-li zobrazit všechny výsledky, zvolte **najdete v článku všechny**, což otevře se stránka hledání protokolů.
   
   ![Zobrazit všechny na výsledky stránky](media/logic-apps-monitor-your-logic-apps-oms/logic-app-seeall.png)
   
   Na stránce hledání protokolů
   * Chcete-li zobrazit výsledky dotazu v tabulce, zvolte **tabulky**.
   * Chcete-li změnit dotaz, můžete upravit řetězec dotazu v panelu vyhledávání. 
   Pro lepší podmínky, zvolte **pokročilé analýzy**.

     ![Zobrazit akce a podrobnosti pro spuštění aplikace logiky](media/logic-apps-monitor-your-logic-apps-oms/log-search-page.png)
     
     Zde můžete na stránce Azure Log Analytics aktualizovat dotazy a zobrazit výsledky z tabulky. 
     Tento dotaz používá [Kusto dotazovací jazyk](https://docs.loganalytics.io/docs/Language-Reference), který můžete upravit, pokud chcete zobrazit různé výsledky. 

     ![Azure Log Analytics - zobrazení dotazu](media/logic-apps-monitor-your-logic-apps-oms/query.png)

## <a name="next-steps"></a>Další postup

* [Monitorování zpráv B2B](../logic-apps/logic-apps-monitor-b2b-message.md)

