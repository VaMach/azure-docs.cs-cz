---
title: "Spark BI pomocí nástrojů vizualizace data v Azure HDInsight | Microsoft Docs"
description: "Pomocí nástrojů vizualizace dat pro analýzu pomocí Apache Spark BI v clusterech prostředí HDInsight"
keywords: Apache spark bi, spark bi vizualizace dat spark, spark business intelligence
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 1448b536-9bc8-46bc-bbc6-d7001623642a
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/21/2017
ms.author: nitinme
ms.openlocfilehash: d1d5405a635b9f990f53b2bf32c8270a71a0f344
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/03/2017
---
# <a name="apache-spark-bi-using-data-visualization-tools-with-azure-hdinsight"></a>Apache Spark BI nástroje vizualizaci dat pomocí Azure HDInsight

Další informace o použití nástroje vizualizaci dat, jako je Power BI a Tableau k analýze nezpracovaná ukázkové sady dat pomocí Apache Spark BI v clusterech prostředí HDInsight.

> [!NOTE]
> 2.1 Spark v Azure HDInsight 3.6 Preview nepodporuje připojení pomocí nástrojů BI, které jsou popsané v tomto článku. Pouze Spark verze 1.6 a 2.0 (HDInsight 3,4, 3.5 v uvedeném pořadí) jsou podporovány.
>

V tomto kurzu je také k dispozici jako poznámkový blok Jupyter v clusteru HDInsight Spark. Poznámkový blok prostředí vám umožní spustit fragmenty Python ze samotného poznámkového bloku. Chcete-li provést kurzem z v rámci Poznámkový blok, vytvořte Spark cluster, spusťte poznámkového bloku Jupyter (`https://CLUSTERNAME.azurehdinsight.net/jupyter`), a poté spusťte poznámkového bloku **nástrojů BI použití s Apache Spark v HDInsight.ipynb** v části **Python** složky.

## <a name="prerequisites"></a>Požadavky

* Cluster Apache Spark v HDInsight. Pokyny najdete v tématu [clusterů vytvořit Apache Spark v Azure HDInsight](apache-spark-jupyter-spark-sql.md).


## <a name="hivetable"></a>Příprava dat pro vizualizaci dat Spark

V této části používáme [Jupyter](https://jupyter.org) poznámkového bloku z clusteru služby HDInsight Spark ke spuštění úloh, které zpracování nezpracovaných ukázkových dat a uložte ho jako tabulku. Ukázková data je soubor .csv (hvac.csv) k dispozici na všech clusterech ve výchozím nastavení. Vaše data uložena jako tabulku, v další části používáme nástrojů BI k připojení do tabulky a vizualizaci dat.

> [!NOTE]
> Pokud po dokončení podle pokynů v provádíte kroky v tomto článku [Spusťte interaktivní dotazy na clusteru HDInsight Spark](apache-spark-load-data-run-query.md), můžete přeskočit na krok 8 níže.
>

1. Z portálu [Azure Portal](https://portal.azure.com/) z úvodního panelu klikněte na dlaždici pro váš cluster Spark (pokud je připnutý na úvodní panel). Můžete také přejít na cluster pod položkou **Procházet vše** > **Clustery HDInsight**.   

2. Z okna clusteru Spark klikněte na **Řídicí panel clusteru** a poté na **Poznámkový blok Jupyter**. Po vyzvání zadejte přihlašovací údaje správce clusteru.

   > [!NOTE]
   > Může také otevřít poznámkový blok Jupyter pro váš cluster tak, že otevřete následující adresu URL v prohlížeči. Nahraďte **CLUSTERNAME** názvem clusteru:
   >
   > `https://CLUSTERNAME.azurehdinsight.net/jupyter`
   >
   >

3. Vytvořte poznámkový blok. Klikněte na tlačítko **Nový** a pak klikněte na tlačítko **PySpark**.

    ![Vytvoření poznámkového bloku Jupyter pro Apache Spark BI](./media/apache-spark-use-bi-tools/create-jupyter-notebook-for-spark-bi.png "vytvoření poznámkového bloku Jupyter pro Apache Spark BI")

4. Nový poznámkový blok se vytvoří a otevře s názvem Untitled.pynb. Klikněte na název poznámkového bloku v horní části a zadejte popisný název.

    ![Zadejte název poznámkového bloku pro Apache Spark BI](./media/apache-spark-use-bi-tools/jupyter-notebook-name-for-spark-bi.png "zadejte název poznámkového bloku pro Apache Spark BI")

5. Vzhledem k tomu, že jste poznámkový blok vytvořili pomocí jádra PySpark, není nutné explicitně tvořit kontexty. Kontexty Spark a Hive se automaticky vytvoří za vás při spuštění první buňky kódu. Můžete začít importem typů nezbytných pro tento scénář. Uděláte to tak, umístěte kurzor do buňky a stiskněte klávesu **SHIFT + ENTER**.

        from pyspark.sql import *

6. Načtěte vzorová data do dočasné tabulky. Když vytvoříte cluster Spark v HDInsight, ukázkový datový soubor **hvac.csv** se zkopíruje do přidruženého účtu úložiště na adrese **\HdiSamples\HdiSamples\SensorSampleData\hvac**.

    Do prázdné buňky vložte následující fragment kódu a stiskněte klávesu **SHIFT + ENTER**. Tento fragment kódu registruje data do tabulce s názvem **TVK**.

        # Create an RDD from sample data
        hvacText = sc.textFile("wasb:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")

        # Create a schema for our data
        Entry = Row('Date', 'Time', 'TargetTemp', 'ActualTemp', 'BuildingID')

        # Parse the data and create a schema
        hvacParts = hvacText.map(lambda s: s.split(',')).filter(lambda s: s[0] != 'Date')
        hvac = hvacParts.map(lambda p: Entry(str(p[0]), str(p[1]), int(p[2]), int(p[3]), int(p[6])))

        # Infer the schema and create a table       
        hvacTable = sqlContext.createDataFrame(hvac)
        hvacTable.registerTempTable('hvactemptable')
        dfw = DataFrameWriter(hvacTable)
        dfw.saveAsTable('hvac')

7. Ověřte, že tabulka byla úspěšně vytvořena. Můžete použít `%%sql` magic ke spuštění Hive dotazuje přímo. Další informace o magických příkazech `%%sql` a dalších magických příkazech, které jsou k dispozici s jádrem PySpark, najdete v části [Jádra dostupná v poznámkových blocích Jupyter s clustery Spark HDInsight](apache-spark-jupyter-notebook-kernels.md#parameters-supported-with-the-sql-magic).

        %%sql
        SHOW TABLES

    Zobrazí výstup jako vidíte níže:

        +---------------+-------------+
        |tableName      |isTemporary  |
        +---------------+-------------+
        |hvactemptable  |true        |
        |hivesampletable|false        |
        |hvac           |false        |
        +---------------+-------------+

    Pouze tabulky, které mají hodnotu false v rámci **isTemporary** sloupce jsou tabulek hive, které jsou uložené v metaúložiště a je přístupný z nástrojů BI. V tomto kurzu jsme se připojit ke **TVK** jsme vytvořili tabulku.

8. Ověřte, zda tabulka obsahuje určený data. Do prázdné buňky v poznámkovém bloku, zkopírujte následující fragment kódu a stiskněte klávesu **SHIFT + ENTER**.

        %%sql
        SELECT * FROM hvac LIMIT 10

9. Vypněte poznámkového bloku k uvolnění prostředků. To provedete kliknutím na položku **Zavřít a zastavit** z nabídky **Soubor** v poznámkovém bloku.

## <a name="powerbi"></a>Pomocí Power BI pro vizualizaci dat Spark

> [!NOTE]
> Tato část je určena pouze pro 1.6 Spark na HDInsight 3.4 a 2.0 Spark na HDInsight 3.5.
>
>

Po uložení dat jako tabulku, můžete Power BI a připojte se k datům vizualizovat má vytvořit sestavy, řídicí panely, atd.

1. Zkontrolujte, zda že máte přístup k Power BI. Můžete získat bezplatnou verzi preview odběr služby Power BI z [http://www.powerbi.com/](http://www.powerbi.com/).

2. Přihlaste se k [Power BI](http://www.powerbi.com/).

3. V dolní části v levém podokně klikněte na tlačítko **načíst Data**.

4. Na **načíst Data** v části **Import nebo připojení k datům**, pro **databáze**, klikněte na tlačítko **získat**.

    ![Získání dat do Power BI pro Apache Spark BI](./media/apache-spark-use-bi-tools/apache-spark-bi-import-data-power-bi.png "načíst data do Power BI pro Apache Spark BI")

5. Na další obrazovce klikněte na tlačítko **Spark v Azure HDInsight** a pak klikněte na **Connect**. Po zobrazení výzvy zadejte adresu URL clusteru (`mysparkcluster.azurehdinsight.net`) a pověření pro připojení ke clusteru.

    ![Připojení k serveru Apache Spark BI](./media/apache-spark-use-bi-tools/connect-to-apache-spark-bi.png "připojit k serveru Apache Spark BI")

    Po navázání připojení Power BI spustí import dat z clusteru Spark v HDInsight.

6. Power BI naimportuje data a přidá **Spark** datovou sadu v části **datové sady** záhlaví. Klikněte na datovou sadu otevřete do nového listu, která bude vizualizovat data. Můžete také list uložíte jako sestavu. Uložení na listu z **soubor** nabídky, klikněte na tlačítko **Uložit**.

    ![Apache Spark BI dlaždice na řídicím panelu Power BI](./media/apache-spark-use-bi-tools/apache-spark-bi-tile-dashboard.png "Apache Spark BI dlaždice na řídicím panelu Power BI")
7. Všimněte si, že **pole** seznamu na pravé seznamy **TVK** tabulky, které jste vytvořili dříve. Rozbalte položku v tabulce najdete pole v tabulce, jak jsou definovány v poznámkovém bloku dříve.

      ![Seznam tabulek na řídicí panel Apache Spark BI](./media/apache-spark-use-bi-tools/apache-spark-bi-display-tables.png "seznam tabulek na řídicí panel Apache Spark BI")

8. Vytvořte vizualizaci zobrazit odchylka mezi teploty cíl a skutečný teploty pro každé sestavení. Vizualizovat data, vyberte **plošný graf** (zobrazené v červeným rámečkem). K definování ose přetažení myší **BuildingID** pole v části **osy**, a **ActualTemp**/**TargetTemp** pole v části **hodnotu**.

    ![Vytvořit vizualizaci dat pomocí Apache Spark BI Spark](./media/apache-spark-use-bi-tools/apache-spark-bi-add-value-columns.png "Spark vytvořit vizualizaci dat pomocí Apache Spark BI")

9. Ve výchozím nastavení zobrazuje vizualizaci součet **ActualTemp** a **TargetTemp**. Pro obě pole, z rozevíracího seznamu vyberte **průměrná** získat v průměru skutečné a teploty cíl pro obě budovy.

    ![Vytvořit vizualizaci dat pomocí Apache Spark BI Spark](./media/apache-spark-use-bi-tools/apache-spark-bi-average-of-values.png "Spark vytvořit vizualizaci dat pomocí Apache Spark BI")

10. Vaše vizualizace dat by měla být podobné na snímku obrazovky. Přesuňte kurzor vizualizaci zobrazíte popisy s příslušnými daty.

    ![Vytvořit vizualizaci dat pomocí Apache Spark BI Spark](./media/apache-spark-use-bi-tools/apache-spark-bi-area-graph.png "Spark vytvořit vizualizaci dat pomocí Apache Spark BI")

11. Klikněte na tlačítko **Uložit** v hlavní nabídce a zadejte název sestavy. Můžete taky připnout vizuál. Pokud připnete vizualizace, je uložený na řídicím panelu, můžete sledovat nejpozdější hodnotu na první pohled.

   Můžete přidat tolik vizualizace, jako má stejné datové sady a jejich připnout na řídicí panel pro snímek vaše data. Navíc clustery Spark v HDInsight jsou připojená k Power BI s protokolem direct connect. Tím se zajistí, že Power BI vždy obsahuje nejnovější data z clusteru, takže není potřeba naplánovat aktualizace pro datovou sadu.

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

