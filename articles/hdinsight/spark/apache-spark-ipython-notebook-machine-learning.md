---
title: "Sestavení aplikace strojového učení Apache Spark v Azure HDInsight | Microsoft Docs"
description: "Podrobné pokyny o tom, jak sestavit aplikaci strojové učení Apache Spark HDInsight Spark clusterů pomocí poznámkového bloku Jupyter"
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: f584ca5e-abee-4b7c-ae58-2e45dfc56bf4
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/28/2017
ms.author: nitinme
ms.openlocfilehash: 8d4f29cab5e548c6ba4780e08c7c2a5b0c9fcefe
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/03/2017
---
# <a name="build-apache-spark-machine-learning-applications-on-azure-hdinsight"></a>Sestavení aplikace strojového učení Apache Spark v Azure HDInsight

Naučte se vytvářet Apache Spark strojového učení aplikace pomocí Spark cluster v HDInsight. Tento článek ukazuje, jak pomocí poznámkového bloku Jupyter dostupné s clusterem pro sestavení a otestování této aplikace. Aplikace používá HVAC.csv ukázková data, která je k dispozici na všech clusterech ve výchozím nastavení.

**Požadavky:**

Musíte mít následující:

* Cluster Apache Spark v HDInsight. Pokyny najdete v tématu [clusterů vytvořit Apache Spark v Azure HDInsight](apache-spark-jupyter-spark-sql.md). 

## <a name="data"></a>Pochopit datové sady
Před můžeme začít vytvářet aplikace, dejte nám porozumět struktury dat, pro kterou jsme sestavení aplikace a na druhu analýzy, kterou uděláme na data. 

V tomto článku používáme vzorku **HVAC.csv** datový soubor, který je k dispozici v účtu Azure Storage, který je spojen s clusteru HDInsight. V rámci účtu úložiště je soubor na **\HdiSamples\HdiSamples\SensorSampleData\hvac**. Stáhněte a otevřete soubor CSV se získat data snímku.  

![Snímek dat použít Spark machine learning třeba](./media/apache-spark-ipython-notebook-machine-learning/spark-machine-learning-understand-data.png "snímek dat použít například Spark machine learning")

Data se zobrazuje teploty cíl a skutečný teploty vytváření, který má nainstalované systémy VZT. Předpokládejme **systému** sloupec představuje ID systému a **SystemAge** sloupec představuje počet let už byl systém TVK zavedené v budově.

Tato data použít k předpovědi, zda v budově bude hotter nebo colder na základě na cílové teploty, daného ID systému a stáří systému.

## <a name="app"></a>Zápis aplikací Spark machine learning pomocí Spark MLlib
V této aplikaci jsme provádět klasifikaci dokumentů pomocí Spark ML kanálu. V kanálu jsme dokumentu rozdělení do slova, převést slova číselné funkce vector a nakonec sestavení předpovědi model pomocí funkce vektory a popisky. Proveďte následující kroky k vytvoření aplikace.

1. Z [Portálu Azure](https://portal.azure.com/) z úvodního panelu klikněte na dlaždici pro váš cluster Spark (pokud je připnutý na úvodní panel). Můžete také přejít na cluster pod položkou **Procházet vše** > **Clustery HDInsight**.   
2. Z okna clusteru Spark klikněte na **Řídicí panel clusteru** a poté na **Poznámkový blok Jupyter**. Po vyzvání zadejte přihlašovací údaje správce clusteru.
   
   > [!NOTE]
   > Může také otevřít poznámkový blok Jupyter pro váš cluster tak, že otevřete následující adresu URL v prohlížeči. Nahraďte **CLUSTERNAME** názvem clusteru:
   > 
   > `https://CLUSTERNAME.azurehdinsight.net/jupyter`
   > 
   > 
3. Vytvořte nový poznámkový blok. Klikněte na tlačítko **Nový** a pak klikněte na tlačítko **PySpark**.
   
    ![Vytvoření poznámkového bloku Jupyter Spark machine learning třeba](./media/apache-spark-ipython-notebook-machine-learning/spark-machine-learning-create-notebook.png "vytvoření poznámkového bloku Jupyter například Spark machine learning")
4. Nový poznámkový blok se vytvoří a otevře s názvem Untitled.pynb. Klikněte na název poznámkového bloku v horní části a zadejte popisný název.
   
    ![Zadejte název poznámkového bloku Spark machine learning třeba](./media/apache-spark-ipython-notebook-machine-learning/spark-machine-learning-notebook-name.png "zadejte název poznámkového bloku například Spark machine learning")
5. Vzhledem k tomu, že jste poznámkový blok vytvořili pomocí jádra PySpark, není nutné explicitně tvořit kontexty. Kontexty Spark a Hive se automaticky vytvoří za vás při spuštění první buňky kódu. Můžete začít importem typů, které jsou požadovány pro tento scénář. Vložte následující fragment kódu do prázdné buňky a stiskněte klávesu **SHIFT + ENTER**. 
   
        from pyspark.ml import Pipeline
        from pyspark.ml.classification import LogisticRegression
        from pyspark.ml.feature import HashingTF, Tokenizer
        from pyspark.sql import Row
   
        import os
        import sys
        from pyspark.sql.types import *
   
        from pyspark.mllib.classification import LogisticRegressionWithSGD
        from pyspark.mllib.regression import LabeledPoint
        from numpy import array
6. Nyní musíte načtení dat (hvac.csv), jeho analyzovat a použít ho k natrénování modelu. V takovém případě definujete funkci, která zkontroluje, zda je větší než je cíl teplota skutečné teploty budovy. Pokud skutečný teploty je větší, vytvářet je aktivní, označený jako hodnota **1.0**. Pokud skutečný teploty je menší, vytvářet je studenou, označený jako hodnota **0,0**. 
   
    Vložte následující fragment kódu do prázdné buňky a stiskněte klávesu **SHIFT + ENTER**.

        # List the structure of data for better understanding. Because the data will be
        # loaded as an array, this structure makes it easy to understand what each element
        # in the array corresponds to

        # 0 Date
        # 1 Time
        # 2 TargetTemp
        # 3 ActualTemp
        # 4 System
        # 5 SystemAge
        # 6 BuildingID

        LabeledDocument = Row("BuildingID", "SystemInfo", "label")

        # Define a function that parses the raw CSV file and returns an object of type LabeledDocument

        def parseDocument(line):
            values = [str(x) for x in line.split(',')]
            if (values[3] > values[2]):
                hot = 1.0
            else:
                hot = 0.0        

            textValue = str(values[4]) + " " + str(values[5])

            return LabeledDocument((values[6]), textValue, hot)

        # Load the raw HVAC.csv file, parse it using the function
        data = sc.textFile("wasb:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")

        documents = data.filter(lambda s: "Date" not in s).map(parseDocument)
        training = documents.toDF()


1. Konfigurace Spark machine learning kanálu, který se skládá ze tří fází: tokenizátor, hashingTF a lr. Další informace o co je kanál, a jak to funguje najdete <a href="http://spark.apache.org/docs/latest/ml-guide.html#how-it-works" target="_blank">Spark machine learning kanálu</a>.
   
    Vložte následující fragment kódu do prázdné buňky a stiskněte klávesu **SHIFT + ENTER**.
   
        tokenizer = Tokenizer(inputCol="SystemInfo", outputCol="words")
        hashingTF = HashingTF(inputCol=tokenizer.getOutputCol(), outputCol="features")
        lr = LogisticRegression(maxIter=10, regParam=0.01)
        pipeline = Pipeline(stages=[tokenizer, hashingTF, lr])
2. Přizpůsobení kanálu v dokumentu školení. Vložte následující fragment kódu do prázdné buňky a stiskněte klávesu **SHIFT + ENTER**.
   
        model = pipeline.fit(training)
3. Ověřte školení dokumentu, který má kontrolní bod průběh s aplikací. Vložte následující fragment kódu do prázdné buňky a stiskněte klávesu **SHIFT + ENTER**.
   
        training.show()
   
    To získat výstup podobný následujícímu:
   
        +----------+----------+-----+
        |BuildingID|SystemInfo|label|
        +----------+----------+-----+
        |         4|     13 20|  0.0|
        |        17|      3 20|  0.0|
        |        18|     17 20|  1.0|
        |        15|      2 23|  0.0|
        |         3|      16 9|  1.0|
        |         4|     13 28|  0.0|
        |         2|     12 24|  0.0|
        |        16|     20 26|  1.0|
        |         9|      16 9|  1.0|
        |        12|       6 5|  0.0|
        |        15|     10 17|  1.0|
        |         7|      2 11|  0.0|
        |        15|      14 2|  1.0|
        |         6|       3 2|  0.0|
        |        20|     19 22|  0.0|
        |         8|     19 11|  0.0|
        |         6|      15 7|  0.0|
        |        13|      12 5|  0.0|
        |         4|      8 22|  0.0|
        |         7|      17 5|  0.0|
        +----------+----------+-----+

    Přejděte zpět a ověřte výstup proti soubor raw sdíleného svazku clusteru. Například první řádek souboru CSV má tato data:

    ![Výstupní data snímku Spark machine learning třeba](./media/apache-spark-ipython-notebook-machine-learning/spark-machine-learning-output-data.png "výstupní data snímku například Spark machine learning")

    Všimněte si, jak skutečné teploty je menší než na cílové teploty návrhy, že je vytvoření cold. Proto v školení výstupu hodnota **popisek** v prvním řádku je **0,0**, což znamená budova není aktivní.

1. Příprava ke spuštění pro cvičný model proti datové sady. To pokud chcete udělat, budeme by předat na ID systému a stáří systému (označené jako **SystemInfo** ve výstupu školení), a modelu by předpovědět, zda bude vytvářet pomocí tohoto ID systému a stáří systému hotter (označen 1.0) nebo (označen chladicí zařízení 0,0).
   
   Vložte následující fragment kódu do prázdné buňky a stiskněte klávesu **SHIFT + ENTER**.
   
       # SystemInfo here is a combination of system ID followed by system age
       Document = Row("id", "SystemInfo")
       test = sc.parallelize([(1L, "20 25"),
                     (2L, "4 15"),
                     (3L, "16 9"),
                     (4L, "9 22"),
                     (5L, "17 10"),
                     (6L, "7 22")]) \
           .map(lambda x: Document(*x)).toDF() 
2. Nakonec provádějte předpovědi v testovacích datech. Vložte následující fragment kódu do prázdné buňky a stiskněte klávesu **SHIFT + ENTER**.
   
        # Make predictions on test documents and print columns of interest
        prediction = model.transform(test)
        selected = prediction.select("SystemInfo", "prediction", "probability")
        for row in selected.collect():
            print row
3. Zobrazený výstup by měl vypadat přibližně takto:
   
       Row(SystemInfo=u'20 25', prediction=1.0, probability=DenseVector([0.4999, 0.5001]))
       Row(SystemInfo=u'4 15', prediction=0.0, probability=DenseVector([0.5016, 0.4984]))
       Row(SystemInfo=u'16 9', prediction=1.0, probability=DenseVector([0.4785, 0.5215]))
       Row(SystemInfo=u'9 22', prediction=1.0, probability=DenseVector([0.4549, 0.5451]))
       Row(SystemInfo=u'17 10', prediction=1.0, probability=DenseVector([0.4925, 0.5075]))
       Row(SystemInfo=u'7 22', prediction=0.0, probability=DenseVector([0.5015, 0.4985]))
   
   Z první řádek v předpovědi, uvidíte, že TVK systému s ID 20 a stáří systému 25 let, vytvoření bude aktivní (**předpovědi = 1.0**). První hodnota DenseVector (0.49999) odpovídá předpovědi 0,0 a druhá hodnota (0.5001) odpovídá předpovědi 1.0. Ve výstupu, i když je druhá hodnota jen málo vyšší, zobrazuje modelu **předpovědi = 1.0**.
4. Po dokončení spuštění aplikace byste měli poznámkový blok vypnout a uvolnit tak prostředky. To provedete kliknutím na položku **Zavřít a zastavit** z nabídky **Soubor** v poznámkovém bloku. Dojde k vypnutí a zavření poznámkového bloku.

## <a name="anaconda"></a>Použít Anaconda scikit-další knihovny pro Spark machine learning
Clustery Apache Spark v HDInsight zahrnují knihovnami Anaconda. To zahrnuje také **scikit-Další** knihoven pro machine learning. Knihovny také obsahuje různé datové sady, které můžete použít k vytvoření ukázkové aplikace přímo z poznámkového bloku Jupyter. Příklady použití scikit-další knihovny najdete [http://scikit-learn.org/stable/auto_examples/index.html](http://scikit-learn.org/stable/auto_examples/index.html).

## <a name="seealso"></a>Viz také
* [Přehled: Apache Spark v Azure HDInsight](apache-spark-overview.md)

### <a name="scenarios"></a>Scénáře
* [Spark s BI: Provádějte interaktivní analýzy dat pomocí Sparku v HDInsight pomocí nástrojů BI](apache-spark-use-bi-tools.md)
* [Spark s Machine Learning: Používejte Spark v HDInsight k předpovědím výsledků kontrol potravin](apache-spark-machine-learning-mllib-ipython.md)
* [Datové proudy Spark: Používejte Spark v HDInsight pro sestavení aplikací datových proudů v reálném čase](apache-spark-eventhub-streaming.md)
* [Analýza protokolu webu pomocí Sparku v HDInsight](apache-spark-custom-library-website-log-analysis.md)

### <a name="create-and-run-applications"></a>Vytvoření a spouštění aplikací
* [Vytvoření samostatné aplikace pomocí Scala](apache-spark-create-standalone-application.md)
* [Vzdálené spouštění úloh na clusteru Sparku pomocí Livy](apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a>Nástroje a rozšíření
* [Modul plug-in nástroje HDInsight pro IntelliJ IDEA pro vytvoření a odesílání aplikací Spark Scala](apache-spark-intellij-tool-plugin.md)
* [Použití modulu plug-in nástroje HDInsight pro IntelliJ IDEA pro vzdálené ladění aplikací Spark](apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [Použití poznámkových bloků Zeppelin s clusterem Sparku v HDInsight](apache-spark-zeppelin-notebook.md)
* [Jádra dostupná pro poznámkový blok Jupyter v clusteru Sparku pro HDInsight](apache-spark-jupyter-notebook-kernels.md)
* [Použití externích balíčků s poznámkovými bloky Jupyter](apache-spark-jupyter-notebook-use-external-packages.md)
* [Instalace Jupyteru do počítače a připojení ke clusteru HDInsight Spark](apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Správa prostředků
* [Správa prostředků v clusteru Apache Spark v Azure HDInsight](apache-spark-resource-manager.md)
* [Sledování a ladění úloh spuštěných v clusteru Apache Spark v HDInsight](apache-spark-job-debugging.md)

[hdinsight-versions]: hdinsight-component-versioning.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md

[hdinsight-weblogs-sample]:../hadoop/apache-hive-analyze-website-log.md
[hdinsight-sensor-data-sample]:../hadoop/apache-hive-analyze-sensor-data.md

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[azure-create-storageaccount]:../../storage/common/storage-create-storage-account.md
