---
title: "Analýza využití dat v Log Analytics | Dokumentace Microsoftu"
description: "Chcete-li zobrazit, kolik dat je odesíláno do služby OMS, můžete k tomu použít stránku Využití v Log Analytics."
services: log-analytics
documentationcenter: 
author: bandersmsft
manager: jwhit
editor: 
ms.assetid: 74d0adcb-4dc2-425e-8b62-c65537cef270
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/11/2016
ms.author: banders
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: 4c1d0319e768eec69718d8b49db30069ec1331d9


---
# <a name="analyze-data-usage-in-log-analytics"></a>Analýza využití dat v Log Analytics
Log Analytics v Operations Management Suite (OMS) shromažďuje data a pravidelně je odesílá do služby OMS.  Chcete-li zobrazit, kolik dat je odesíláno do služby OMS, můžete k tomu použít stránku **Využití**. Stránka **Využití** také ukazuje, kolik dat je denně odesíláno řešeními a jak často vaše servery data odesílají.

> [!NOTE]
> Máte-li bezplatný účet vytvořený pomocí [webu OMS](http://www.microsoft.com/oms), je odesílání dat do služby OMS omezeno na 500 MB denně. Pokud denní limit překročíte, analýza dat se zastaví a bude pokračovat na začátku dalšího dne. Bude také nutné znovu odeslat veškerá data, která nebyla přijata nebo zpracována v OMS.
> 
> 

Využití můžete zobrazit pomocí dlaždice **Využití** na řídicím panelu **Přehled** v OMS.

![dlaždice využití](./media/log-analytics-usage/usage-tile.png)

Pokud jste denní limit využití překročili nebo se mu blížíte, můžete volitelně odebrat některé řešení a snížit tak množství dat, která odesíláte do služby OMS. Další informace o odebírání řešení najdete v tématu [Přidání řešení Log Analytics z Galerie řešení](log-analytics-add-solutions.md).

![řídicí panel využití](./media/log-analytics-usage/usage-dashboard.png)

Stránka **Využití** zobrazuje následující informace:

* Průměrné denní využití
* Využití dat pro jednotlivá řešení za posledních 30 dnů
* Množství dat odeslaných servery ve vašem prostředí do služby OMS za posledních 30 dnů
* Cenová úroveň datového tarifu a odhadované náklady
* Informace o smlouvě o rozsahu služeb (SLA) včetně toho, jak dlouho trvá zpracování vašich dat v OMS

## <a name="to-work-with-usage-data"></a>Práce s daty o využití
1. Na stránce **Přehled** klikněte na dlaždici **Využití**.
2. Na stránce **Využití** si prohlédněte kategorie využití představující oblasti, o které se zajímáte.
3. Máte-li řešení, které využívá příliš velkou část vaší denní kvóty pro nahrávání, měli byste zvážit odebrání takového řešení.

## <a name="to-view-your-estimated-cost-and-billing-information"></a>Zobrazení odhadovaných nákladů a informací o fakturaci
1. Na stránce **Přehled** klikněte na dlaždici **Využití**.
2. Na stránce **Využití** v části **Využití** klikněte na dvojitou šipku (**>**) vedle položky **Odhadované náklady**.
3. V rozbalených podrobnostech **Váš datový tarif** uvidíte odhadované měsíční náklady.  
    ![Váš datový tarif](./media/log-analytics-usage/usage-data-plan.png)
4. Chcete-li zobrazit informace o fakturaci, kliknutím na **Zobrazit fakturu** zobrazíte informace o svém předplatném.
   * Na stránce s předplatnými kliknutím na své předplatné zobrazíte podrobnosti a seznam položek řádku s využitím.  
       ![předplatné](./media/log-analytics-usage/usage-sub01.png)
   * Na stránce Souhrn pro vaše předplatné můžete provádět různé úlohy správy a zobrazit další podrobnosti o předplatném.  
       ![podrobnosti o předplatném](./media/log-analytics-usage/usage-sub02.png)

## <a name="to-view-data-batches-for-your-sla"></a>Zobrazení datových balíků pro vaši SLA
1. Na stránce **Přehled** klikněte na dlaždici **Využití**.
2. V části **Smlouva SLA** klikněte na **Stáhnout podrobnosti o SLA**.
3. Stáhne se soubor XLSX aplikace Excel, ve kterém můžete údaje zkontrolovat.  
    ![Podrobnosti o SLA](./media/log-analytics-usage/usage-sla-details.png)

## <a name="next-steps"></a>Další kroky
* Chcete-li zobrazit podrobné informace shromážděné řešeními, přečtěte si téma [Vyhledávání protokolů v Log Analytics](log-analytics-log-searches.md).




<!--HONumber=Nov16_HO2-->


