---
title: "Vytvoření clusteru Spark ve službě Azure HDInsight a použití Spark SQL z Jupyteru pro interaktivní analýzu | Dokumentace Microsoftu"
description: "Podrobné pokyny, jak rychle vytvořit cluster Apache Spark v HDInsight a pak použít Spark SQL z poznámkových bloků Jupyter ke spuštění interaktivních dotazů."
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 91f41e6a-d463-4eb4-83ef-7bbb1f4556cc
ms.service: hdinsight
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 01/06/2017
ms.author: nitinme
translationtype: Human Translation
ms.sourcegitcommit: 791b6a5a07bb87302cb382290a355c9a14c63ff0
ms.openlocfilehash: cc1d484d40dce0b1c64f2e8cdebb9377a38705cb


---
# <a name="get-started-create-apache-spark-cluster-in-azure-hdinsight-and-run-interactive-queries-using-spark-sql"></a>Začínáme: Vytvoření clusteru Apache Spark ve službě Azure HDInsight a spuštění interaktivních dotazů pomocí Spark SQL
Naučte se vytvářet cluster [Apache Spark](hdinsight-apache-spark-overview.md) ve službě HDInsight a pak použijte poznámkový blok [Jupyter](https://jupyter.org) ke spouštění interaktivních dotazů Spark SQL v clusteru Spark.

   ![Začněte používat Apache Spark v HDInsight](./media/hdinsight-apache-spark-jupyter-spark-sql/hdispark.getstartedflow.png "Kurz Začněte používat Apache Spark v HDInsight Popsané postupy: vytvoření účtu úložiště; vytvoření clusteru; spuštění příkazů Spark SQL")

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="prerequisites"></a>Požadavky
* **Předplatné Azure**. Než začnete tento kurz, musíte mít předplatné Azure. Přečtěte si téma [Bezplatné vytvoření účtu Microsoft Azure ještě dnes](https://azure.microsoft.com/free).
* **Klient Secure Shell (SSH)**: systémy Linux, Unix a OS X poskytují klienta SSH pomocí příkazu `ssh`. Pro systémy Windows najdete informace v tématu [Použití SSH se systémem Linux Hadoop ve službě HDInsight ze systému Windows pomocí klienta PuTTY](hdinsight-hadoop-linux-use-ssh-windows.md). Pro systémy Linux, Unix a OS X najdete informace v tématu [Použití SSH se systémem Linux Hadoop ve službě HDInsight ze systému OS X, Linux a Unix](hdinsight-hadoop-linux-use-ssh-unix.md).

> [!NOTE]
> Tento článek používá šablonu Azure Resource Manageru k vytvoření clusteru Spark, který využívá [Azure Storage Bloby jako úložiště clusteru](hdinsight-hadoop-use-blob-storage.md). Můžete také vytvořit cluster Spark, který používá [Azure Data Lake Store](../data-lake-store/data-lake-store-overview.md) jako další úložiště, navíc k úložišti objektů blob Azure jako výchozího úložiště. Pokyny naleznete v tématu [Vytvoření clusteru HDInsight pomocí Data Lake Store](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md).
>
>

### <a name="access-control-requirements"></a>Požadavky na řízení přístupu
[!INCLUDE [access-control](../../includes/hdinsight-access-control-requirements.md)]

## <a name="create-spark-cluster"></a>Vytvoření clusteru Spark
V této části vytvoříte cluster Spark ve službě HDInsight pomocí [šablony Azure Resource Manageru](https://azure.microsoft.com/resources/templates/101-hdinsight-spark-linux/). Informace o verzích HDInsight a jejich SLA naleznete v tématu [Správa verzí komponenty HDInsight](hdinsight-component-versioning.md). Ostatní metody tvorby clusteru najdete v části [Tvorba clusterů HDInsight](hdinsight-hadoop-provision-linux-clusters.md).

1. Kliknutím na následující obrázek otevřete šablonu na portálu Azure Portal.         

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-hdinsight-spark-linux%2Fazuredeploy.json" target="_blank"><img src="./media/hdinsight-apache-spark-jupyter-spark-sql/deploy-to-azure.png" alt="Deploy to Azure"></a>

2. Zadejte následující hodnoty:

    ![Vytvoření clusteru Spark ve službě HDInsight pomocí šablony Azure Resource Manageru](./media/hdinsight-apache-spark-jupyter-spark-sql/create-spark-cluster-in-hdinsight-using-azure-resource-manager-template.png "Vytvoření clusteru Spark ve službě HDInsight pomocí šablony Azure Resource Manageru")

   * **Předplatné:** Vyberte předplatné Azure pro tento cluster.
   * **Skupina prostředků:** Vytvořte novou skupinu prostředků nebo vyberte stávající. Skupina prostředků slouží ke správě prostředků Azure pro vaše projekty.
   * **Umístění:** Vyberte umístění pro skupinu prostředků.  Toto umístění se používá také pro výchozí úložiště clusteru a cluster HDInsight.
   * **Název clusteru**: Zadejte název pro cluster Hadoop, který chcete vytvořit.
   * **Přihlašovací jméno a heslo clusteru**: výchozí přihlašovací jméno je admin.
   * **Uživatelské jméno a heslo SSH**.

   Zapište tyto hodnoty.  Ty budete potřebovat později v kurzu.

3. Vyberte **Souhlasím s podmínkami a ujednáními uvedenými nahoře**, vyberte **Připnout na řídicí panel** a potom klikněte na **Koupit**. Zobrazí se nová dlaždice s názvem Odeslání nasazení pro šablonu nasazení. Vytvoření clusteru trvá přibližně 20 minut.

## <a name="run-spark-sql-queries-using-a-jupyter-notebook"></a>Spuštění dotazů Spark SQL pomocí poznámkového bloku Jupyter
V této části použijete ke spouštění dotazů Spark SQL poznámkový blok Jupyter v clusteru Spark. Clustery HDInsight Spark poskytují dvě jádra, která můžete použít s poznámkovým blokem Jupyter. Jsou to:

* **PySpark** (pro aplikace napsané v Pythonu)
* **Spark** (pro aplikace napsané v jazyce Scala)

V tomto článku budete používat jádro PySpark. V jádrech článku [, která jsou k dispozici na pro poznámkové bloky Jupyter s clustery Spark HDInsight](hdinsight-apache-spark-jupyter-notebook-kernels.md#why-should-i-use-the-pyspark-or-spark-kernels), si můžete přečíst podrobnosti o výhodách používání jádra PySpark. Pár klíčových výhod používání jádra PySpark však představuje:

* Nemusíte nastavovat kontexty pro Spark a Hive. Automaticky se nastavení za vás.
* Buňky magic můžete použít jako `%%sql` pro přímé spouštění dotazů SQL nebo Hive, bez předchozích fragmentů kódu.
* Výstup pro dotazy SQL nebo Hive se automaticky vizualizuje.

### <a name="create-jupyter-notebook-with-pyspark-kernel"></a>Vytvoření poznámkového bloku Jupyter s jádrem PySpark

1. Otevřete web [Azure Portal](https://portal.azure.com/).
2. V levé nabídce klikněte na **Skupina prostředků**.
3. Klikněte na skupinu prostředků, kterou jste vytvořili v poslední části. Pokud je skupin prostředků hodně, můžete použít funkce vyhledávání. Zobrazí se dva prostředky ve skupině, cluster HDInsight a výchozí účet úložiště.
4. Klikněte na cluster a ten se otevře.
 
2. V části **Rychlé odkazy** klikněte na **Řídicí panely clusteru** a potom klikněte na **Poznámkový blok Jupyter**. Po vyzvání zadejte přihlašovací údaje správce clusteru.

   ![Řídicí panely clusteru HDInsight](./media/hdinsight-apache-spark-jupyter-spark-sql/hdinsight-azure-portal-cluster-dashboards.png "Řídicí panely clusteru HDInsight")

   > [!NOTE]
   > Může také otevřít poznámkový blok Jupyter pro váš cluster tak, že otevřete následující adresu URL v prohlížeči. Nahraďte **CLUSTERNAME** názvem clusteru:
   >
   > `https://CLUSTERNAME.azurehdinsight.net/jupyter`
   >
   >
3. Vytvořte nový poznámkový blok. Klikněte na tlačítko **Nový** a pak klikněte na tlačítko **PySpark**.

   ![Vytvoření nového poznámkového bloku Jupyter](./media/hdinsight-apache-spark-jupyter-spark-sql/hdispark.note.jupyter.createnotebook.png "Vytvoření nového poznámkového bloku Jupyter")

   Nový poznámkový blok se vytvoří a otevře s názvem Bez názvu (Bez názvu.pynb). 

4. Pokud chcete, klikněte na název poznámkového bloku v horní části a zadejte popisný název.

    ![Zadání názvu poznámkového bloku](./media/hdinsight-apache-spark-jupyter-spark-sql/hdispark.note.jupyter.notebook.name.png "Zadání názvu poznámkového bloku")
5. Do prázdné buňky vložte následující kód a stiskněte **SHIFT + ENTER** ke spuštění kódu. Kód naimportuje typy potřebné pro tento scénář:

        from pyspark.sql.types import *

    Vzhledem k tomu, že jste poznámkový blok vytvořili pomocí jádra PySpark, není nutné explicitně tvořit kontexty. Kontexty Spark a Hive se automaticky vytvoří za vás při spuštění první buňky kódu.

    ![Stav úlohy poznámkového bloku Jupyter](./media/hdinsight-apache-spark-jupyter-spark-sql/hdispark.jupyter.job.status.png "Stav úlohy poznámkového bloku Jupyter")

    Při každém spuštění úlohy v Jupyter se název okna webového prohlížeče zobrazí jako **(Zaneprázdněn)** společně s názvem poznámkového bloku. Zobrazí se také plný kroužek vedle textu **PySpark** v pravém horním rohu. Po dokončení úlohy se změní na prázdný kruh.

6. Spuštěním následujícího kódu zaregistrujte ukázková data do dočasné tabulky s názvem **hvac**.

        # Load the data
        hvacText = sc.textFile("wasbs:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")

        # Create the schema
        hvacSchema = StructType([StructField("date", StringType(), False),StructField("time", StringType(), False),StructField("targettemp", IntegerType(), False),StructField("actualtemp", IntegerType(), False),StructField("buildingID", StringType(), False)])

        # Parse the data in hvacText
        hvac = hvacText.map(lambda s: s.split(",")).filter(lambda s: s[0] != "Date").map(lambda s:(str(s[0]), str(s[1]), int(s[2]), int(s[3]), str(s[6]) ))

        # Create a data frame
        hvacdf = sqlContext.createDataFrame(hvac,hvacSchema)

        # Register the data fram as a table to run queries against
        hvacdf.registerTempTable("hvac")

    Clustery Spark ve službě HDInsight se dodávají s ukázkovým datovým souborem **hvac.csv**, který je uložený v **\HdiSamples\HdiSamples\SensorSampleData\hvac**.
    
7. Jako dotaz na data spusťte následující kód:

        %%sql
        SELECT buildingID, (targettemp - actualtemp) AS temp_diff, date FROM hvac WHERE date = \"6/1/13\"

   Vzhledem k tomu, že používáte jádro PySpark, můžete nyní přímo spustit dotaz SQL na dočasnou tabulku **TVK**, kterou jste právě vytvořili pomocí `%%sql` magic. Další informace o `%%sql` magic a také dalších magic, které jsou k dispozici s jádrem PySpark, naleznete v části [Jádra dostupná v poznámkových blocích Jupyter s clustery Spark HDInsight](hdinsight-apache-spark-jupyter-notebook-kernels.md#why-should-i-use-the-pyspark-or-spark-kernels).

   Ve výchozím nastavení se zobrazí následující tabulkový výstup.

     ![Tabulkový výstup výsledků dotazu](./media/hdinsight-apache-spark-jupyter-spark-sql/tabular.output.png "Tabulkový výstup výsledků dotazu")

    Výsledky můžete také zobrazit v dalších vizualizacích. Například plošný graf pro stejný výstup bude vypadat následovně.

    ![Plošný graf výsledku dotazu](./media/hdinsight-apache-spark-jupyter-spark-sql/area.output.png "Plošný graf výsledku dotazu")

9. Po dokončení spuštění aplikace můžete poznámkový blok vypnout a uvolnit tak prostředky. To provedete kliknutím na položku **Zavřít a zastavit** z nabídky **Soubor** v poznámkovém bloku. Dojde k vypnutí a zavření poznámkového bloku.

## <a name="delete-the-cluster"></a>Odstranění clusteru
[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="see-also"></a>Viz také
* [Přehled: Apache Spark v Azure HDInsight](hdinsight-apache-spark-overview.md)

### <a name="scenarios"></a>Scénáře
* [Spark s BI: Provádějte interaktivní analýzy dat pomocí Sparku v HDInsight pomocí nástrojů BI](hdinsight-apache-spark-use-bi-tools.md)
* [Spark s Machine Learning: Používejte Spark v HDInsight pro analýzu teploty v budově pomocí dat HVAC](hdinsight-apache-spark-ipython-notebook-machine-learning.md)
* [Spark s Machine Learning: Používejte Spark v HDInsight k předpovědím výsledků kontrol potravin](hdinsight-apache-spark-machine-learning-mllib-ipython.md)
* [Datové proudy Spark: Používejte Spark v HDInsight pro sestavení aplikací datových proudů v reálném čase](hdinsight-apache-spark-eventhub-streaming.md)
* [Analýza protokolu webu pomocí Sparku v HDInsight](hdinsight-apache-spark-custom-library-website-log-analysis.md)
* [Analýza dat telemetrie Application Insights pomocí Sparku v HDInsight](hdinsight-spark-analyze-application-insight-logs.md)

### <a name="create-and-run-applications"></a>Vytvoření a spouštění aplikací
* [Vytvoření samostatné aplikace pomocí Scala](hdinsight-apache-spark-create-standalone-application.md)
* [Vzdálené spouštění úloh na clusteru Sparku pomocí Livy](hdinsight-apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a>Nástroje a rozšíření
* [Modul plug-in nástroje HDInsight pro IntelliJ IDEA pro vytvoření a odesílání aplikací Spark Scala](hdinsight-apache-spark-intellij-tool-plugin.md)
* [Použití modulu plug-in nástroje HDInsight pro IntelliJ IDEA pro vzdálené ladění aplikací Spark](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [Použití poznámkových bloků Zeppelin s clusterem Sparku v HDInsight](hdinsight-apache-spark-use-zeppelin-notebook.md)
* [Jádra dostupná pro poznámkový blok Jupyter v clusteru Sparku pro HDInsight](hdinsight-apache-spark-jupyter-notebook-kernels.md)
* [Použití externích balíčků s poznámkovými bloky Jupyter](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)
* [Instalace Jupyteru do počítače a připojení ke clusteru HDInsight Spark](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Správa prostředků
* [Správa prostředků v clusteru Apache Spark v Azure HDInsight](hdinsight-apache-spark-resource-manager.md)
* [Sledování a ladění úloh spuštěných v clusteru Apache Spark v HDInsight](hdinsight-apache-spark-job-debugging.md)

[hdinsight-versions]: hdinsight-component-versioning.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[azure-management-portal]: https://manage.windowsazure.com/
[azure-create-storageaccount]: storage-create-storage-account.md



<!--HONumber=Jan17_HO2-->


