---
title: "Sestavení prvního objektu pro vytváření dat (Azure Portal) | Dokumentace Microsoftu"
description: "V tomto kurzu vytvoříte pomocí editoru služby Data Factory na webu Azure Portal ukázkový kanál služby Azure Data Factory."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: d5b14e9e-e358-45be-943c-5297435d402d
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 12/06/2016
ms.author: spelluru
translationtype: Human Translation
ms.sourcegitcommit: fbf77e9848ce371fd8d02b83275eb553d950b0ff
ms.openlocfilehash: c9f2e3beafd19e0d4d62e409a80da336be17b90b


---
# <a name="tutorial-build-your-first-azure-data-factory-using-azure-portal"></a>Kurz: Sestavení prvního objektu pro vytváření dat Azure pomocí webu Azure Portal
> [!div class="op_single_selector"]
> * [Přehled a požadavky](data-factory-build-your-first-pipeline.md)
> * [Azure Portal](data-factory-build-your-first-pipeline-using-editor.md)
> * [Visual Studio](data-factory-build-your-first-pipeline-using-vs.md)
> * [PowerShell](data-factory-build-your-first-pipeline-using-powershell.md)
> * [Šablona Resource Manageru](data-factory-build-your-first-pipeline-using-arm.md)
> * [REST API](data-factory-build-your-first-pipeline-using-rest-api.md)


V tomto článku se dozvíte, jak vytvořit první objekt pro vytváření dat Azure pomocí [webu Azure Portal](https://portal.azure.com/). Pokud chcete udělat kurz pomocí jiných nástrojů nebo sad SDK, vyberte jednu z možností z rozevíracího seznamu. 

> [!NOTE]
> Datový kanál v tomto kurzu transformuje vstupní data, aby vytvořil výstupní data. Nekopíruje data ze zdrojového úložiště dat do cílového úložiště dat. Kurz předvádějící způsoby kopírování dat pomocí Azure Data Factory najdete v tématu popisujícím [kurz kopírování dat z Blob Storage do SQL Database](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).
> 
> Dvě aktivity můžete zřetězit (spustit jednu aktivitu po druhé) nastavením výstupní datové sady jedné aktivity jako vstupní datové sady druhé aktivity. Podrobné informace najdete v tématu s popisem [plánování a provádění ve službě Data Factory](data-factory-scheduling-and-execution.md). 

## <a name="prerequisites"></a>Požadavky
1. Přečtěte si článek [Přehled kurzu](data-factory-build-your-first-pipeline.md) a proveďte **nutné** kroky.
2. Tento článek neposkytuje koncepční přehled služby Azure Data Factory. Doporučujeme projít si podrobnější přehled služby, který najdete v článku [Úvod do Azure Data Factory](data-factory-introduction.md).  

## <a name="create-data-factory"></a>Vytvoření objektu pro vytváření dat
Objekt pro vytváření dat může mít jeden nebo víc kanálů. Kanál může obsahovat jednu nebo víc aktivit. Může obsahovat třeba aktivitu kopírování, která slouží ke kopírování dat ze zdrojového do cílového úložiště dat, a aktivitu HDInsight Hive pro spuštění skriptu Hive, který umožňuje transformovat vstupní data na výstupní data produktu. V tomto kroku začneme vytvořením objektu pro vytváření dat.

1. Přihlaste se k [portálu Azure](https://portal.azure.com/).
2. V nabídce vlevo klikněte na **NOVÝ**, klikněte na **Data + Analýza** a poté na **Objekt pro vytváření dat**.

   ![Okno Vytvořit](./media/data-factory-build-your-first-pipeline-using-editor/create-blade.png)
3. V okně **Nový objekt pro vytváření dat** zadejte název **GetStartedDF**.

   ![Okno Nový objekt pro vytváření dat](./media/data-factory-build-your-first-pipeline-using-editor/new-data-factory-blade.png)

   > [!IMPORTANT]
   > Název objektu pro vytváření dat Azure musí být **globálně jedinečný**. Pokud se zobrazí chyba: **Název objektu pro vytváření dat GetStartedDF není k dispozici**, změňte název objektu pro vytváření dat (např. na váš_název_GetStartedDF) a zkuste ho vytvořit znovu. V tématu [Objekty pro vytváření dat – pravidla pojmenování](data-factory-naming-rules.md) najdete pravidla pojmenování artefaktů služby Data Factory.
   >
   > Název objektu pro vytváření dat se může v budoucnu zaregistrovat jako název **DNS** a tak se stát veřejně viditelným.
   >
   >
4. Vyberte **předplatné Azure**, ve kterém chcete objekt pro vytváření dat vytvořit.
5. Vyberte existující **skupinu prostředků** nebo vytvořte novou. Pro účely tohoto kurzu vytvořte skupinu prostředků s názvem **ADFGetStartedRG**.
6. V okně **Nový objekt pro vytváření dat** klikněte na **Vytvořit**.

   > [!IMPORTANT]
   > Chcete-li vytvářet instance služby Data Factory, musíte být členem role [Přispěvatel Data Factory](../active-directory/role-based-access-built-in-roles.md#data-factory-contributor) na úrovni předplatného a skupiny prostředků.
   >
   >
7. Na dlaždici **Úvodní panel** na webu Azure Portal uvidíte, jak se objekt pro vytváření dat vytváří:   

   ![Stav vytváření objektu pro vytváření dat](./media/data-factory-build-your-first-pipeline-using-editor/creating-data-factory-image.png)
8. Blahopřejeme! Úspěšně jste vytvořili první objekt pro vytváření dat. Po úspěšném vytvoření objektu pro vytváření dat se zobrazí stránka s obsahem objektu pro vytváření dat.     

    ![Okno Objekt pro vytváření dat](./media/data-factory-build-your-first-pipeline-using-editor/data-factory-blade.png)

Před vytvořením kanálu v objektu pro vytváření dat je nejdříve potřeba vytvořit několik entit služby Data Factory. Nejprve vytvoříte propojené služby, které propojí úložiště dat a výpočetní služby s vaším úložištěm dat, definujete vstupní a výstupní datové sady reprezentující vstupní a výstupní data v propojených úložištích dat, a poté vytvoříte kanál s aktivitou, která tyto datové sady používá.

## <a name="create-linked-services"></a>Vytvoření propojených služeb
V tomto kroku propojíte svůj účet služby Azure Storage a cluster Azure HDInsight na vyžádání s objektem pro vytváření dat. Účet služby Azure Storage v této ukázce obsahuje vstupní a výstupní data pro kanál. Propojená služba HDInsight slouží v této ukázce ke spuštění skriptu Hive určeného v aktivitě kanálu. Určete, jaké [úložiště dat](data-factory-data-movement-activities.md) a /[výpočetní služby](data-factory-compute-linked-services.md) se ve vašem scénáři používají, a vytvořením propojených služeb propojte tyto služby s objektem pro vytváření dat.  

### <a name="create-azure-storage-linked-service"></a>Vytvoření propojené služby Azure Storage
V tomto kroku propojíte se svým objektem pro vytváření dat svůj účet služby Azure Storage. V tomto kurzu použijete tento účet služby Azure Storage také k uložení vstupních a výstupních dat a souboru skriptu HQL.

1. V okně **OBJEKT PRO VYTVÁŘENÍ DAT** pro objekt **GetStartedDF** klikněte na **Vytvořit a nasadit**. Měli byste vidět editor služby Data Factory.

   ![Dlaždice Vytvořit a nasadit](./media/data-factory-build-your-first-pipeline-using-editor/data-factory-author-deploy.png)
2. Klikněte na **Nové datové úložiště** a zvolte **Účet Azure**.

   ![Nové úložiště dat – Azure Storage – nabídka](./media/data-factory-build-your-first-pipeline-using-editor/new-data-store-azure-storage-menu.png)
3. V editoru by se měl zobrazit skript JSON pro vytvoření propojené služby Azure Storage.

   ![Propojená služba Azure Storage](./media/data-factory-build-your-first-pipeline-using-editor/azure-storage-linked-service.png)
4. Nahraďte **název účtu** názvem účtu služby Azure Storage a **klíč účtu** přístupovým klíčem k účtu Azure Storage. Chcete-li zjistit, jak získat přístupový klíč k úložišti, přečtěte si informace o zobrazení, kopírování a opětovném vygenerování přístupových klíčů k úložišti v tématu [Správa účtu úložiště](../storage/storage-create-storage-account.md#manage-your-storage-account).
5. Propojenou službu nasadíte kliknutím na **Nasadit** na panelu příkazů.

    ![Tlačítko Nasadit](./media/data-factory-build-your-first-pipeline-using-editor/deploy-button.png)

   Po úspěšném nasazení propojené služby by mělo okno **Koncept-1** zmizet a v zobrazení stromu nalevo se zobrazí služba **AzureStorageLinkedService**.

    ![Propojená služba úložiště v nabídce](./media/data-factory-build-your-first-pipeline-using-editor/StorageLinkedServiceInTree.png)    

### <a name="create-azure-hdinsight-linked-service"></a>Vytvoření propojené služby Azure HDInsight
V tomto kroku propojíte se svým objektem pro vytváření dat cluster HDInsight na vyžádání. Cluster HDInsight se automaticky vytvoří za běhu, a až dokončí zpracování, po určité zadané době nečinnosti se odstraní.

1. V **Data Factory Editoru** klikněte na **... Další**, klikněte na **Nový výpočet** a vyberte **Cluster HDInsight na vyžádání**.

    ![Nový výpočet](./media/data-factory-build-your-first-pipeline-using-editor/new-compute-menu.png)
2. Následující fragment kódu zkopírujte a vložte ho do okna **Koncept-1**. Tento fragment kódu JSON popisuje vlastnosti, které se použijí při vytváření clusteru HDInsight na vyžádání.

    ```JSON
    {
      "name": "HDInsightOnDemandLinkedService",
      "properties": {
        "type": "HDInsightOnDemand",
        "typeProperties": {
          "version": "3.2",
          "clusterSize": 1,
          "timeToLive": "00:30:00",
          "linkedServiceName": "AzureStorageLinkedService"
        }
      }
    }
    ```

    Následující tabulka obsahuje popis vlastností použitých v tomto fragmentu kódu JSON:

   | Vlastnost | Popis |
   |:--- |:--- |
   | Version |Určuje, že vytvářený cluster HDInsight má mít verzi 3.2. |
   | ClusterSize |Určuje velikost clusteru HDInsight. |
   | TimeToLive |Určuje dobu nečinnosti před odstraněním clusteru HDInsight. |
   | linkedServiceName |Určuje účet úložiště, který se má použít k ukládání protokolů generovaných clusterem HDInsight. |

    Je třeba počítat s následujícím:

   * Pomocí tohoto kódu JSON služba Data Factory vytvoří cluster HDInsight **se systémem Windows** za vás. Můžete ale také vytvořit cluster HDInsight **se systémem Linux**. Podrobnosti najdete v tématu [Propojená služba HDInsight na vyžádání](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service).
   * Místo clusteru HDInsight na vyžádání můžete použít také **vlastní cluster HDInsight**. Podrobnosti najdete v tématu [Propojená služba HDInsight](data-factory-compute-linked-services.md#azure-hdinsight-linked-service).
   * Cluster HDInsight vytvoří **výchozí kontejner** ve službě Blob Storage, kterou jste určili v kódu JSON (**linkedServiceName**). Při odstranění clusteru HDInsight neprovede odstranění tohoto kontejneru. Toto chování je záměrné. Díky propojené službě HDInsight na vyžádání se cluster HDInsight vytvoří pokaždé, když je zpracován určitý řez, pokud neexistuje aktivní cluster (**timeToLive**). Po dokončení zpracování se cluster automaticky odstraní.

       Po zpracování dalších řezů se vám ve službě Azure Blob Storage objeví spousta kontejnerů. Pokud je nepotřebujete k řešení potíží s úlohami, můžete je odstranit, abyste snížili náklady na úložiště. Názvy těchto kontejnerů používají následující formát: „adf**název_vašeho_objektu_pro_vytváření_dat**-**název_propojené_služby**-razítko_data_a_času“. K odstranění kontejnerů ze služby Azure Blob Storage můžete použít nástroje, jako je třeba [Průzkumník úložišť od Microsoftu](http://storageexplorer.com/).

     Podrobnosti najdete v tématu [Propojená služba HDInsight na vyžádání](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service).
3. Propojenou službu nasadíte kliknutím na **Nasadit** na panelu příkazů.

    ![Nasazení propojené služby HDInsight na vyžádání](./media/data-factory-build-your-first-pipeline-using-editor/ondemand-hdinsight-deploy.png)
4. Zkontrolujte, jestli se v zobrazení stromu vlevo zobrazuje služba **AzureStorageLinkedService** i služba **HDInsightOnDemandLinkedService**.

    ![Zobrazení stromu s propojenými službami](./media/data-factory-build-your-first-pipeline-using-editor/tree-view-linked-services.png)

## <a name="create-datasets"></a>Vytvoření datových sad
V tomto kroku vytvoříte datové sady, které představují vstupní a výstupní data pro zpracování Hive. Tyto datové sady odkazují na službu **AzureStorageLinkedService**, kterou už jste v tomto kurzu vytvořili. Propojená služba odkazuje na účet služby Azure Storage a datové sady určují kontejner, složku a název souboru v úložišti, který obsahuje vstupní a výstupní data.   

### <a name="create-input-dataset"></a>Vytvoření vstupní datové sady
1. V **Data Factory Editoru** klikněte na **... Další**, klikněte na **Nová datová sada** a vyberte **Azure Blob Storage**.

    ![Nová datová sada](./media/data-factory-build-your-first-pipeline-using-editor/new-data-set.png)
2. Následující fragment kódu zkopírujte a vložte ho do okna Koncept-1. V tomto fragmentu kódu JSON vytváříte datovou sadu s názvem **AzureBlobInput**, která představuje vstupní data pro aktivitu v kanálu. Kromě toho určujete, že se vstupní data nachází v kontejneru objektů blob s názvem **adfgetstarted** ve složce s názvem **inputdata**.

    ```JSON
    {
        "name": "AzureBlobInput",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService",
            "typeProperties": {
                "fileName": "input.log",
                "folderPath": "adfgetstarted/inputdata",
                "format": {
                    "type": "TextFormat",
                    "columnDelimiter": ","
                }
            },
            "availability": {
                "frequency": "Month",
                "interval": 1
            },
            "external": true,
            "policy": {}
        }
    }
    ```
    Následující tabulka obsahuje popis vlastností použitých v tomto fragmentu kódu JSON:

   | Vlastnost | Popis |
   |:--- |:--- |
   | type |Vlastnost type je nastavená na hodnotu AzureBlob, protože se data nachází ve službě Azure Blob Storage. |
   | linkedServiceName |Odkazuje na službu AzureStorageLinkedService, kterou jste vytvořili předtím. |
   | fileName |Tato vlastnost je nepovinná. Pokud ji vynecháte, vyberou se všechny soubory v cestě folderPath. V tomto případě se zpracovává jenom soubor input.log. |
   | type |Soubory protokolů jsou v textovém formátu, takže použijeme hodnotu TextFormat. |
   | columnDelimiter |Sloupce v souborech protokolu jsou oddělené znakem čárky (,) |
   | frequency/interval |Frekvence je nastavená na hodnotu Month (Měsíc) a interval je 1, takže vstupní řezy jsou dostupné jednou za měsíc. |
   | external |Pokud vstupní data nevygenerovala služba Data Factory, je tato vlastnost nastavená na hodnotu true. |
3. Nově vytvořenou datovou sadu nasadíte kliknutím na **Nasadit** na panelu příkazů. Datová sada by se měla objevit v zobrazení stromu vlevo.

### <a name="create-output-dataset"></a>Vytvoření výstupní datové sady
Nyní vytvoříte výstupní datovou sadu, která bude představovat výstupní data ve službě Azure Blob Storage.

1. V **Data Factory Editoru** klikněte na **... Další**, klikněte na **Nová datová sada** a vyberte **Azure Blob Storage**.  
2. Následující fragment kódu zkopírujte a vložte ho do okna Koncept-1. V tomto fragmentu kódu JSON vytváříte datovou sadu s názvem **AzureBlobOutput** a určujete strukturu dat vytvářených skriptem Hive. Kromě toho určujete, že se mají výsledky ukládat do kontejneru objektů blob s názvem **adfgetstarted** do složky s názvem **partitioneddata**. Oddíl **availability** určuje, že se výstupní sada vytváří jednou měsíčně.

    ```JSON
    {
      "name": "AzureBlobOutput",
      "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties": {
          "folderPath": "adfgetstarted/partitioneddata",
          "format": {
            "type": "TextFormat",
            "columnDelimiter": ","
          }
        },
        "availability": {
          "frequency": "Month",
          "interval": 1
        }
      }
    }
    ```
    Popisy těchto vlastností najdete v části **Vytvoření vstupní datové sady**. U výstupní datové sady nenajdete vlastnost external, protože datovou sadu vytváří služba Data Factory.
3. Nově vytvořenou datovou sadu nasadíte kliknutím na **Nasadit** na panelu příkazů.
4. Zkontrolujte, jestli se datová sada úspěšně vytvořila.

    ![Zobrazení stromu s propojenými službami](./media/data-factory-build-your-first-pipeline-using-editor/tree-view-data-set.png)

## <a name="create-pipeline"></a>Vytvoření kanálu
V tomto kroku vytvoříte svůj první kanál s aktivitou **HDInsightHive**. Vstupní řez je dostupný jednou měsíčně (frequency: Month, interval: 1), výstupní řez se vytváří také jednou měsíčně a vlastnost scheduler pro aktivitu je také nastavena na jednou měsíčně. Nastavení výstupní datové sady a vlastnosti scheduler se musí shodovat. V současnosti určuje plán výstupní datová sada, takže musíte výstupní datovou sadu vytvořit i v případě, že aktivita nevytváří žádný výstup. Pokud aktivita nemá žádný vstup, vstupní datovou sadu vytvářet nemusíte. Vysvětlení vlastností použitých v následujícím kódu JSON najdete na konci této části.

1. V **editoru služby Data Factory** klikněte na **(…) Další příkazy** a poté klikněte na **Nový kanál**.

    ![Tlačítko Nový kanál](./media/data-factory-build-your-first-pipeline-using-editor/new-pipeline-button.png)
2. Následující fragment kódu zkopírujte a vložte ho do okna Koncept-1.

   > [!IMPORTANT]
   > V kódu JSON nahraďte **storageaccountname** názvem vašeho účtu úložiště.
   >
   >

    ```JSON
    {
        "name": "MyFirstPipeline",
        "properties": {
            "description": "My first Azure Data Factory pipeline",
            "activities": [
                {
                    "type": "HDInsightHive",
                    "typeProperties": {
                        "scriptPath": "adfgetstarted/script/partitionweblogs.hql",
                        "scriptLinkedService": "AzureStorageLinkedService",
                        "defines": {
                            "inputtable": "wasb://adfgetstarted@<storageaccountname>.blob.core.windows.net/inputdata",
                            "partitionedtable": "wasb://adfgetstarted@<storageaccountname>.blob.core.windows.net/partitioneddata"
                        }
                    },
                    "inputs": [
                        {
                            "name": "AzureBlobInput"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "AzureBlobOutput"
                        }
                    ],
                    "policy": {
                        "concurrency": 1,
                        "retry": 3
                    },
                    "scheduler": {
                        "frequency": "Month",
                        "interval": 1
                    },
                    "name": "RunSampleHiveActivity",
                    "linkedServiceName": "HDInsightOnDemandLinkedService"
                }
            ],
            "start": "2016-04-01T00:00:00Z",
            "end": "2016-04-02T00:00:00Z",
            "isPaused": false
        }
    }
    ```

    V tomto fragmentu kódu JSON vytváříte kanál sestávající z jediné aktivity, která zpracovává data v clusteru HDInsight pomocí skriptu Hive.

    Soubor skriptu Hive **partitionweblogs.hql** je uložený v účtu služby Azure Storage (který určuje služba scriptLinkedService s názvem **AzureStorageLinkedService**) a ve složce **script** v kontejneru **adfgetstarted**.

    Oddíl **defines** určuje nastavení běhového prostředí, které se předá skriptu Hive jako konfigurační hodnoty Hive (např. ${hiveconf:inputtable}, ${hiveconf:partitionedtable}).

    Vlastnosti kanálu **start** a **end** určují období aktivity kanálu.

    V kódu JSON aktivity určujete, že má skript Hive běžet ve výpočetní službě určené vlastností **linkedServiceName**, tedy **HDInsightOnDemandLinkedService**.

   > [!NOTE]
   > V části kódu JSON kanálu v tématu [Kanály a aktivity ve službě Azure Data Factory](data-factory-create-pipelines.md) najdete podrobnosti o vlastnostech JSON použitých v příkladu.
   >
   >
3. Zkontrolujte:

   1. Jestli ve složce **inputdata** v kontejneru **adfgetstarted** v úložišti objektů blob Azure existuje soubor **input.log**.
   2. Jestli ve složce **script** v kontejneru **adfgetstarted** v úložišti objektů blob Azure existuje soubor **partitionweblogs.hql**. Pokud tyto soubory nevidíte, proveďte požadované kroky uvedené v článku [Přehled kurzu](data-factory-build-your-first-pipeline.md).
   3. Ujistěte se, že jste položku **storageaccountname** v kódu JSON kanálu nahradili názvem účtu úložiště.
4. Kanál nasadíte kliknutím na **Nasadit** na panelu příkazů. Časy **start** a **end** jsou nastavené na minulost a vlastnost **isPaused** má hodnotu false, takže se kanál (aktivita v kanálu) spustí hned po nasazení.
5. Zkontrolujte, jestli se kanál objevil v zobrazení stromu.

    ![Zobrazení stromu s kanálem](./media/data-factory-build-your-first-pipeline-using-editor/tree-view-pipeline.png)
6. Úspěšně jste vytvořili první kanál, blahopřejeme!

## <a name="monitor-pipeline"></a>Monitorování kanálu
### <a name="monitor-pipeline-using-diagram-view"></a>Monitorování kanálu pomocí Zobrazení diagramu
1. Kliknutím na **X** zavřete editor služby Data Factory a vrátíte se zpátky do okna Objekt pro vytváření dat. Tam klikněte na **Diagram**.

    ![Dlaždice Diagram](./media/data-factory-build-your-first-pipeline-using-editor/diagram-tile.png)
2. V zobrazení diagramu uvidíte přehled kanálů a datové sady použité v tomto kurzu.

    ![Zobrazení diagramu](./media/data-factory-build-your-first-pipeline-using-editor/diagram-view-2.png)
3. Pokud chcete zobrazit všechny aktivity v kanálu, klikněte na kanál v diagramu pravým tlačítkem myši a potom klikněte na Otevřít kanál.

    ![Nabídka Otevřít kanál](./media/data-factory-build-your-first-pipeline-using-editor/open-pipeline-menu.png)
4. Zkontrolujte, jestli je v kanálu vidět aktivita HDInsightHive.

    ![Zobrazení Otevřít kanál](./media/data-factory-build-your-first-pipeline-using-editor/open-pipeline-view.png)

    Pokud se chcete vrátit do předchozího zobrazení, klikněte v nabídce navigace s popisem cesty v horní části na **Objekt pro vytváření dat**.
5. V **zobrazení diagramu** dvakrát klikněte na datovou sadu **AzureBlobInput**. Zkontrolujte, jestli je řez ve stavu **Připraveno**. Než se řez zobrazí ve stavu Připraveno, může to několik minut trvat. Pokud se to do nějaké doby nestane, zkontrolujte, jestli je vstupní soubor (input.log) umístěný ve správném kontejneru (adfgetstarted) a složce (inputdata).

   ![Vstupní řez ve stavu Připraveno](./media/data-factory-build-your-first-pipeline-using-editor/input-slice-ready.png)
6. Kliknutím na tlačítko **X** zavřete okno **AzureBlobInput**.
7. V **zobrazení diagramu** dvakrát klikněte na datovou sadu **AzureBlobOutput**. Zobrazí se řez, který se právě zpracovává.

   ![Datová sada](./media/data-factory-build-your-first-pipeline-using-editor/dataset-blade.png)
8. Po dokončení zpracování bude řez ve stavu **Připraveno**.

   ![Datová sada](./media/data-factory-build-your-first-pipeline-using-editor/dataset-slice-ready.png)  

   > [!IMPORTANT]
   > Vytváření clusteru HDInsight na vyžádání většinou nějakou dobu trvá (přibližně 20 minut). Proto počítejte s tím, že zpracování řezu kanálem bude trvat **přibližně 30 minut**.
   >
   >

9. Pokud je řez ve stavu **Připraveno**, zkontrolujte, jestli se ve složce **partitioneddata** v kontejneru **adfgetstarted** ve službě Blob Storage nachází výstupní data.  

   ![Výstupní data](./media/data-factory-build-your-first-pipeline-using-editor/three-ouptut-files.png)
10. Kliknutím na řez otevřete okno **Datový řez** s podrobnostmi o řezu.

   ![Podrobnosti o datovém řezu](./media/data-factory-build-your-first-pipeline-using-editor/data-slice-details.png)  
11. Kliknutím na spuštění aktivity v **seznamu spuštění aktivit** zobrazíte podrobnosti o spuštění aktivity (v našem scénáři aktivity Hivu) v okně **Podrobnosti o spuštění aktivit**.   

   ![Podrobnosti o spuštění aktivit](./media/data-factory-build-your-first-pipeline-using-editor/activity-window-blade.png)    

   V souborech protokolů můžete zobrazit provedený dotaz Hivu s informacemi o jeho stavu. Tyto protokoly jsou užitečné při řešení potíží.
   Další podrobnosti najdete v článku [Monitorování a správa kanálů pomocí oken Azure Portal](data-factory-monitor-manage-pipelines.md).

> [!IMPORTANT]
> Po úspěšném zpracování řezu se vstupní soubor odstraní. Pokud tedy chcete spustit řez znovu nebo si znovu projít tento kurz, načtěte vstupní soubor (input.log) do složky inputdata v kontejneru adfgetstarted.
>
>

### <a name="monitor-pipeline-using-monitor--manage-app"></a>Monitorování kanálu pomocí aplikace pro monitorování a správu
K monitorování kanálů můžete také použít aplikaci pro monitorování a správu. Podrobnosti o použití této aplikace najdete v tématu [Monitorování a správa kanálů služby Azure Data Factory pomocí aplikace pro monitorování a správu](data-factory-monitor-manage-app.md).

1. Na domovské stránce svého objektu pro vytváření dat klikněte na dlaždici **Monitorování a správa**.

    ![Dlaždice Monitorování a správa](./media/data-factory-build-your-first-pipeline-using-editor/monitor-and-manage-tile.png)
2. Měla by se zobrazit **aplikace pro monitorování a správu**. Změňte hodnoty **Čas spuštění** a **Čas ukončení** tak, aby odpovídaly času spuštění (01.04.2016 00:00) a ukončení (02.04.2016 00:00) vašeho kanálu, a klikněte na **Použít**.

    ![Aplikace pro monitorování a správu](./media/data-factory-build-your-first-pipeline-using-editor/monitor-and-manage-app.png)
3. Výběrem okna aktivity v seznamu **Okna aktivit** zobrazíte podrobnosti.

    ![Podrobnosti o okně aktivity](./media/data-factory-build-your-first-pipeline-using-editor/activity-window-details.png)

## <a name="summary"></a>Souhrn
V tomto kurzu jste vytvořili objekt pro zpracování dat Azure, který zpracovává data pomocí skriptu Hive v clusteru HDInsight Hadoop. Pomocí editoru služby Data Factory na webu Azure Portal jste provedli tyto kroky:  

1. Vytvořili jste **objekt pro vytváření dat** Azure.
2. Vytvořili jste dvě **propojené služby**:
   1. Propojená služba **Azure Storage** spojuje úložiště objektů blob Azure, které obsahuje vstupní/výstupní soubory, s objektem pro vytváření dat.
   2. Propojená služba na vyžádání **Azure HDInsight** spojuje cluster na vyžádání HDInsight Hadoop s objektem pro vytváření dat. Služba Azure Data Factory vytvoří cluster HDInsight Hadoop ve chvíli, kdy je potřeba zpracovat vstupní data a vygenerovat výstupní data.
3. Vytvořili jste dvě **datové sady**, které popisují vstupní a výstupní data aktivity HDInsight Hive v kanálu.
4. Vytvořili jste **kanál** s aktivitou **HDInsight Hive**.

## <a name="next-steps"></a>Další kroky
V tomto článku jste vytvořili kanál s aktivitou transformace (aktivita HDInsight), která v clusteru HDInsight na vyžádání spouští skript Hive. Pokud chcete zjistit, jak pomocí aktivity kopírování zkopírovat data z Azure Blob do Azure SQL, projděte si článek [Kurz: Kopírování dat z Azure Blob do Azure SQL](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).

## <a name="see-also"></a>Viz také
| Téma | Popis |
|:--- |:--- |
| [Kanály](data-factory-create-pipelines.md) |Tento článek vám pomůže pochopit kanály a aktivity ve službě Azure Data Factory a porozumět tomu, jak se dají ve vaší situaci nebo firmě použít k sestavení kompletních pracovních postupů založených na datech. |
| [Datové sady](data-factory-create-datasets.md) |Tento článek vám pomůže pochopit datové sady ve službě Azure Data Factory. |
| [Plánování a provádění](data-factory-scheduling-and-execution.md) |Tento článek vysvětluje aspekty plánování a provádění aplikačního modelu služby Azure Data Factory. |
| [Monitorování a správa kanálů pomocí monitorovací aplikace](data-factory-monitor-manage-app.md) |Tento článek popisuje, jak monitorovat, spravovat a ladit kanály pomocí aplikace pro monitorování a správu. |



<!--HONumber=Feb17_HO1-->


