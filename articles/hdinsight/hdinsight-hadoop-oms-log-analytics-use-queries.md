---
title: "Dotaz na Azure Log Analytics k monitorování clusterů Azure HDInsight | Microsoft Docs"
description: "Zjistěte, jak ke spouštění dotazů na Azure Log Analytics ke sledování úloh spuštěných v clusteru služby HDInsight."
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/11/2017
ms.author: nitinme
ms.openlocfilehash: 8fe91bed69a1c06367346041d8caba4aaee4c82a
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="query-azure-log-analytics-to-monitor-hdinsight-clusters-preview"></a>Dotaz Azure Log Analytics k monitorování clusterů HDInsight (Preview)

V tomto článku se podíváte na některé scénáře o tom, jak používat Azure Log Analytics s Azure HDInsight clustery. Tři nejběžnější scénáře jsou:

* Analyzujte metriky clustery HDInsight v OMS
* Vyhledejte konkrétní protokolu zpráv pro clustery služby HDInsight
* Vytvářet výstrahy na základě událostí, ke kterým dochází v clustery

## <a name="prerequisites"></a>Požadavky

* Musíte nakonfigurovat cluster služby HDInsight používat Azure Log Analytics. Pokyny najdete v tématu [použití Azure Log Analytics s clustery HDInsight](hdinsight-hadoop-oms-log-analytics-tutorial.md).

* Je třeba přidat řešení správy specifických pro cluster HDInsight do pracovního prostoru OMS jak je popsáno v [řešení pro správu clusteru přidat HDInsight k analýze protokolů](hdinsight-hadoop-oms-log-analytics-management-solutions.md).

## <a name="analyze-hdinsight-cluster-metrics-in-oms"></a>Analyzujte metriky clustery HDInsight v OMS

V této části jsme provede kroky k vyhledání konkrétní metriky pro váš cluster HDInsight.

1. Otevřete řídicí panel OMS. Na portálu Azure otevřete okno clusteru HDInsight, který je spojen s Azure Log Analytics, klikněte na kartě Sledování a kliknutím na možnost **otevřete řídicí panel OMS**.

    ![Otevřete řídicí panel OMS](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-open-oms-dashboard.png "OMS otevřete řídicí panel")

2. Na řídicím panelu OMS z domovské obrazovce, klikněte na tlačítko **hledání protokolů**.

    ![Otevřete vyhledávání protokolu](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-click-log-search.png "otevřete vyhledávání protokolu")

3. V okně hledání protokolů v **Begin vyhledávání zde** textového pole, typ `*` chcete vyhledat všechny metriky pro všechny dostupné metriky pro všechny clustery HDInsight, které jsou nakonfigurované na používání Azure Log Analytics. Stiskněte klávesu ENTER.

    ![Vyhledat všechny metriky](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-search-all-metrics.png "vyhledat všechny metriky")

4. Měli byste vidět výstup jako následující.

    ![Vyhledat všechny metriky výstup](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-search-all-metrics-output.png "hledání veškerý výstup metriky")

5. V levém podokně v části **typ** kategorie, vyhledávání a metriku, kterou chcete prozkoumat o. V tomto kurzu vytvoříme `metrics_resourcemanager_queue_root_default_CL`. Zaškrtněte políčko odpovídající metriky a pak klikněte na **použít**.

    > [!NOTE]
    > Možná budete muset klikněte **[+] Další** tlačítko Najít metriku, kterou hledáte. Navíc **použít** tlačítko je v dolní části seznamu, musí přejděte dolů k jeho zobrazení.
    > 
    >    
    Všimněte, že dotaz do textového pole nyní změní na jednu zobrazené v poli zvýrazněná na následujícím snímku obrazovky:

    ![Vyhledejte konkrétní metriky](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-search-specific-metrics.png "vyhledejte konkrétní metriky")

6. Můžete můžete nyní podrobněji tato konkrétní metrika. Například můžete upřesnit teď existující výstup podle průměr prostředky používané v 10minutových intervalech, zařazený do kategorie službou název clusteru. Zadejte následující dotaz do textového pole dotazu.

        * (Type=metrics_resourcemanager_queue_root_default_CL) | measure avg(UsedAMResourceMB_d) by ClusterName_s interval 10minute

    ![Vyhledejte konkrétní metriky](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-search-more-specific-metrics.png "vyhledejte konkrétní metriky")

7. Místo upřesnění podle průměr prostředky využívané, můžete použít následující dotaz k zpřesnění výsledků na základě při maximální prostředky používaly (i 90 a 95. percentil) v okně 10 minut.

        * (Type=metrics_resourcemanager_queue_root_default_CL) | measure max(UsedAMResourceMB_d) , pct95(UsedAMResourceMB_d), pct90(UsedAMResourceMB_d)  by ClusterName_s interval 10minute

    ![Vyhledejte konkrétní metriky](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-search-more-specific-metrics-1.png "vyhledejte konkrétní metriky")

## <a name="search-for-specific-log-messages-in-hdinsight-clusters"></a>Vyhledejte konkrétní protokolu zpráv v clusterech HDInsight

V této části jsme provede jednotlivými kroky vzhled chybových zpráv během konkrétního časového okna. Kroky zde jsou pouze jeden z příkladů na tom, jak můžete dorazit chybovou zprávu můžete zajímá. Můžete použít libovolné vlastnosti, které jsou k dispozici. Vyhledejte chyby, které chcete najít.

1. Otevřete řídicí panel OMS. Na portálu Azure otevřete okno clusteru HDInsight, který je spojen s Azure Log Analytics, klikněte na kartě Sledování a kliknutím na možnost **otevřete řídicí panel OMS**.

    ![Otevřete řídicí panel OMS](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-open-oms-dashboard.png "OMS otevřete řídicí panel")

2. Na řídicím panelu OMS z domovské obrazovce, klikněte na tlačítko **hledání protokolů**.

    ![Otevřete vyhledávání protokolu](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-click-log-search.png "otevřete vyhledávání protokolu")

3. V okně hledání protokolů v **Begin vyhledávání zde** textového pole, typ `"Error"` (včetně uvozovek) k vyhledání všech chybových zpráv pro všechny clustery HDInsight, které jsou nakonfigurované na používání Azure Log Analytics. Stiskněte klávesu ENTER.

    ![Vyhledat všechny chyby](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-search-all-errors.png "vyhledávání všech chyb")

4. Měli byste vidět výstup jako následující.

    ![Vyhledat všechny chyby výstupu](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-search-all-errors-output.png "vyhledat všechny chyby výstupu")

5. V levém podokně v části **typ** kategorie, typu chyby, který chcete prozkoumat o vyhledávání. V tomto kurzu vytvoříme `log_sparkappsexecutors_CL`. Zaškrtněte políčko odpovídající metriky a pak klikněte na **použít**.

    ![Vyhledejte konkrétní chyby](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-search-specific-error.png "vyhledejte konkrétní chyby")

        
6. Všimněte si, že dotaz do textového pole nyní změny ukazuje zvýrazněné níže zobrazené políčko a výsledky jsou přesnějších jenom zobrazit chyba typu, který jste vybrali.

    ![Vyhledejte konkrétní chyby výstupu](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-search-specific-error-output.png "vyhledejte konkrétní chyby výstupu")

7. Můžete můžete nyní podrobněji tento seznam konkrétních chyb pomocí možnosti dostupné v levém podokně. Například můžete upřesnit pouze podívejte se na chybové zprávy z konkrétního pracovního uzlu dotazu.

    ![Vyhledejte konkrétní chyby výstupu](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-search-specific-error-refined.png "vyhledejte konkrétní chyby výstupu")

8. V můžete další zóny na dobu, kterou si myslíte, že výběrem příslušné časová hodnota v levém podokně se stala chyba.

    ![Vyhledejte konkrétní chyby výstupu](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-search-specific-error-time.png "vyhledejte konkrétní chyby výstupu")

9. Nyní jste na specifické chybě, kterou hledáte. Můžete kliknout na **[+] zobrazit další** podívat se na skutečné chybové zprávě.

    ![Vyhledejte konkrétní chyby výstupu](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-search-specific-error-arrived.png "vyhledejte konkrétní chyby výstupu")

## <a name="create-alerts-to-track-events"></a>Vytvoření oznámení pro sledování událostí

Prvním krokem k vytvoření výstrahy je přicházejí na dotaz, podle kterého se výstraha spustila. Pro jednoduchost použijeme následující dotaz, který obsahuje seznam selhání aplikací běžících na clustery HDInsight.

    * (Typ = metrics_resourcemanager_queue_root_default_CL) AppsFailed_d > 0 

Můžete použít jakýkoli dotaz, který chcete vytvořit výstrahu.

1. Otevřete řídicí panel OMS. Na portálu Azure otevřete okno clusteru HDInsight, který je spojen s Azure Log Analytics, klikněte na kartě Sledování a kliknutím na možnost **otevřete řídicí panel OMS**.

    ![Otevřete řídicí panel OMS](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-open-oms-dashboard.png "OMS otevřete řídicí panel")

2. Na řídicím panelu OMS z domovské obrazovce, klikněte na tlačítko **hledání protokolů**.

    ![Otevřete vyhledávání protokolu](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-click-log-search.png "otevřete vyhledávání protokolu")

3. V okně hledání protokolů v **Begin vyhledávání zde** text pole, vložte dotaz, na kterém chcete vytvořit výstrahu, stiskněte klávesu ENTER a pak klikněte na tlačítko **výstraha** tlačítko.

    ![Zadejte dotaz na vytvoření výstrahy](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-create-alert-query.png "zadejte dotaz na vytvoření výstrahy")

4. V **přidat pravidlo výstrah** okno, zadejte dotaz a další podrobnosti o vytvoření výstrahy, a pak klikněte na **Uložit**.

    ![Zadejte dotaz na vytvoření výstrahy](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-create-alert.png "zadejte dotaz na vytvoření výstrahy")

    Na tomto snímku obrazovky jsme jenom k odesílání e-mailové oznámení Pokud je výstraha dotazu načte výstup.

5. Můžete také upravit nebo odstranit existující výstrahy. Uděláte to tak, že z libovolné stránce na portálu OMS klikněte **nastavení** ikonu.

    ![Zadejte dotaz na vytvoření výstrahy](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-edit-alert.png "zadejte dotaz na vytvoření výstrahy")

6. Z **nastavení** klikněte na tlačítko **výstrahy** zobrazte výstrahy, které jste vytvořili. Můžete také povolit nebo zakázat výstrahu, upravit nebo odstranit. Další informace najdete v tématu [práce s pravidla výstrah v analýzy protokolů](../log-analytics/log-analytics-alerts-creating.md).

## <a name="see-also"></a>Viz také

* [Práce s OMS analýzy protokolů](https://blogs.msdn.microsoft.com/wei_out_there_with_system_center/2016/07/03/oms-log-analytics-create-tiles-drill-ins-and-dashboards-with-the-view-designer/)
* [Vytvořit pravidla výstrah v analýzy protokolů](../log-analytics/log-analytics-alerts-creating.md)
