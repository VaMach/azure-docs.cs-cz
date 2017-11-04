---
title: "Vyvolání Spark programy z Azure Data Factory | Microsoft Docs"
description: "Zjistěte, jak má být vyvolán Spark programy ze služby Azure data factory pomocí činnost MapReduce."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: fd98931c-cab5-4d66-97cb-4c947861255c
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/19/2017
ms.author: spelluru
robots: noindex
ms.openlocfilehash: 5220ca664d5c7584f3aada0bb707099f91d5650f
ms.sourcegitcommit: 43c3d0d61c008195a0177ec56bf0795dc103b8fa
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/01/2017
---
# <a name="invoke-spark-programs-from-azure-data-factory-pipelines"></a>Vyvolání Spark programy z kanálů služby Azure Data Factory

> [!div class="op_single_selector" title1="Transformation Activities"]
> * [Aktivita Hive](data-factory-hive-activity.md)
> * [Pig aktivity](data-factory-pig-activity.md)
> * [Činnost MapReduce](data-factory-map-reduce.md)
> * [Streamované aktivitě Hadoop](data-factory-hadoop-streaming-activity.md)
> * [Spark aktivity](data-factory-spark.md)
> * [Aktivita Provedení dávky služby Machine Learning](data-factory-azure-ml-batch-execution-activity.md)
> * [Aktivita Aktualizace prostředků služby Machine Learning](data-factory-azure-ml-update-resource-activity.md)
> * [Aktivita Uložená procedura](data-factory-stored-proc-activity.md)
> * [Aktivita U-SQL služby Data Lake Analytics](data-factory-usql-activity.md)
> * [Vlastní aktivity rozhraní .NET](data-factory-use-custom-activities.md)

> [!NOTE]
> Tento článek se týká verze 1 Azure Data Factory, který je všeobecně dostupná (GA). Pokud používáte verze 2 služby Data Factory, který je ve verzi preview, najdete v části [transformace dat pomocí spark aktivity v datové továrně verze 2](../transform-data-using-spark.md).

## <a name="introduction"></a>Úvod
Aktivita Spark je jedním z [aktivit transformace dat](data-factory-data-transformation-activities.md) podporovaných službou Azure Data Factory. Tato aktivita běží zadaný program Spark na cluster Apache Spark v Azure HDInsight.    

> [!IMPORTANT]
> - Aktivita Spark nepodporuje clustery HDInsight Spark, které používají Azure Data Lake Store jako primární úložiště.
> - Aktivita Spark podporuje pouze existující (vlastní) clustery HDInsight Spark. HDInsight propojené služby na vyžádání nepodporuje.

## <a name="walkthrough-create-a-pipeline-with-spark-activity"></a>Návod: vytvoření kanálu s aktivitou Spark
Tady jsou obvyklá kroky k vytvoření objektu pro vytváření dat kanál s aktivitou Spark.  

1. Vytvoření datové továrny
2. Vytvoření služby Azure Storage, propojené k propojení vaší úložiště Azure, který je přidružen k objektu pro vytváření dat cluster HDInsight Spark.     
2. Vytvoření služby Azure HDInsight propojené propojení cluster Apache Spark v Azure HDInsight s data factory.
3. Vytvořte datovou sadu, která odkazuje propojenou službu úložiště Azure. V současné době je třeba zadat datovou sadu výstupů pro aktivitu i v případě, že neexistuje žádný výstup se vytváří.  
4. Vytvoření kanálu s aktivitou Spark, která odkazuje propojená služba HDInsight vytvořené v #. 2. Aktivita je nakonfigurován s datovou sadou, kterou jste vytvořili v předchozím kroku jako výstupní datové. Výstupní datové sady je určuje plán (hodinový, denní, atd.). Proto je nutné zadat výstupní datovou sadu, i v případě, že aktivita nevytváří skutečně výstup.

### <a name="prerequisites"></a>Požadavky
1. Vytvořit **účet úložiště pro obecné účely Azure** podle pokynů v návodu: [vytvořit účet úložiště](../../storage/common/storage-create-storage-account.md#create-a-storage-account).  
2. Vytvořit **cluster Apache Spark v Azure HDInsight** podle pokynů v tomto kurzu: [cluster vytvořit Apache Spark v Azure HDInsight](../../hdinsight/hdinsight-apache-spark-jupyter-spark-sql.md). Přidružení účtu úložiště Azure, kterou jste vytvořili v kroku #1 s tímto clusterem.  
3. Stáhnout a revidovat souboru skriptu jazyka python **test.py** nacházející se v: [https://adftutorialfiles.blob.core.windows.net/sparktutorial/test.py](https://adftutorialfiles.blob.core.windows.net/sparktutorial/test.py).  
3.  Nahrát **test.py** k **pyFiles** složku **adfspark** kontejneru ve službě Azure Blob storage. Vytvořte kontejner, složku, pokud neexistují.

### <a name="create-data-factory"></a>Vytvoření objektu pro vytváření dat
V tomto kroku začneme vytvořením objektu pro vytváření dat.

1. Přihlaste se k portálu [Azure Portal](https://portal.azure.com/).
2. V nabídce vlevo klikněte na **NOVÝ**, klikněte na **Data + Analýza** a poté na **Objekt pro vytváření dat**.
3. V **nový objekt pro vytváření dat** okno, zadejte **SparkDF** pro název.

   > [!IMPORTANT]
   > Název objektu pro vytváření dat Azure musí být **globálně jedinečný**. Pokud se zobrazí následující chyba: **název objektu pro vytváření dat "SparkDF" není k dispozici**. Změňte název objektu pro vytváření dat (například yournameSparkDFdate a zkuste to znovu. V tématu [Objekty pro vytváření dat – pravidla pojmenování](data-factory-naming-rules.md) najdete pravidla pojmenování artefaktů služby Data Factory.   
4. Vyberte **předplatné Azure**, ve kterém chcete objekt pro vytváření dat vytvořit.
5. Vyberte existující **skupiny prostředků** nebo vytvořte skupinu prostředků Azure.
6. Vyberte **připnout na řídicí panel** možnost.  
6. V okně **Nový objekt pro vytváření dat** klikněte na **Vytvořit**.

   > [!IMPORTANT]
   > Chcete-li vytvářet instance služby Data Factory, musíte být členem role [Přispěvatel Data Factory](../../active-directory/role-based-access-built-in-roles.md#data-factory-contributor) na úrovni předplatného a skupiny prostředků.
7. Zobrazí objektu pro vytváření dat vytváří ve **řídicí panel** na portálu Azure, následujícím způsobem:   
8. Po úspěšném vytvoření objektu pro vytváření dat se zobrazí stránka s obsahem objektu pro vytváření dat. Pokud se stránka objektu pro vytváření dat nezobrazí, klikněte na dlaždici objektu pro vytváření dat na řídicím panelu.

    ![Okno Objekt pro vytváření dat](./media/data-factory-spark/data-factory-blade.png)

### <a name="create-linked-services"></a>Vytvoření propojených služeb
V tomto kroku vytvoříte dvě propojené služby, jednu pro váš cluster Spark propojit objekt pro vytváření dat a další propojení úložiště Azure pro vytváření dat.  

#### <a name="create-azure-storage-linked-service"></a>Vytvoření propojené služby Azure Storage
V tomto kroku propojíte se svým objektem pro vytváření dat svůj účet služby Azure Storage. Datové sady, které vytvoříte v kroku později v tomto názorném postupu odkazuje na tato propojená služba. Propojená služba HDInsight, kterou definujete v dalším kroku odkazuje na tato propojená služba příliš.  

1. Klikněte na tlačítko **vytvořit a nasadit** na **Data Factory** okno objektu pro vytváření dat. Měli byste vidět editor služby Data Factory.
2. Klikněte na **Nové datové úložiště** a zvolte **Účet Azure**.

   ![Nové úložiště dat – Azure Storage – nabídka](./media/data-factory-spark/new-data-store-azure-storage-menu.png)
3. Měli byste vidět **skript JSON** pro vytvoření Azure Storage propojená služba v editoru.

   ![Propojená služba Azure Storage](./media/data-factory-build-your-first-pipeline-using-editor/azure-storage-linked-service.png)
4. Nahraďte **název účtu** a **klíč účtu** názvem a přístupovým klíčem k účtu úložiště Azure. Chcete-li zjistit, jak získat přístupový klíč k úložišti, přečtěte si informace o zobrazení, kopírování a opětovném vygenerování přístupových klíčů k úložišti v tématu [Správa účtu úložiště](../../storage/common/storage-create-storage-account.md#manage-your-storage-account).
5. Chcete-li nasadit propojené služby, klikněte na tlačítko **nasadit** na panelu příkazů. Po úspěšném nasazení propojené služby by mělo okno **Koncept-1** zmizet a v zobrazení stromu nalevo se zobrazí služba **AzureStorageLinkedService**.

#### <a name="create-hdinsight-linked-service"></a>Vytvoření propojené služby HDInsight
V tomto kroku vytvoříte propojené služby Azure HDInsight propojit k objektu pro vytváření dat cluster HDInsight Spark. HDInsight cluster se používá ke spuštění programu Spark určeného v aktivitě Spark kanálu v této ukázce.  

1. Pokud tlačítko nevidíte, klikněte na panelu nástrojů na **... Další** na panelu nástrojů klikněte na tlačítko **nový výpočet**a potom klikněte na **clusteru HDInsight**.

    ![Vytvoření propojené služby HDInsight](media/data-factory-spark/new-hdinsight-linked-service.png)
2. Následující fragment kódu zkopírujte a vložte ho do okna **Koncept-1**. V editoru JSON proveďte následující kroky:
    1. Zadejte **URI** pro cluster HDInsight Spark. Například: `https://<sparkclustername>.azurehdinsight.net/`.
    2. Zadejte název **uživatele** kdo má přístup ke clusteru Spark.
    3. Zadejte **heslo** pro uživatele.
    4. Zadejte **propojená služba Azure Storage** který je přidružen ke clusteru HDInsight Spark. V tomto příkladu je: **AzureStorageLinkedService**.

    ```json
    {
        "name": "HDInsightLinkedService",
        "properties": {
            "type": "HDInsight",
            "typeProperties": {
                "clusterUri": "https://<sparkclustername>.azurehdinsight.net/",
                "userName": "admin",
                "password": "**********",
                "linkedServiceName": "AzureStorageLinkedService"
            }
        }
    }
    ```

    > [!IMPORTANT]
    > - Aktivita Spark nepodporuje clustery HDInsight Spark, které používají Azure Data Lake Store jako primární úložiště.
    > - Aktivita Spark podporuje pouze existující (vlastní) clusteru HDInsight Spark. HDInsight propojené služby na vyžádání nepodporuje.

    V tématu [propojená služba HDInsight](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) podrobnosti o HDInsight propojené služby.
3.  Chcete-li nasadit propojené služby, klikněte na tlačítko **nasadit** na panelu příkazů.  

### <a name="create-output-dataset"></a>Vytvoření výstupní datové sady
Výstupní datové sady je určuje plán (hodinový, denní, atd.). Proto je nutné zadat výstupní datovou sadu aktivity spark v kanálu, i když aktivita skutečně nevytváří žádný výstup. Určení vstupní datové sady pro aktivitu je volitelné.

1. V **Data Factory Editoru** klikněte na **... Další**, klikněte na **Nová datová sada** a vyberte **Azure Blob Storage**.  
2. Následující fragment kódu zkopírujte a vložte ho do okna Koncept-1. Fragmentu kódu JSON Určuje datovou sadu s názvem **OutputDataset**. Kromě toho určíte, že se mají výsledky ukládat do kontejneru objektů blob s názvem **adfspark** a ve složce s názvem **pyFiles výstupní**. Jak už bylo zmíněno dříve, je tato datová sada fiktivní datová sada. Program Spark v tomto příkladu nevytváří žádný výstup. **Dostupnosti** části určuje, že se výstupní sada vytváří denně.  

    ```json
    {
        "name": "OutputDataset",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService",
            "typeProperties": {
                "fileName": "sparkoutput.txt",
                "folderPath": "adfspark/pyFiles/output",
                "format": {
                    "type": "TextFormat",
                    "columnDelimiter": "\t"
                }
            },
            "availability": {
                "frequency": "Day",
                "interval": 1
            }
        }
    }
    ```
3. Chcete-li nasadit datovou sadu, klikněte na tlačítko **nasadit** na panelu příkazů.


### <a name="create-pipeline"></a>Vytvoření kanálu
V tomto kroku vytvoříte kanál s **HDInsightSpark** aktivity. V současnosti určuje plán výstupní datová sada, takže musíte výstupní datovou sadu vytvořit i v případě, že aktivita nevytváří žádný výstup. Pokud aktivita nemá žádný vstup, vstupní datovou sadu vytvářet nemusíte. Proto žádné vstupní datové sady je zadána v tomto příkladu.

1. V **editoru služby Data Factory**, klikněte na tlačítko **... Další** na panelu příkazů a pak klikněte na tlačítko **nový kanál**.
2. Skript v okna koncept-1 nahraďte následující skript:

    ```json
    {
        "name": "SparkPipeline",
        "properties": {
            "activities": [
                {
                    "type": "HDInsightSpark",
                    "typeProperties": {
                        "rootPath": "adfspark\\pyFiles",
                        "entryFilePath": "test.py",
                        "getDebugInfo": "Always"
                    },
                    "outputs": [
                        {
                            "name": "OutputDataset"
                        }
                    ],
                    "name": "MySparkActivity",
                    "linkedServiceName": "HDInsightLinkedService"
                }
            ],
            "start": "2017-02-05T00:00:00Z",
            "end": "2017-02-06T00:00:00Z"
        }
    }
    ```
    Je třeba počítat s následujícím:
    - **Typ** je nastavena na **HDInsightSpark**.
    - **RootPath** je nastaven na **adfspark\\pyFiles** kde adfspark je kontejner objektů Blob v Azure a pyFiles je dobře složku v daném kontejneru. V tomto příkladu úložiště objektů Blob Azure je ten, který je přidružen Spark cluster. Můžete nahrát soubor do jiného úložiště Azure. Pokud tak učiníte, vytvoření služby Azure Storage, propojené propojení objektu pro vytváření dat. Tento účet úložiště. Potom zadejte název propojené služby, jako hodnotu **sparkJobLinkedService** vlastnost. V tématu [vlastnosti aktivity Spark](#spark-activity-properties) podrobnosti o této vlastnosti a dalších vlastností podporované aktivitou Spark.  
    - **EntryFilePath** je nastaven na **test.py**, což je soubor python.
    - **Getdebuginfo –** je nastavena na **vždy**, tzn., soubory protokolů jsou vždy vygeneruje (úspěch nebo neúspěch).

        > [!IMPORTANT]
        > Doporučujeme, abyste tuto vlastnost nenastavujte na `Always` v produkčním prostředí Pokud řešíte problém.
    - **Výstupy** obsahuje jednu výstupní datovou sadu. Je třeba zadat datovou sadu výstupů i v případě, že spark program nevytváří žádný výstup. Výstupní datovou sadu jednotky plán pro kanál (hodinový, denní, atd.).  

        Podrobnosti o vlastnostech podporovaných aktivitou Spark najdete v tématu [Spark vlastnosti aktivity](#spark-activity-properties) části.
3. Chcete-li nasadit kanálu, klikněte na tlačítko **nasadit** na panelu příkazů.

### <a name="monitor-pipeline"></a>Monitorování kanálu
1. Klikněte na tlačítko **X** zavřete okna editoru služby Data Factory a přejděte zpět na domovskou stránku služby Data Factory. Klikněte na tlačítko **monitorování a správa** ke spuštění monitorování aplikací na jiné kartě.

    ![Dlaždice monitorování a Správa](media/data-factory-spark/monitor-and-manage-tile.png)
2. Změna **počáteční čas** filtru v horní části na **2/1/2017**a klikněte na tlačítko **použít**.
3. Pouze jeden interval aktivity byste měli vidět, jako je pouze jeden den mezi počátečním (2017-02-01) a koncový čas (2017-02-02) kanálu. Potvrďte, že datový řez je v **připraven** stavu.

    ![Monitorování kanálu](media/data-factory-spark/monitor-and-manage-app.png)    
4. Vyberte **okně aktivita** zobrazíte podrobnosti o aktivity při spuštění. Pokud dojde k chybě, zobrazí podrobnosti o tom, v pravém podokně.

### <a name="verify-the-results"></a>Ověření výsledků

1. Spusťte **Poznámkový blok Jupyter** pro váš cluster HDInsight Spark přechodem na: https://CLUSTERNAME.azurehdinsight.net/jupyter. Můžete také spustit řídicí panel clusteru pro váš cluster HDInsight Spark a poté spusťte **Poznámkový blok Jupyter**.
2. Klikněte na tlačítko **nový** -> **PySpark** spusťte nový poznámkový blok.

    ![Nový poznámkový blok Jupyter](media/data-factory-spark/jupyter-new-book.png)
3. Spusťte následující příkaz kopírování/vkládání textu a stisknutím klávesy **SHIFT + ENTER** na konci druhý příkaz.  

    ```sql
    %%sql

    SELECT buildingID, (targettemp - actualtemp) AS temp_diff, date FROM hvac WHERE date = \"6/1/13\"
    ```
4. Zkontrolujte, jestli data z tabulky TVK:  

    ![Výsledky dotazu Jupyter](media/data-factory-spark/jupyter-notebook-results.png)

<!-- Removed bookmark #run-a-hive-query-using-spark-sql since it doesn't exist in the target article -->
V tématu [spuštění dotazů Spark SQL](../../hdinsight/hdinsight-apache-spark-jupyter-spark-sql.md) části Podrobné pokyny. 

### <a name="troubleshooting"></a>Řešení potíží
Vzhledem k tomu, že nastavíte **getdebuginfo –** k **vždy**, uvidíte **protokolu** podsložky v **pyFiles** složky v kontejnerech objektů Blob Azure. Další podrobnosti najdete v souboru protokolu ve složce protokolů. Tento soubor protokolu je obzvláště užitečná, když dojde k chybě. V produkčním prostředí, můžete ji nastavit na **selhání**.

Další informace o řešení, postupujte takto:


1. Přejděte na `https://<CLUSTERNAME>.azurehdinsight.net/yarnui/hn/cluster`.

    ![Uživatelské rozhraní YARN aplikace](media/data-factory-spark/yarnui-application.png)  
2. Klikněte na tlačítko **protokoly** pro jedno spuštění pokusí.

    ![Stránka aplikace](media/data-factory-spark/yarn-applications.png)
3. Měli byste vidět další chybové informace na stránce protokolu.

    ![Chyba protokolu](media/data-factory-spark/yarnui-application-error.png)

Následující části obsahují informace o entit služby Data Factory používat cluster Apache Spark a aktivity Spark v datové továrně.

## <a name="spark-activity-properties"></a>Vlastnosti aktivity Spark
Zde je ukázka definici JSON kanálu s aktivitou Spark:    

```json
{
    "name": "SparkPipeline",
    "properties": {
        "activities": [
            {
                "type": "HDInsightSpark",
                "typeProperties": {
                    "rootPath": "adfspark\\pyFiles",
                    "entryFilePath": "test.py",
                    "arguments": [ "arg1", "arg2" ],
                    "sparkConfig": {
                        "spark.python.worker.memory": "512m"
                    }
                    "getDebugInfo": "Always"
                },
                "outputs": [
                    {
                        "name": "OutputDataset"
                    }
                ],
                "name": "MySparkActivity",
                "description": "This activity invokes the Spark program",
                "linkedServiceName": "HDInsightLinkedService"
            }
        ],
        "start": "2017-02-01T00:00:00Z",
        "end": "2017-02-02T00:00:00Z"
    }
}
```

Následující tabulka popisuje vlastnostech JSON použitých v definici JSON:

| Vlastnost | Popis | Požaduje se |
| -------- | ----------- | -------- |
| jméno | Název aktivity v kanálu. | Ano |
| description | Popisuje, jakým způsobem aktivita naloží text. | Ne |
| type | Tato vlastnost musí být nastavená na HDInsightSpark. | Ano |
| linkedServiceName | Název propojená služba HDInsight na kterém běží Spark program. | Ano |
| rootPath | Kontejner objektů Blob v Azure a složky, která obsahuje soubor Spark. Název souboru je malá a velká písmena. | Ano |
| entryFilePath | Relativní cesta ke kořenové složce Spark kódu nebo balíčku. | Ano |
| Název třídy | Hlavní třídy aplikace Java/Spark | Ne |
| Argumenty | Seznam argumentů příkazového řádku pro Spark program. | Ne |
| proxyUser | Uživatelský účet zosobnění spuštění programu Spark | Ne |
| sparkConfig | Zadejte hodnoty pro vlastnosti konfigurace Spark vypsané v tomto tématu: [Spark konfigurace – vlastnosti aplikace](https://spark.apache.org/docs/latest/configuration.html#available-properties). | Ne |
| getdebuginfo – | Určuje, kdy soubory protokolu Spark se zkopírují do úložiště Azure používá HDInsight cluster (nebo) zadaný ve sparkJobLinkedService. Povolené hodnoty: None, vždy nebo selhání. Výchozí hodnota: žádné. | Ne |
| sparkJobLinkedService | Azure Storage propojená služba, která obsahuje Spark soubor úlohy, závislosti a protokoly.  Pokud hodnotu pro tuto vlastnost nezadáte, použije se úložiště přidružený k clusteru HDInsight. | Ne |

## <a name="folder-structure"></a>Struktura složek
Aktivita Spark nepodporuje skriptu v řádku jako Pig a do aktivity Hive. Spark úlohy jsou také více extensible než úlohy Pig nebo Hive. Pro úlohy Spark, můžete zadat více závislostí, jako jar balíčky (umístěné v jazyce java cesty pro třídy), soubory python (umístit na PYTHONPATH) a všechny další soubory.

Vytvořte následující strukturu složek ve službě Azure Blob storage, na kterou odkazuje propojená služba HDInsight. Potom obsah nahrajete soubory závislé na příslušné podsložek v kořenové složce reprezentována **entryFilePath**. Například nahrajte python soubory do pyFiles podsložky a soubory jar do podsložky JAR kořenové složky. Služba Data Factory v době běhu očekává následující strukturu složek ve službě Azure Blob storage:     

| Cesta | Popis | Požaduje se | Typ |
| ---- | ----------- | -------- | ---- |
| . | Kořenová cesta úlohy Spark v úložišti propojená služba  | Ano | Složka |
| &lt;uživatelem definované&gt; | Cesta k souboru položka úlohy Spark | Ano | File |
| . / jars | Všechny soubory v této složce se nahrála a umístit na cestě třídy java clusteru | Ne | Složka |
| . / pyFiles | Všechny soubory v této složce se nahrála a umístit do PYTHONPATH clusteru | Ne | Složka |
| . / soubory | Všechny soubory v této složce se nahrála a umístit na vykonavatele pracovní adresář | Ne | Složka |
| . / archivy | Všechny soubory v této složce nekomprimované | Ne | Složka |
| . / protokoly | Složky, kde jsou uložené protokoly z clusteru Spark.| Ne | Složka |

Tady je příklad pro úložiště, který obsahuje dva soubory úlohy Spark v Azure Blob Storage odkazuje propojená služba HDInsight.

```
SparkJob1
    main.jar
    files
        input1.txt
        input2.txt
    jars
        package1.jar
        package2.jar
    logs

SparkJob2
    main.py
    pyFiles
        scrip1.py
        script2.py
    logs
```
