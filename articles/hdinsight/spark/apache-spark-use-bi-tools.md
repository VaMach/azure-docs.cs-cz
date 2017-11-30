---
title: "Spark BI pomocí nástrojů vizualizace data v Azure HDInsight | Microsoft Docs"
description: "Pomocí nástrojů vizualizace dat pro analýzu pomocí Apache Spark BI v clusterech prostředí HDInsight"
keywords: Apache spark bi, spark bi vizualizace dat spark, spark business intelligence
services: hdinsight
documentationcenter: 
author: mumian
manager: cgronlun
editor: cgronlun
tags: azure-portal
ms.assetid: 1448b536-9bc8-46bc-bbc6-d7001623642a
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/24/2017
ms.author: jgao
ms.openlocfilehash: fabf48da80cf44e82068d180c896ebbca7078d18
ms.sourcegitcommit: 29bac59f1d62f38740b60274cb4912816ee775ea
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/29/2017
---
# <a name="apache-spark-bi-using-data-visualization-tools-with-azure-hdinsight"></a>Apache Spark BI nástroje vizualizaci dat pomocí Azure HDInsight

Naučte se používat Power BI a Tableau k vizualizaci dat v cluster Apache Spark v Azure HDInsight.

## <a name="prerequisites"></a>Požadavky

* Cluster Apache Spark v HDInsight. Pokyny najdete v tématu [clusterů vytvořit Apache Spark v Azure HDInsight](apache-spark-jupyter-spark-sql.md).
* Ukázková data v clusteru. Pokyny najdete v tématu [Spusťte interaktivní dotazy na clusteru HDInsight Spark](apache-spark-load-data-run-query.md).
* Power BI: [Power BI Desktop](https://powerbi.microsoft.com/en-us/desktop/) a [zkušební předplatné Power BI](https://app.powerbi.com/signupredirect?pbi_source=web) (volitelné).
* Tableau: [Tableau plochy](http://www.tableau.com/products/desktop) a [ovladač Microsoft Spark ODBC](http://go.microsoft.com/fwlink/?LinkId=616229).


## <a name="hivetable"></a>Přečtěte si ukázková data

Poznámkového bloku Jupyter, který jste vytvořili v [předchozí kurzu](apache-spark-load-data-run-query.md) obsahuje kód k vytvoření `hvac` tabulky. Tato tabulka podle souboru CSV, k dispozici na všech clusterech HDInsight Spark v **\HdiSamples\HdiSamples\SensorSampleData\hvac\hvac.csv**. Pojďme si shrnout data v clusteru Spark před vytvořením vizualizace.

1. Ověřte, že očekávané tabulky existuje. Do prázdné buňky v poznámkovém bloku, zkopírujte následující fragment kódu a stiskněte klávesu **SHIFT + ENTER**.

        %%sql
        SHOW TABLES

    Zobrazí výstup podobný jako vidíte níže:

    ![Zobrazení tabulky v Spark](./media/apache-spark-use-bi-tools/show-tables.png)

    Pokud jste zavřeli poznámkového bloku před zahájením tohoto kurzu `hvactemptable` je vyčištěna, takže není zahrnut ve výstupu.
    Pouze tabulky, které jsou uložené v metaúložiště hive (indikován **False** pod **isTemporary** sloupec) lze přistupovat z nástrojů BI. V tomto kurzu jsme se připojit ke **TVK** jsme vytvořili tabulku.

2. Ověřte, zda tabulka obsahuje očekávaná data. Do prázdné buňky v poznámkovém bloku, zkopírujte následující fragment kódu a stiskněte klávesu **SHIFT + ENTER**.

        %%sql
        SELECT * FROM hvac LIMIT 10

    Zobrazí výstup podobný jako vidíte níže:

    ![Zobrazit řádky z tabulky TVK v Spark](./media/apache-spark-use-bi-tools/select-limit.png)

3. Vypněte poznámkového bloku k uvolnění prostředků. To provedete kliknutím na položku **Zavřít a zastavit** z nabídky **Soubor** v poznámkovém bloku.

## <a name="powerbi"></a>Pomocí Power BI pro vizualizaci dat Spark

Teď, když ověříte, že existuje očekávaná data, můžete vytvářet vizualizace, sestavy a řídicí panely z těchto dat Power BI. V tomto článku jsme vám zobrazit jednoduchý příklad s statických dat, ale dejte nám vědět v komentářích, pokud chcete složitější streamování příklady.

### <a name="create-a-report-in-power-bi-desktop"></a>Vytvoření sestavy v Power BI Desktop
První kroky při práci se službou Spark se připojte ke clusteru v Power BI Desktop, načtení dat z clusteru a vytvořit základní vizualizace založené na těchto datech.

> [!NOTE]
> Konektor ukázáno v tomto článku je aktuálně ve verzi preview, ale doporučujeme vám použít ji a poskytování zpětné vazby máte prostřednictvím [Power BI komunity](https://community.powerbi.com/) lokality nebo [Power BI nápady](https://ideas.powerbi.com/forums/265200-power-bi-ideas).

1. V Power BI Desktop na **Domů** , klikněte na **načíst Data**, pak **Další**.

2. Vyhledejte `Spark`, vyberte **Azure HDInsight Spark**, pak klikněte na tlačítko **Connect**.

    ![Načíst data do Power BI z Apache Spark BI](./media/apache-spark-use-bi-tools/apache-spark-bi-import-data-power-bi.png "načíst data do Power BI z Apache Spark BI")

3. Zadejte adresu URL clusteru (ve formě `mysparkcluster.azurehdinsight.net`), vyberte **DirectQuery**, pak klikněte na tlačítko **OK**.

    ![Připojení k serveru Apache Spark BI](./media/apache-spark-use-bi-tools/connect-to-apache-spark-bi.png "připojit k serveru Apache Spark BI")

    > [!NOTE]
    > Můžete buď režim připojení s Spark. Pokud používáte DirectQuery, změny se projeví v sestavách bez obnovení celou datovou sadu. Pokud importujete data, je nutné aktualizovat sadu dat a podívejte se změny. Další informace, jak a kdy použít DirectQuery najdete v tématu [pomocí DirectQuery v Power BI](https://powerbi.microsoft.com/documentation/powerbi-desktop-directquery-about/). 

4. Zadejte informace o účtu přihlášení HDInsight **uživatelské jméno** a **heslo** (výchozí účet je `admin`), pak klikněte na tlačítko **Connect**.

    ![Spark clusteru uživatelské jméno a heslo](./media/apache-spark-use-bi-tools/user-password.png "Spark clusteru uživatelské jméno a heslo")

5. Vyberte `hvac` tabulky a počkejte zobrazíte náhled data. Pak klikněte na tlačítko **zatížení**.

    ![Spark clusteru uživatelské jméno a heslo](./media/apache-spark-use-bi-tools/apache-spark-bi-select-table.png "Spark clusteru uživatelské jméno a heslo")

    Power BI Desktop teď má všechny informace, musí se připojit k Spark clusteru a načtení dat z `hvac` tabulky. Tabulka a její sloupce jsou zobrazeny v **pole** podokně.

    ![Seznam tabulek na řídicí panel Apache Spark BI](./media/apache-spark-use-bi-tools/apache-spark-bi-display-tables.png "seznam tabulek na řídicí panel Apache Spark BI")

7. Vytvořit vizualizaci zobrazit odchylka mezi teploty cíl a skutečný teploty pro každé sestavení: 

    1. V **VIZUALIZACE** podokně, vyberte **plošný graf**. Přetáhněte **BuildingID** do **osy**a přetáhněte ji **ActualTemp** a **TargetTemp** polí k **hodnotu**.

        ![Vytvořit vizualizaci dat pomocí Apache Spark BI Spark](./media/apache-spark-use-bi-tools/apache-spark-bi-add-value-columns.png "Spark vytvořit vizualizaci dat pomocí Apache Spark BI")

    2. Ve výchozím nastavení zobrazuje vizualizaci součet **ActualTemp** a **TargetTemp**. Pro obě pole, z rozevíracího seznamu vyberte **průměrná** získat v průměru skutečné a teploty cíl pro každé sestavení.

        ![Vytvořit vizualizaci dat pomocí Apache Spark BI Spark](./media/apache-spark-use-bi-tools/apache-spark-bi-average-of-values.png "Spark vytvořit vizualizaci dat pomocí Apache Spark BI")

    3. Vaše vizualizace dat by měla být podobné na snímku obrazovky. Přesuňte kurzor vizualizaci zobrazíte popisy s příslušnými daty.

        ![Vytvořit vizualizaci dat pomocí Apache Spark BI Spark](./media/apache-spark-use-bi-tools/apache-spark-bi-area-graph.png "Spark vytvořit vizualizaci dat pomocí Apache Spark BI")

11. Klikněte na tlačítko **soubor** pak **Uložit**a zadejte název `spark.pbix` souboru. 

### <a name="publish-the-report-to-the-power-bi-service-optional"></a>Sestava bude publikována ve službě Power BI (volitelné)
Teď máte plně funkční sestavy v Power BI Desktop, můžete zastavit existuje, ale mnoho uživatelé chtějí využít služby Power BI, který umožňuje snadno sdílet sestavy a řídicí panely v rámci vaší organizace. 

V této části Publikování datové sady a sestavy, který je obsažen v Power BI Desktop soubor, který jste vytvořili. Potom kód pin vizualizaci ze sestavy na řídicí panel. Řídicí panely jsou obvykle používány umožňuje zaměřit se na podmnožinu dat v sestavě; v sestavě obsahuje pouze jeden vizualizace, ale je stále užitečné projít kroky.

1. V Power BI Desktop na **Domů** , klikněte na **publikovat**.

    ![Publikování z Power BI Desktop](./media/apache-spark-use-bi-tools/apache-spark-bi-publish.png "publikování z Power BI Desktop")

2. Vyberte pracovní prostor k publikování datovou sadu a nahlásit, pak klikněte na tlačítko **vyberte**. Na následujícím obrázku, výchozí **pracovní prostor** je vybrána.

    ![Vyberte pracovní prostor k publikování datové sady a nahlásit](./media/apache-spark-use-bi-tools/apache-spark-bi-select-workspace.png "vyberte pracovní prostor k publikování datové sady a nahlásit") 

3. Po návratu Power BI Desktop s zpráva o úspěšném provedení klikněte na tlačítko **otevřete 'spark.pbix' v Power BI**.

    ![Publikovat úspěšné, klikněte na tlačítko zadat přihlašovací údaje](./media/apache-spark-use-bi-tools/apache-spark-bi-publish-success.png "publikovat úspěšné, klikněte na tlačítko zadat přihlašovací údaje") 

4. V rámci služby Power BI, klikněte na **zadejte přihlašovací údaje**.

    ![Zadejte přihlašovací údaje ve službě Power BI](./media/apache-spark-use-bi-tools/apache-spark-bi-enter-credentials.png "zadejte přihlašovací údaje ve službě Power BI")

5. Klikněte na tlačítko **upravit přihlašovací údaje**.

    ![Upravit přihlašovací údaje do služby Power BI](./media/apache-spark-use-bi-tools/apache-spark-bi-edit-credentials.png "upravit přihlašovací údaje do služby Power BI")

6. Zadejte přihlašovací údaje k účtu HDInsight (obvykle výchozí `admin` účet používaný v Power BI Desktop), pak klikněte na tlačítko **přihlášení**.

    ![Přihlaste se k clusteru Spark](./media/apache-spark-use-bi-tools/apache-spark-bi-sign-in.png "Přihlaste se k clusteru Spark")

7. V levém podokně přejděte do **pracovních prostorů** > **pracovní prostor** > **sestavy**, pak klikněte na tlačítko **spark**.

    ![Sestava uvedené v části sestavy v levém podokně](./media/apache-spark-use-bi-tools/apache-spark-bi-service-left-pane.png "sestavy uvedené v části sestavy v levém podokně")

    Měli byste taky vidět **spark** uvedené v části **datové sady** v levém podokně.

8. Vizuál, který jste vytvořili v Power BI Desktop je nyní k dispozici ve službě. Chcete-li připnout na řídicí panel tento vizuál, najeďte myší visual a kliknutím na ikonu Připnutí.

    ![Sestavy v rámci služby Power BI](./media/apache-spark-use-bi-tools/apache-spark-bi-service-report.png "sestavy ve službě Power BI")

9. Vyberte "Novým řídicím panelem", zadejte název `SparkDemo`, pak klikněte na tlačítko **Pin**.

    ![Připnout na řídicí panel nové](./media/apache-spark-use-bi-tools/apache-spark-bi-pin-dashboard.png "připnout na řídicí panel nové")

10. V sestavě, klikněte na tlačítko **přejděte na řídicí panel**. 

    ![Přejděte na řídicí panel](./media/apache-spark-use-bi-tools/apache-spark-bi-open-dashboard.png "přejděte na řídicí panel")

Vizuál je připnutá na řídicí panel - můžete přidat jiných vizuálních prvcích do sestavy a připnete ji na stejné řídicí panel. Další informace o sestavy a řídicí panely, najdete v části [sestavy v Power BI](https://powerbi.microsoft.com/documentation/powerbi-service-reports/)a [řídicí panely v Power BI](https://powerbi.microsoft.com/documentation/powerbi-service-dashboards/).

## <a name="tableau"></a>Použijte plochu Tableau pro vizualizaci dat Spark

> [!NOTE]
> Tato část je určena pouze pro clustery Spark 1.5.2 vytvořené v Azure HDInsight.
>
>

1. Nainstalujte [Tableau plochy](http://www.tableau.com/products/desktop) na počítači, na kterém je spuštěn v tomto kurzu Apache Spark BI.

2. Ujistěte se, že tento počítač má také nainstalovaný ovladač Microsoft Spark ODBC. Můžete nainstalovat ovladač z [zde](http://go.microsoft.com/fwlink/?LinkId=616229).

1. Spuštění Tableau plochy. V levém podokně, ze seznamu serveru pro připojení, klikněte na tlačítko **Spark SQL**. Pokud není ve výchozím nastavení v levém podokně zobrazí Spark SQL, můžete nějakého najít kliknutím **více serverů**.
2. V dialogovém okně připojení Spark SQL zadejte hodnoty, jak je znázorněno na snímku obrazovky a pak klikněte na tlačítko **OK**.

    ![Připojení ke clusteru Apache Spark BI](./media/apache-spark-use-bi-tools/connect-to-tableau-apache-spark-bi.png "připojení ke clusteru Apache Spark BI")

    Rozevírací seznamy ověřování **služby Microsoft Azure HDInsight** jako možnost, pouze pokud jste nainstalovali [ovladač ODBC Microsoft Sparku](http://go.microsoft.com/fwlink/?LinkId=616229) v počítači.
3. Na další obrazovce z **schématu** rozevírací seznam, klikněte **najít** ikonu a pak klikněte na tlačítko **výchozí**.

    ![Nalezeno schéma pro Apache Spark BI](./media/apache-spark-use-bi-tools/tableau-find-schema-apache-spark-bi.png "najít schéma pro Apache Spark BI")
4. Pro **tabulky** pole, klikněte na tlačítko **najít** ikonu seznam všech tabulek Hive v clusteru k dispozici. Měli byste vidět **TVK** tabulky, které jste vytvořili dříve pomocí poznámkového bloku.

    ![Najít tabulku pro Apache Spark BI](./media/apache-spark-use-bi-tools/tableau-find-table-apache-spark-bi.png "najít tabulku pro Apache Spark BI")
5. Přetáhnout myší v tabulce nejvyšší pole na pravé straně. Tableau naimportuje data a schéma se zobrazuje jako zvýrazněná podle červeným rámečkem.

    ![Přidání tabulky do Tableau pro Apache Spark BI](./media/apache-spark-use-bi-tools/tableau-add-table-apache-spark-bi.png "přidání tabulky do Tableau pro Apache Spark BI")
6. Klikněte **Sheet1** kartě dole vlevo. Ujistěte se, vizualizace, který ukazuje průměrná cíl a skutečný teploty pro všechny budovy pro jednotlivá data. Přetáhněte **datum** a **vytváření ID** k **sloupce** a **skutečné Temp**/**cíle Temp** k **řádky**. V části **značky**, vyberte **oblasti** sloužící oblasti mapy pro vizualizaci dat Spark.

     ![Přidat pole pro vizualizaci dat Spark](./media/apache-spark-use-bi-tools/spark-data-visualization-add-fields.png "přidat pole pro vizualizaci dat Spark")
7. Standardně se zobrazují pole teploty jako agregace. Pokud chcete zobrazit průměrné teploty místo, můžete provést tak z rozevíracího seznamu, jak je uvedeno níže.

    ![Trvat průměrná z teploty pro vizualizaci dat Spark](./media/apache-spark-use-bi-tools/spark-data-visualization-average-temperature.png "trvat průměrná z teploty pro vizualizaci dat Spark")

8. Můžete také super použít jedna mapa teploty z nich získat lepší chování rozdíl mezi cíl a skutečný teploty. Přesuňte myš do rohu nižší oblasti mapy, dokud najdete v části obrazce popisovač zvýrazněných v červeném kroužku. Přetáhněte mapy do jiných mapy nahoře a uvolnění tlačítka myši, když se tvar zvýrazněných v red obdélníku.

    ![Sloučení mapy pro vizualizaci dat Spark](./media/apache-spark-use-bi-tools/spark-data-visualization-merge-maps.png "sloučení mapy pro vizualizaci dat Spark")

     Vaše vizualizace dat měli změnit, jak je znázorněno na snímku obrazovky:

    ![Výstup tableau vizualizace dat Spark](./media/apache-spark-use-bi-tools/spark-data-visualization-tableau-output.png "Tableau výstup vizualizace dat Spark")
9. Klikněte na tlačítko **Uložit** uložit listu. Můžete vytvořit řídicí panely a do ní přidejte jeden nebo více stylů.

## <a name="next-steps"></a>Další kroky

Pokud jste zjistili, jak vytvořit cluster, vytvořit Spark datové rámce k dotazování na data a pak z nástrojů BI přístup k datům. Teď můžete prohlížet pokyny, jak spravovat prostředky clusteru a ladění úloh, které jsou spuštěné v clusteru služby HDInsight Spark.

* [Správa prostředků v clusteru Apache Spark v Azure HDInsight](apache-spark-resource-manager.md)
* [Sledování a ladění úloh spuštěných v clusteru Apache Spark v HDInsight](apache-spark-job-debugging.md)

