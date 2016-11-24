---
title: "Vytvoření clusteru Spark v systému HDInsight Linux a použití Spark SQL z Jupyteru pro interaktivní analýzu | Dokumentace Microsoftu"
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
ms.date: 10/28/2016
ms.author: nitinme
translationtype: Human Translation
ms.sourcegitcommit: cc59d7785975e3f9acd574b516d20cd782c22dac
ms.openlocfilehash: d2109a45249e4e362cd87f730683ef87f1411b86


---
# <a name="get-started-create-apache-spark-cluster-on-hdinsight-linux-and-run-interactive-queries-using-spark-sql"></a>Začínáme: vytvořte cluster Apache Spark na HDInsight Linux a spusťte interaktivní dotazy pomocí Spark SQL
Naučte se vytvářet cluster Apache Spark v HDInsight a pak použijte poznámkový blok [Jupyter](https://jupyter.org) ke spouštění interaktivních dotazů Spark SQL na clusteru Spark.

   ![Začněte používat Apache Spark v HDInsight](./media/hdinsight-apache-spark-jupyter-spark-sql/hdispark.getstartedflow.png "Get started using Apache Spark in HDInsight tutorial. Steps illustrated: create a storage account; create a cluster; run Spark SQL statements")

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="prerequisites"></a>Požadavky
* **Předplatné Azure**. Než začnete tento kurz, musíte mít předplatné Azure. Viz [Získání bezplatné zkušební verze Azure](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* **Klient Secure Shell (SSH)**: systémy Linux, Unix a OS X poskytují klienta SSH pomocí příkazu `ssh`. Pro systémy Windows doporučujeme [PuTTY](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).
* **Klíče Secure Shell (SSH) (volitelné)**: účet SSH používaný k připojení ke clusteru pomocí hesla nebo veřejného klíče můžete zabezpečit. Pomocí hesel budete moci rychle začít a tuto možnost doporučujeme použít, pokud chcete rychle vytvořit cluster a spustit některé testovací úlohy. Použití klíče je bezpečnější, ale vyžaduje další nastavení. Tuto metodu můžete chtít použít při vytváření clusteru výroby. V tomto článku používáme přístup s heslem. Pokyny k vytvoření a použití klíčů SSH s HDInsight naleznete v následujících článcích:

  * Z počítače se systémem Linux – [Použití SSH s linuxovým systémem HDInsight (Hadoop) z OS X, Linux a Unix](hdinsight-hadoop-linux-use-ssh-unix.md).
  * Z počítače se systémem Windows – [Použití SSH s linuxovým systémem HDInsight (Hadoop) z Windows](hdinsight-hadoop-linux-use-ssh-windows.md).

> [!NOTE]
> Tento článek používá šablonu Azure Resource Manageru k vytvoření clusteru Spark, který využívá [služby Azure Storage Blob jako úložiště clusteru](hdinsight-hadoop-use-blob-storage.md). Můžete také vytvořit cluster Spark, který používá [Azure Data Lake Store](../data-lake-store/data-lake-store-overview.md) jako další úložiště, navíc k úložišti objektů blob Azure jako výchozího úložiště. Pokyny naleznete v tématu [Vytvoření clusteru HDInsight pomocí Data Lake Store](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md).
>
>

### <a name="access-control-requirements"></a>Požadavky na řízení přístupu
[!INCLUDE [access-control](../../includes/hdinsight-access-control-requirements.md)]

## <a name="create-spark-cluster"></a>Vytvoření clusteru Spark
V této části vytvoříte cluster HDInsight verze 3.4 (Spark verze 1.6.1) pomocí šablony Azure Resource Manageru. Informace o verzích HDInsight a jejich SLA naleznete v tématu [Správa verzí komponenty HDInsight](hdinsight-component-versioning.md). Ostatní metody tvorby clusteru najdete v části [Tvorba clusterů HDInsight](hdinsight-hadoop-provision-linux-clusters.md).

1. Kliknutím na následující obrázek otevřete šablonu na webu Azure Portal.         

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Farmtemplates%2Fcreate-linux-based-spark-cluster-in-hdinsight.json" target="_blank"><img src="https://acom.azurecomcdn.net/80C57D/cdn/mediahandler/docarticles/dpsmedia-prod/azure.microsoft.com/en-us/documentation/articles/hdinsight-hbase-tutorial-get-started-linux/20160201111850/deploy-to-azure.png" alt="Deploy to Azure"></a>

    Šablona je umístěna v kontejneru veřejného objektu blob *https://hditutorialdata.blob.core.windows.net/armtemplates/create-linux-based-spark-cluster-in-hdinsight.json*.
2. Z okna Parametry zadejte následující údaje:

   * **Název clusteru**: Zadejte název pro cluster Hadoop, který chcete vytvořit.
   * **Přihlašovací jméno a heslo clusteru**: výchozí přihlašovací jméno je admin.
   * **Uživatelské jméno a heslo SSH**.

     Zapište tyto hodnoty.  Ty budete potřebovat později v kurzu.

     > [!NOTE]
     > SSH se používá pro vzdálený přístup ke clusteru HDInsight pomocí příkazového řádku. Uživatelské jméno a heslo, které zde můžete použít, se používá při připojování ke clusteru prostřednictvím SSH. Uživatelské jméno SSH musí být také jedinečné, jelikož vytvoří uživatelský účet na všech uzlech clusteru HDInsight. Následují některé názvy účtů vyhrazené pro služby v clusteru, které nelze použít jako uživatelské jméno SSH:
     >
     > root, hdiuser, storm, hbase, ubuntu, zookeeper, hdfs, yarn, mapred, hbase, hive, oozie, falcon, sqoop, admin, tez, hcat, hdinsight-zookeeper.
     >
     > Další informace o použití SSH s HDInsight naleznete v následujících článcích:
     >
     > * [Použití SSH se systémem Linux Hadoop ve službě HDInsight ze systému OS X, Linux a Unix](hdinsight-hadoop-linux-use-ssh-unix.md).
     > * [Použití SSH se systémem Linux Hadoop ve službě HDInsight ze systému Windows](hdinsight-hadoop-linux-use-ssh-windows.md).
     >
     >

3. Klikněte na možnost **OK** a uložte parametry.

4. Z okna **Vlastní nasazení** klikněte na rozevírací pole **Skupina prostředků** a pak klikněte na tlačítko **Nový** a vytvořte novou skupinu prostředků. Skupina prostředků je kontejner, který seskupuje cluster, účet závislého úložiště a další propojené prostředky skupin.

5. Klikněte na tlačítko **Smluvní podmínky** a pak klikněte na tlačítko **Vytvořit**.

6. Klikněte na **Vytvořit**. Zobrazí se nová dlaždice s názvem Odeslání nasazení pro šablonu nasazení. Vytvoření clusteru a databáze SQL trvá přibližně 20 minut.

## <a name="run-spark-sql-queries-using-a-jupyter-notebook"></a>Spuštění dotazů Spark SQL pomocí poznámkového bloku Jupyter
V této části použijete ke spouštění dotazů Spark SQL poznámkový blok Jupyter v clusteru Spark. Clustery HDInsight Spark poskytují dvě jádra, která můžete použít s poznámkovým blokem Jupyter. Jsou to:

* **PySpark** (pro aplikace napsané v Pythonu)
* **Spark** (pro aplikace napsané v jazyce Scala)

V tomto článku budete používat jádro PySpark. V jádrech článku [, která jsou k dispozici na pro poznámkové bloky Jupyter s clustery Spark HDInsight](hdinsight-apache-spark-jupyter-notebook-kernels.md#why-should-i-use-the-pyspark-or-spark-kernels), si můžete přečíst podrobnosti o výhodách používání jádra PySpark. Pár klíčových výhod používání jádra PySpark však představuje:

* Nemusíte nastavovat kontexty pro Spark a Hive. Automaticky se nastavení za vás.
* Buňky magic můžete použít jako `%%sql` pro přímé spouštění dotazů SQL nebo Hive, bez předchozích fragmentů kódu.
* Výstup pro dotazy SQL nebo Hive se automaticky vizualizuje.

### <a name="create-jupyter-notebook-with-pyspark-kernel"></a>Vytvoření poznámkového bloku Jupyter s jádrem PySpark
1. Z [Portálu Azure](https://portal.azure.com/) z úvodního panelu klikněte na dlaždici pro váš cluster Spark (pokud je připnutý na úvodní panel). Můžete také přejít na cluster pod položkou **Procházet vše** > **Clustery HDInsight**.   
2. Z okna clusteru Spark klikněte na **Řídicí panel clusteru** a poté na **Poznámkový blok Jupyter**. Po vyzvání zadejte přihlašovací údaje správce clusteru.

   > [!NOTE]
   > Může také otevřít poznámkový blok Jupyter pro váš cluster tak, že otevřete následující adresu URL v prohlížeči. Nahraďte **CLUSTERNAME** názvem clusteru:
   >
   > `https://CLUSTERNAME.azurehdinsight.net/jupyter`
   >
   >
3. Vytvořte nový poznámkový blok. Klikněte na tlačítko **Nový** a pak klikněte na tlačítko **PySpark**.

    ![Vytvoření nového poznámkového bloku Jupyter](./media/hdinsight-apache-spark-jupyter-spark-sql/hdispark.note.jupyter.createnotebook.png "Create a new Jupyter notebook")
4. Nový poznámkový blok se vytvoří a otevře s názvem Untitled.pynb. Klikněte na název poznámkového bloku v horní části a zadejte popisný název.

    ![Zadání názvu poznámkového bloku](./media/hdinsight-apache-spark-jupyter-spark-sql/hdispark.note.jupyter.notebook.name.png "Provide a name for the notebook")
5. Vzhledem k tomu, že jste poznámkový blok vytvořili pomocí jádra PySpark, není nutné explicitně tvořit kontexty. Kontexty Spark a Hive se automaticky vytvoří za vás při spuštění první buňky kódu. Můžete začít importem typů nezbytných pro tento scénář. Chcete-li tak učinit, vložte následující fragment kódu do buňky a stiskněte klávesu **SHIFT + ENTER**.

        from pyspark.sql.types import *

    Při každém spuštění úlohy v Jupyter se název okna webového prohlížeče zobrazí jako **(Zaneprázdněn)** společně s názvem poznámkového bloku. Zobrazí se také plný kroužek vedle textu **PySpark** v pravém horním rohu. Po dokončení úlohy se změní na prázdný kruh.

     ![Stav úlohy poznámkového bloku Jupyter](./media/hdinsight-apache-spark-jupyter-spark-sql/hdispark.jupyter.job.status.png "Status of a Jupyter notebook job")
6. Načtěte vzorová data do dočasné tabulky. Když vytvoříte cluster Spark v HDInsight, ukázkový datový soubor **hvac.csv** se zkopíruje do přidruženého účtu úložiště na adrese **\HdiSamples\HdiSamples\SensorSampleData\hvac**.

    Do prázdné buňky vložte následující příklad kódu a stiskněte klávesu **SHIFT + ENTER**. Tento ukázkový kód registruje data do dočasné tabulky nazývané **TVK**.

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
7. Vzhledem k tomu, že používáte jádro PySpark, můžete nyní přímo spustit dotaz SQL na dočasnou tabulku **TVK**, kterou jste právě vytvořili pomocí `%%sql` magic. Další informace o `%%sql` magic a také dalších magic, které jsou k dispozici s jádrem PySpark, naleznete v části [Jádra dostupná v poznámkových blocích Jupyter s clustery Spark HDInsight](hdinsight-apache-spark-jupyter-notebook-kernels.md#why-should-i-use-the-pyspark-or-spark-kernels).

        %%sql
        SELECT buildingID, (targettemp - actualtemp) AS temp_diff, date FROM hvac WHERE date = \"6/1/13\"
8. Po úspěšném dokončení úlohy se ve výchozím nastavení zobrazí následující tabulkový výstup.

     ![Tabulkový výstup výsledků dotazu](./media/hdinsight-apache-spark-jupyter-spark-sql/tabular.output.png "Table output of query result")

    Výsledky můžete také zobrazit v dalších vizualizacích. Například plošný graf pro stejný výstup bude vypadat následovně.

    ![Plošný graf výsledku dotazu](./media/hdinsight-apache-spark-jupyter-spark-sql/area.output.png "Area graph of query result")
9. Po dokončení spuštění aplikace byste měli poznámkový blok vypnout a uvolnit tak prostředky. To provedete kliknutím na položku **Zavřít a zastavit** z nabídky **Soubor** v poznámkovém bloku. Dojde k vypnutí a zavření poznámkového bloku.

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



<!--HONumber=Nov16_HO2-->


