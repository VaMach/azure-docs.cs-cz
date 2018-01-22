---
title: "Transformace dat pomocí Sparku v Azure Data Factory | Dokumentace Microsoftu"
description: "Tento kurz obsahuje podrobné pokyny pro transformaci dat pomocí aktivity Sparku ve službě Azure Data Factory."
services: data-factory
documentationcenter: 
author: shengcmsft
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 01/10/2018
ms.author: shengc
ms.openlocfilehash: 8bd9382ed5a855368533c6bf2305682861c109c0
ms.sourcegitcommit: 384d2ec82214e8af0fc4891f9f840fb7cf89ef59
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/16/2018
---
# <a name="transform-data-in-the-cloud-by-using-spark-activity-in-azure-data-factory"></a>Transformace dat v cloudu pomocí aktivity Sparku ve službě Azure Data Factory
V tomto kurzu pomocí webu Azure Portal vytvoříte kanál Data Factory, který transformuje data pomocí aktivity Sparku a propojené služby HDInsight na vyžádání. V tomto kurzu provedete následující kroky:

> [!div class="checklist"]
> * Vytvoření datové továrny 
> * Vytvoření kanálu s aktivitou Sparku
> * Aktivace spuštění kanálu
> * Monitorování spuštění kanálu

> [!NOTE]
> Tento článek se týká verze 2 služby Data Factory, která je aktuálně ve verzi Preview. Pokud používáte verzi 1 služby Data Factory, který je všeobecně dostupná (GA), prostudujte si [dokumentaci služby Data Factory verze 1](v1/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).

Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/) před tím, než začnete.

## <a name="prerequisites"></a>Požadavky
* **Účet služby Azure Storage**. Vytvoříte skript Pythonu a vstupní soubor a nahrajete je do úložiště Azure. V tomto účtu úložiště se ukládá výstup z programu Sparku. Cluster Spark na vyžádání používá stejný účet úložiště jako primární úložiště.  
* **Azure PowerShell**. Postupujte podle pokynů v tématu [Jak nainstalovat a nakonfigurovat Azure PowerShell](/powershell/azure/install-azurerm-ps).


### <a name="upload-python-script-to-your-blob-storage-account"></a>Uložení skriptu Pythonu do účtu služby Blob Storage
1. Vytvořte soubor Pythonu s názvem **WordCount_Spark.py** s následujícím obsahem: 

    ```python
    import sys
    from operator import add
    
    from pyspark.sql import SparkSession
    
    def main():
        spark = SparkSession\
            .builder\
            .appName("PythonWordCount")\
            .getOrCreate()
            
        lines = spark.read.text("wasbs://adftutorial@<storageaccountname>.blob.core.windows.net/spark/inputfiles/minecraftstory.txt").rdd.map(lambda r: r[0])
        counts = lines.flatMap(lambda x: x.split(' ')) \
            .map(lambda x: (x, 1)) \
            .reduceByKey(add)
        counts.saveAsTextFile("wasbs://adftutorial@<storageaccountname>.blob.core.windows.net/spark/outputfiles/wordcount")
        
        spark.stop()
    
    if __name__ == "__main__":
        main()
    ```
2. Nahraďte **&lt;storageAccountName&gt;** názvem vašeho účtu služby Azure Storage. Pak soubor uložte. 
3. Ve službě Azure Blob Storage, vytvořte kontejner **adftutorial**, pokud ještě neexistuje. 
4. Vytvořte složku s názvem **spark**.
5. Ve složce **spark** vytvořte podsložku s názvem **script**. 
6. Do podsložky **script** uložte soubor **WordCount_Spark.py**. 


### <a name="upload-the-input-file"></a>Nahrání vstupního souboru
1. Vytvořte soubor s názvem **minecraftstory.txt** a nějakým textem. Program Sparku spočítá slova v tomto textu. 
2. Ve složce `spark` vytvořte podsložku s názvem `inputfiles`. 
3. Do podsložky `inputfiles` uložte soubor `minecraftstory.txt`. 

## <a name="create-a-data-factory"></a>Vytvoření datové továrny

1. V nabídce vlevo klikněte na **Nový**, klikněte na **Data + analýzy** a pak na **Data Factory**. 
   
   ![Nový -> Objekt pro vytváření dat](./media/tutorial-transform-data-spark-portal/new-azure-data-factory-menu.png)
2. Na stránce **Nová datová továrna** jako **název** zadejte **ADFTutorialDataFactory**. 
      
     ![Stránka Nová datová továrna](./media/tutorial-transform-data-spark-portal/new-azure-data-factory.png)
 
   Název objektu pro vytváření dat Azure musí být **globálně jedinečný**. Pokud se zobrazí následující chyba pole názvu, změňte název datové továrny (například na vaše_jméno_ADFTutorialDataFactory). Pravidla pojmenování artefaktů služby Data Factory najdete v článku [Data Factory – pravidla pojmenování](naming-rules.md).
  
     ![Název není dostupný – chyba](./media/tutorial-transform-data-spark-portal/name-not-available-error.png)
3. Vyberte své **předplatné** Azure, ve kterém chcete vytvořit datovou továrnu. 
4. Pro **Skupinu prostředků** proveďte jeden z následujících kroků:
     
      - Vyberte **Použít existující** a z rozevíracího seznamu vyberte existující skupinu prostředků. 
      - Vyberte **Vytvořit novou** a zadejte název skupiny prostředků.   
         
      Některé kroky v tomto rychlém startu vychází z předpokladu, že pro skupinu prostředků použijete název **ADFTutorialResourceGroup**. Informace o skupinách prostředků najdete v článku [Použití skupin prostředků ke správě prostředků Azure](../azure-resource-manager/resource-group-overview.md).  
4. Jako **verzi** vyberte **V2 (Preview)**.
5. Vyberte **umístění** pro objekt pro vytváření dat. Data Factory V2 v současné době umožňuje vytváření datových továren jenom v oblastech Východní USA, Východní USA 2 a Západní Evropa. Úložiště dat (Azure Storage, Azure SQL Database atd.) a výpočetní prostředí (HDInsight atd.) používané datovou továrnou mohou být v jiných oblastech.
6. Zaškrtněte **Připnout na řídicí panel**.     
7. Klikněte na možnost **Vytvořit**.
8. Na řídicím panelu vidíte následující dlaždice se statusem: **Nasazování datové továrny**. 

    ![nasazování dlaždice datové továrny](media//tutorial-transform-data-spark-portal/deploying-data-factory.png)
9. Po vytvoření se zobrazí stránka **Datová továrna**, jak je znázorněno na obrázku.
   
    ![Domovská stránka objektu pro vytváření dat](./media/tutorial-transform-data-spark-portal/data-factory-home-page.png)
10. Kliknutím na dlaždici **Vytvořit a monitorovat** otevřete na samostatné kartě aplikaci uživatelského rozhraní služby Data Factory.

## <a name="create-linked-services"></a>Vytvoření propojených služeb
V této části vytvoříte dvě propojené služby: 
    
- **Propojená služba Azure Storage**, která propojí účet služby Azure Storage s datovou továrnou. Toto úložiště používá cluster HDInsight na vyžádání. Obsahuje také skript Sparku, který se má spustit. 
- **Propojená služba HDInsight na vyžádání**. Azure Data Factory automaticky vytvoří cluster HDInsight, spustí program Sparku a pak odstraní cluster HDInsight, jakmile bude nečinný po předkonfigurovanou dobu. 

### <a name="create-an-azure-storage-linked-service"></a>Vytvoření propojené služby Azure Storage

1. Na stránce **Začínáme** přepněte na levém panelu na kartu **Upravit**, jak je znázorněno na následujícím obrázku: 

    ![Dlaždice Vytvořit kanál](./media/tutorial-transform-data-spark-portal/get-started-page.png)

2. Klikněte na **Připojení** v dolní části okna a pak klikněte na **+ Nové**. 

    ![Tlačítko Nové připojení](./media/tutorial-transform-data-spark-portal/new-connection.png)
3. V okně **Nová propojená služba** vyberte **Azure Blob Storage** a klikněte na **Pokračovat**. 

    ![Výběr služby Azure Blob Storage](./media/tutorial-transform-data-spark-portal/select-azure-storage.png)
4. Vyberte **název svého účtu služby Azure Storage** a klikněte na **Uložit**. 

    ![Zadání účtu služby Blob Storage](./media/tutorial-transform-data-spark-portal/new-azure-storage-linked-service.png)


### <a name="create-an-on-demand-hdinsight-linked-service"></a>Vytvoření propojené služby HDInsight na vyžádání

1. Znovu klikněte na tlačítko **+ Nová** a vytvořte další propojenou službu. 
2. V okně **Nová propojená služba** vyberte **Azure HDInsight** a klikněte na **Pokračovat**. 

    ![Výběr služby Azure HDInsight](./media/tutorial-transform-data-spark-portal/select-azure-hdinsight.png)
2. V okně **Nová propojená služba** proveďte následující kroky: 

    1. Jako **Název** zadejte **AzureHDInsightLinkedService**.
    2. Ověřte, že jako **Typ** je vybraná možnost **HDInsight na vyžádání**.
    3. Jako **Propojená služba Azure Storage** vyberte **AzureStorage1**. Tuto propojenou službu jste vytvořili dříve. Pokud jste použili jiný název, zadejte do tohoto pole správný název. 
    4. Jako **Typ clusteru** vyberte **spark**.
    5. Zadejte **ID instančního objektu** s oprávněním k vytvoření clusteru HDInsight. Tento instanční objekt musí být členem role přispěvatele předplatného nebo skupiny prostředků, ve které se cluster vytvoří. Podrobnosti najdete v tématu [Vytvoření aplikace Azure Active Directory a instančního objektu](../azure-resource-manager/resource-group-create-service-principal-portal.md).
    6. Zadejte **Klíč instančního objektu**. 
    7. Jako **Skupina prostředků** vyberte stejnou skupinu prostředků, kterou jste použili při vytváření datové továrny. Cluster Spark se vytvoří v této skupině prostředků. 
    8. Rozbalte **Typ operačního systému**.
    9. Zadejte **jméno** pro **uživatele** clusteru. 
    10. Zadejte **heslo** pro tohoto uživatele. 
    11. Klikněte na **Uložit**. 

        ![Nastavení propojené služby HDInsight](./media/tutorial-transform-data-spark-portal/azure-hdinsight-linked-service-settings.png)

> [!NOTE]
> Pro Azure HDInsight platí omezení celkového počtu jader, které můžete v jednotlivých podporovaných oblastech použít. V případě propojené služby HDInsight na vyžádání se cluster HDInsight vytvoří ve stejném umístění jako služba Azure Storage, kterou používá jako primární úložiště. Ujistěte se, že máte dostatečné kvóty pro jádra, aby bylo možné cluster úspěšně vytvořit. Další informace najdete v tématu [Nastavení clusterů v HDInsight se systémem Hadoop, Spark, Kafka a dalšími](../hdinsight/hdinsight-hadoop-provision-linux-clusters.md). 

## <a name="create-a-pipeline"></a>Vytvoření kanálu

2. Klikněte na tlačítko + (plus) a v nabídce klikněte na **Kanál**.

    ![Nabídka Nový kanál](./media/tutorial-transform-data-spark-portal/new-pipeline-menu.png)
3. Na panelu nástrojů **Aktivity** rozbalte **HDInsight** a přetáhněte aktivitu **Spark** z panelu nástrojů **Aktivity** na plochu návrháře kanálu. 

    ![Přetažení aktivity Spark](./media/tutorial-transform-data-spark-portal/drag-drop-spark-activity.png)
4. Ve vlastnostech v dolní části okna aktivity **Spark** proveďte následující kroky: 

    1. Přepněte na kartu **Cluster HDInsight**.
    2. Vyberte službu **AzureHDInsightLinkedService**, kterou jste vytvořili v předchozím kroku. 
        
    ![Zadání propojené služby HDInsight](./media/tutorial-transform-data-spark-portal/select-hdinsight-linked-service.png)
5. Přepněte na kartu **Skripty/Jar** a proveďte následující kroky: 

    1. Jako **Propojená služba úloh** vyberte **AzureStorage1**.
    2. Klikněte na **Procházet úložiště**. 
    3. Přejděte do složky **adftutorial/spark/script**, vyberte soubor **WordCount_Spark.py** a klikněte na **Dokončit**.      

    ![Zadání skriptu Sparku](./media/tutorial-transform-data-spark-portal/specify-spark-script.png)
6. Pokud chcete kanál ověřit, klikněte na tlačítko **Ověřit** na panelu nástrojů. Kliknutím na tlačítko s **šipkami doprava** (>>) zavřete okno ověřování. 
    
    ![Tlačítko Ověřit](./media/tutorial-transform-data-spark-portal/validate-button.png)
7. Klikněte na **Publikovat**. Uživatelské rozhraní služby Data Factory publikuje entity (propojené služby a kanál) do služby Azure Data Factory. 

## <a name="trigger-a-pipeline-run"></a>Aktivace spuštění kanálu
Klikněte na **Aktivační událost** na panelu nástrojů a pak klikněte na **Aktivovat**. 

![Aktivovat](./media/tutorial-transform-data-spark-portal/trigger-now-menu.png)

## <a name="monitor-the-pipeline-run"></a>Monitorování spuštění kanálu

1. Přepněte na kartu **Monitorování**. Ověřte, že se zobrazuje spuštění kanálu. Vytvoření clusteru Spark trvá přibližně 20 minut. 

    ![Monitorování spuštění kanálu](./media/tutorial-transform-data-spark-portal/monitor-tab.png)
2. Pravidelně klikejte na **Aktualizovat** a kontrolujte stav spuštění kanálu. 

    ![Stav spuštění kanálu](./media/tutorial-transform-data-spark-portal/pipeline-run-succeeded.png)
1. Pokud chcete zobrazit spuštění aktivit související se spuštěním kanálu, klikněte na akci **Zobrazit spuštění aktivit** ve sloupci Akce. Zpět na zobrazení spuštění kanálu můžete přepnout kliknutím na odkaz **Kanály** v horní části.

    ![Zobrazení Spuštění aktivit](./media/tutorial-transform-data-spark-portal/activity-runs.png)

## <a name="verify-the-output"></a>Ověření výstupu
Ověřte, že se ve složce spark/otuputfiles/wordcount kontejneru adftutorial vytvořil výstupní soubor. 

![Ověření výstupu](./media/tutorial-transform-data-spark-portal/verity-output.png)

Tento soubor by měl obsahovat všechna slova ze vstupního textového souboru a počet výskytů těchto slov v souboru. Příklad: 

```
(u'This', 1)
(u'a', 1)
(u'is', 1)
(u'test', 1)
(u'file', 1)
```

## <a name="next-steps"></a>Další kroky
Kanál v této ukázce transformuje data pomocí aktivity Sparku a propojené služby HDInsight na vyžádáni. Naučili jste se tyto postupy: 

> [!div class="checklist"]
> * Vytvoření datové továrny 
> * Vytvoření kanálu s aktivitou Sparku
> * Aktivace spuštění kanálu
> * Monitorování spuštění kanálu

Pokud chcete zjistit, jak transformovat data spuštěním skriptu Hivu v clusteru Azure HDInsight ve virtuální síti, přejděte k následujícímu kurzu: 

> [!div class="nextstepaction"]
> [Kurz: Transformace dat pomocí Hivu ve službě Azure Virtual Network](tutorial-transform-data-hive-virtual-network-portal.md)





