---
title: "Vytvoření clusteru Apache Spark ve službě Azure HDInsight | Dokumentace Microsoftu"
description: "Rychlý start k HDInsight Spark týkající se vytvoření clusteru Apache Spark ve službě HDInsight."
keywords: spark quickstart, interactive spark, interactive query, hdinsight spark, azure spark
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 91f41e6a-d463-4eb4-83ef-7bbb1f4556cc
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/21/2017
ms.author: nitinme
ms.translationtype: HT
ms.sourcegitcommit: 22aa82e5cbce5b00f733f72209318c901079b665
ms.openlocfilehash: ad4330a1fc7f8de154d9aaa8df3acc2ab59b9dc1
ms.contentlocale: cs-cz
ms.lasthandoff: 07/24/2017

---
# <a name="create-an-apache-spark-cluster-in-azure-hdinsight"></a>Vytvoření clusteru Apache Spark ve službě Azure HDInsight

V tomto článku se dozvíte, jak vytvořit cluster Apache Spark ve službě Azure HDInsight. Informace o Apache Spark ve službě HDInsight najdete v tématu [Přehled: Apache Spark v Azure HDInsight](hdinsight-apache-spark-overview.md).

   ![Diagram rychlého startu popisující postup vytvoření clusteru Apache Spark ve službě Azure HDInsight](./media/hdinsight-apache-spark-jupyter-spark-sql/hdinsight-spark-quickstart-interactive-spark-query-flow.png "Rychlý start k použití Apache Spark ve službě HDInsight. Popsané postupy: vytvoření clusteru, spuštění interaktivního dotazu Spark")

## <a name="prerequisites"></a>Požadavky

* **Předplatné Azure**. Než začnete tento kurz, musíte mít předplatné Azure. Přečtěte si téma [Bezplatné vytvoření účtu Microsoft Azure ještě dnes](https://azure.microsoft.com/free).

## <a name="create-hdinsight-spark-cluster"></a>Vytvoření clusteru HDInsight Spark

V této části vytvoříte cluster HDInsight Spark pomocí [šablony Azure Resource Manageru](https://azure.microsoft.com/resources/templates/101-hdinsight-spark-linux/). Ostatní metody tvorby clusteru najdete v části [Tvorba clusterů HDInsight](hdinsight-hadoop-provision-linux-clusters.md).

1. Kliknutím na následující obrázek otevřete šablonu na portálu Azure Portal.         

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-hdinsight-spark-linux%2Fazuredeploy.json" target="_blank"><img src="./media/hdinsight-apache-spark-jupyter-spark-sql/deploy-to-azure.png" alt="Deploy to Azure"></a>

2. Zadejte následující hodnoty:

    ![Vytvoření clusteru HDInsight Spark pomocí šablony Azure Resource Manageru](./media/hdinsight-apache-spark-jupyter-spark-sql/create-spark-cluster-in-hdinsight-using-azure-resource-manager-template.png "Vytvoření clusteru Spark ve službě HDInsight pomocí šablony Azure Resource Manageru")

    * **Předplatné:** Vyberte předplatné Azure pro tento cluster.
    * **Skupina prostředků:** Vytvořte skupinu prostředků nebo vyberte stávající. Skupina prostředků slouží ke správě prostředků Azure pro vaše projekty.
    * **Umístění:** Vyberte umístění pro skupinu prostředků. Šablona toto umístění používá k vytvoření clusteru i jako výchozí úložiště clusteru.
    * **Název clusteru:** Zadejte název pro cluster HDInsight, který chcete vytvořit.
    * **Verze Sparku:** Jako verzi, kterou chcete nainstalovat v clusteru, vyberte **2.0**.
    * **Přihlašovací jméno a heslo clusteru**: výchozí přihlašovací jméno je admin.
    * **Uživatelské jméno a heslo SSH**.

   Zapište si tyto hodnoty.  Budete je potřebovat později v kurzu.

3. Vyberte **Souhlasím s podmínkami a ujednáními uvedenými nahoře**, vyberte **Připnout na řídicí panel** a potom klikněte na **Koupit**. Zobrazí se nová dlaždice s názvem Odeslání nasazení pro šablonu nasazení. Vytvoření clusteru trvá přibližně 20 minut.

Pokud narazíte na problém s vytvářením clusterů HDInsight, může to být způsobeno tím, že k tomu nemáte správná oprávnění. Další informace najdete v tématu popisujícím [požadavky na řízení přístupu](hdinsight-administer-use-portal-linux.md#create-clusters).

> [!NOTE]
> Tento článek vytvoří cluster Spark, který využívá [Azure Storage Blob jako úložiště clusteru](hdinsight-hadoop-use-blob-storage.md). Můžete také vytvořit cluster Spark, který jako výchozí úložiště používá [Azure Data Lake Store](hdinsight-hadoop-use-data-lake-store.md). Pokyny naleznete v tématu [Vytvoření clusteru HDInsight pomocí Data Lake Store](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md).
>
>

## <a name="run-a-hive-query-using-spark-sql"></a>Spuštění dotazu Hive pomocí Spark SQL

Pokud používáte poznámkový blok Jupyter nakonfigurovaný pro váš cluster HDInsight Spark, získáte přednastavený kontext `sqlContext`, který můžete použít ke spouštění dotazů Hive pomocí Spark SQL. V této části zjistíte, jak spustit poznámkový blok Jupyter a potom spustit základní dotaz Hive.

1. Otevřete web [Azure Portal](https://portal.azure.com/).

2. Pokud jste se rozhodli připnout cluster na řídicí panel, kliknutím na dlaždici clusteru na řídicím panelu spusťte okno clusteru.

    Pokud jste cluster na řídicí panel nepřipnuli, v levém podokně klikněte na **Clustery HDInsight** a pak klikněte na cluster, který jste vytvořili.

3. V části **Rychlé odkazy** klikněte na **Řídicí panely clusteru** a potom klikněte na **Poznámkový blok Jupyter**. Po vyzvání zadejte přihlašovací údaje správce clusteru.

   ![Otevření poznámkového bloku Jupyter pro spuštění interaktivního dotazu Spark SQL](./media/hdinsight-apache-spark-jupyter-spark-sql/hdinsight-spark-open-jupyter-interactive-spark-sql-query.png "Otevření poznámkového bloku Jupyter pro spuštění interaktivního dotazu Spark SQL")

   > [!NOTE]
   > K poznámkovému bloku Jupyter pro váš cluster se dostanete také otevřením následující adresy URL v prohlížeči. Nahraďte **CLUSTERNAME** názvem clusteru:
   >
   > `https://CLUSTERNAME.azurehdinsight.net/jupyter`
   >
   >
3. Vytvořte poznámkový blok. Klikněte na tlačítko **Nový** a pak klikněte na tlačítko **PySpark**.

   ![Vytvoření poznámkového bloku Jupyter pro spuštění interaktivního dotazu Spark SQL](./media/hdinsight-apache-spark-jupyter-spark-sql/hdinsight-spark-create-jupyter-interactive-Spark-SQL-query.png "Vytvoření poznámkového bloku Jupyter pro spuštění interaktivního dotazu Spark SQL")

   Nový poznámkový blok se vytvoří a otevře s názvem Bez názvu (Bez názvu.pynb).

4. Pokud chcete, klikněte na název poznámkového bloku v horní části a zadejte popisný název.

    ![Zadání názvu, ze kterého má poznámkový blok Jupyter spustit interaktivní dotaz Spark](./media/hdinsight-apache-spark-jupyter-spark-sql/hdinsight-spark-jupyter-notebook-name.png "Zadání názvu, ze kterého má poznámkový blok Jupyter spustit interaktivní dotaz Spark")

5.  Do prázdné buňky vložte následující kód a stisknutím **SHIFT + ENTER** kód spusťte. V následujícím kódu příkaz `%%sql` (označovaný jako magický příkaz jazyka SQL) říká poznámkovému bloku Jupyter, aby ke spuštění dotazu Hive použil přednastavený kontext `sqlContext`. Dotaz načte prvních 10 řádků z tabulky Hive (**hivesampletable**), která je ve výchozím nastavení k dispozici na všech clusterech HDInsight.

        %%sql
        SELECT * FROM hivesampletable LIMIT 10

    ![Dotaz Hive v HDInsight Spark](./media/hdinsight-apache-spark-jupyter-spark-sql/hdinsight-spark-get-started-hive-query.png "Dotaz Hive v HDInsight Spark")

    Další informace o magickém příkazu `%%sql` a přednastavených kontextech najdete v tématu [Dostupná jádra Jupyter pro cluster HDInsight](hdinsight-apache-spark-jupyter-notebook-kernels.md).

    > [!NOTE]
    > Při každém spuštění dotazu v Jupyter se v názvu okna webového prohlížeče zobrazí stav **(Busy)** (Zaneprázdněn) společně s názvem poznámkového bloku. Zobrazí se také plný kroužek vedle textu **PySpark** v pravém horním rohu. Po dokončení úlohy se změní na prázdný kruh.
    >
    >
    
6. Obrazovka by se měla aktualizovat a zobrazit výstup dotazu.

    ![Výstup dotazu Hive v HDInsight Spark](./media/hdinsight-apache-spark-jupyter-spark-sql/hdinsight-spark-get-started-hive-query-output.png "Výstup dotazu Hive v HDInsight Spark")

7. Po dokončení spuštění aplikace můžete poznámkový blok vypnout a uvolnit tak prostředky clusteru. To provedete kliknutím na položku **Zavřít a zastavit** z nabídky **Soubor** v poznámkovém bloku.

8. Pokud se chystáte další kroky dokončit později, nezapomeňte odstranit cluster HDInsight, který jste vytvořili v rámci tohoto článku. 

    [!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="next-step"></a>Další krok 

V tomto článku jste zjistili, jak vytvořit cluster HDInsight Spark a spustit základní dotaz Spark SQL. Přejděte k dalšímu článku a zjistěte, jak pomocí clusteru HDInsight Spark spouštět interaktivní dotazy na ukázková data.

> [!div class="nextstepaction"]
>[Spouštění interaktivních dotazů v clusteru HDInsight Spark.](hdinsight-apache-spark-load-data-run-query.md)




