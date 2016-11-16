---
title: "Kurz: Vytvoření kanálu s aktivitou kopírování pomocí webu Azure Portal | Dokumentace Microsoftu"
description: "V tomto kurzu vytvoříte kanál služby Azure Data Factory s aktivitou kopírování pomocí editoru služby Data Factory na webu Azure Portal."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: d9317652-0170-4fd3-b9b2-37711272162b
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 09/16/2016
ms.author: spelluru
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: cee537753b025ed5119c116dfcc644101be3271f


---
# <a name="tutorial-create-a-pipeline-with-copy-activity-using-azure-portal"></a>Kurz: Vytvoření kanálu s aktivitou kopírování pomocí webu Azure Portal
> [!div class="op_single_selector"]
> * [Přehled a požadavky](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
> * [Průvodce kopírováním](data-factory-copy-data-wizard-tutorial.md)
> * [Azure Portal](data-factory-copy-activity-tutorial-using-azure-portal.md)
> * [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md)
> * [PowerShell](data-factory-copy-activity-tutorial-using-powershell.md)
> * [Šablona Azure Resource Manageru](data-factory-copy-activity-tutorial-using-azure-resource-manager-template.md)
> * [REST API](data-factory-copy-activity-tutorial-using-rest-api.md)
> * [.NET API](data-factory-copy-activity-tutorial-using-dotnet-api.md)
> 
> 

V tomto kurzu je uvedeno, jak vytvořit a monitorovat objekt pro vytváření dat Azure pomocí webu Azure Portal. Kanál v objektu pro vytváření dat využívá aktivitu kopírování, s jejíž pomocí kopíruje data ze služby Azure Blob Storage do služby Azure SQL Database.

Zde jsou kroky, které provedete v rámci tohoto kurzu:

| Krok | Popis |
| --- | --- |
| [Vytvoření služby Azure Data Factory](#create-data-factory) |V tomto kroku vytvoříte objekt pro vytváření dat Azure s názvem **ADFTutorialDataFactory**. |
| [Vytvoření propojených služeb](#create-linked-services) |V tomto kroku vytvoříte dvě propojené služby: **AzureStorageLinkedService** a **AzureSqlLinkedService**. <br/><br/>AzureStorageLinkedService propojuje službu Azure Storage a AzureSqlLinkedService propojuje službu Azure SQL Database k objektu ADFTutorialDataFactory. Vstupní data pro kanál se nachází v kontejneru objektů blob v Azure Blob Storage a výstupní data budou uložena v tabulce v Azure SQL Database. Proto přidáváte tyto dvě úložiště dat jako propojené služby objektu pro vytváření dat. |
| [Vytvoření vstupní a výstupní datové sady](#create-datasets) |V předchozím kroku jste vytvořili propojené služby, které odkazují na úložiště dat, která obsahují vstupní a výstupní data. V tomto kroku nadefinujete dvě datové sady – **InputDataset** a **OutputDataset** – které představují vstupní a výstupní data uložená v úložištích dat. <br/><br/>Pro InputDataset zadáte kontejner objektů blob, který obsahuje objekt blob se zdrojovými daty, a pro OutputDataset zadáte tabulku SQL, do které se uloží výstupní data. Zadáte také další vlastnosti, jako jsou například struktura, dostupnost a zásady. |
| [Vytvoření kanálu](#create-pipeline) |V tomto kroku vytvoříte v objektu ADFTutorialDataFactory kanál s názvem **ADFTutorialPipeline**. <br/><br/>Přidáte do kanálu **aktivitu kopírování**, která kopíruje vstupní data z objektu blob Azure do výstupní tabulky Azure SQL. Aktivita kopírování provádí přesun dat ve službě Azure Data Factory. Používá globálně dostupnou službu, která může kopírovat data mezi různými úložišti dat zabezpečeným, spolehlivým a škálovatelným způsobem. Podrobnosti o aktivitě kopírování najdete v článku [Aktivity přesunu dat](data-factory-data-movement-activities.md). |
| [Monitorování kanálu](#monitor-pipeline) |V tomto kroku budete monitorovat řezy vstupní a výstupní tabulky pomocí webu Azure Portal. |

## <a name="prerequisites"></a>Požadavky
Než se pustíte do tohoto kurzu, dokončete požadované kroky uvedené v článku [Přehled kurzu](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).

## <a name="create-data-factory"></a>Vytvoření objektu pro vytváření dat
V tomto kroku vytvoříte pomocí webu Azure Portal objekt pro vytváření dat Azure s názvem **ADFTutorialDataFactory**.

1. Po přihlášení na webu [Azure Portal](https://portal.azure.com/) klikněte na **Nový**, vyberte **Inteligence a analýza** a klikněte na **Data Factory**. 
   
   ![Nový -> Objekt pro vytváření dat](./media/data-factory-copy-activity-tutorial-using-azure-portal/NewDataFactoryMenu.png)    
2. V okně **Nový objekt pro vytváření dat**:
   
   1. Do pole **Název** zadejte **ADFTutorialDataFactory**. 
      
         ![Okno Nový objekt pro vytváření dat](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-new-data-factory.png)
      
       Název objektu pro vytváření dat Azure musí být **globálně jedinečný**. Pokud se zobrazí následující chyba, změňte název objektu pro vytváření dat (třeba na váš_název_ADFTutorialDataFactory) a zkuste to znovu. V tématu [Objekty pro vytváření dat – pravidla pojmenování](data-factory-naming-rules.md) najdete pravidla pojmenování artefaktů služby Data Factory.
      
           Data factory name “ADFTutorialDataFactory” is not available  
      
       ![Název objektu pro vytváření dat není k dispozici](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-data-factory-not-available.png)
   2. Vyberte své **předplatné** Azure.
   3. Pro skupinu prostředků proveďte jeden z následujících kroků:
      
      1. Vyberte **Použít existující** a z rozevíracího seznamu vyberte existující skupinu prostředků. 
      2. Vyberte **Vytvořit novou** a zadejte název skupiny prostředků.   
         
          Některé kroky v tomto kurzu vychází z předpokladu, že pro skupinu prostředků použijete název **ADFTutorialResourceGroup**. Informace o skupinách prostředků najdete v článku [Použití skupin prostředků ke správě prostředků Azure](../azure-resource-manager/resource-group-overview.md).  
   4. Vyberte **umístění** pro objekt pro vytváření dat. V rozevíracím seznamu jsou uvedené pouze oblasti podporované službou Data Factory.
   5. Vyberte položku **Připnout na Úvodní panel**.     
   6. Klikněte na možnost **Vytvořit**.
      
      > [!IMPORTANT]
      > Chcete-li vytvářet instance služby Data Factory, musíte být členem role [Přispěvatel Data Factory](../active-directory/role-based-access-built-in-roles.md#data-factory-contributor) na úrovni předplatného a skupiny prostředků.
      > 
      > Název objektu pro vytváření dat se může v budoucnu zaregistrovat jako název DNS, takže pak bude veřejně viditelný.                
      > 
      > 
3. Chcete-li zobrazit zprávy o stavu a oznámení, klikněte na ikonu zvonu na panelu nástrojů. 
   
   ![Oznámení](./media/data-factory-copy-activity-tutorial-using-azure-portal/Notifications.png) 
4. Po vytvoření se zobrazí okno **Objekt pro vytváření dat**, jak je znázorněno na obrázku.
   
   ![Domovská stránka objektu pro vytváření dat](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-data-factory-home-page.png)

## <a name="create-linked-services"></a>Vytvoření propojených služeb
Propojené služby propojují úložiště dat nebo výpočetní služby s objektem pro vytváření dat Azure. Všechny zdroje a jímky podporované aktivitou kopírování najdete v tématu [podporovaná úložiště dat](data-factory-data-movement-activities.md##supported-data-stores-and-formats). Seznam výpočetních služeb podporovaných službou Data Factory najdete v tématu [Propojené výpočetní služby](data-factory-compute-linked-services.md). V tomto kurzu žádné výpočetní služby nepoužijete. 

V tomto kroku vytvoříte dvě propojené služby: **AzureStorageLinkedService** a **AzureSqlLinkedService**. Propojená služba AzureStorageLinkedService propojuje účet Azure Storage a AzureSqlLinkedService propojuje službu Azure SQL Database s objektem **ADFTutorialDataFactory**. Později v tomto kurzu vytvoříte kanál, který kopíruje data z kontejneru objektů blob ve službě AzureStorageLinkedService do tabulky SQL ve službě AzureSqlLinkedService.

### <a name="create-a-linked-service-for-the-azure-storage-account"></a>Vytvoření propojené služby pro účet úložiště Azure
1. V okně **Objekt pro vytváření dat** klikněte na dlaždici **Vytvořit a nasadit**, abyste spustili **Editor** pro objekt pro vytváření dat.
   
   ![Dlaždice Vytvořit a nasadit](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-author-deploy-tile.png) 
2. V **editoru ** klikněte na panelu nástrojů na tlačítko **Nové datové úložiště** a z rozevírací nabídky vyberte **Úložiště Azure**. V pravém podokně by se měla zobrazit šablona JSON pro vytvoření propojené služby Azure Storage. 
   
    ![Tlačítko Nové datové úložiště v editoru](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-editor-newdatastore-button.png)    
3. Hodnoty `<accountname>` a `<accountkey>` nahraďte názvem účtu služby Azure Storage a jeho klíčem. 
   
    ![Editor – Blob Storage – JSON](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-editor-blob-storage-json.png)    
4. Na panelu nástrojů klikněte na **Nasadit**. Nyní byste měli v zobrazení stromu vidět nasazenou službu **AzureStorageLinkedService**. 
   
    ![Editor – Blob Storage – nasazení](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-editor-blob-storage-deploy.png)

> [!NOTE]
> Podrobnosti o vlastnostech JSON najdete v tématu [Přesun dat z/do Azure Blob](data-factory-azure-blob-connector.md#azure-storage-linked-service).
> 
> 

### <a name="create-a-linked-service-for-the-azure-sql-database"></a>Vytvoření propojené služby pro Azure SQL Database
1. V **editoru služby Data Factory** klikněte na panelu nástrojů na tlačítko **Nové úložiště dat** a z rozevírací nabídky vyberte **Azure SQL Database**. V pravém podokně by se měla zobrazit šablona JSON pro vytvoření propojené služby Azure SQL.
2. Hodnoty `<servername>`, `<databasename>`, `<username>@<servername>` a `<password>` nahraďte názvy serveru Azure SQL, databáze, uživatelského účtu a heslem. 
3. Službu **AzureSqlLinkedService** vytvoříte a nasadíte kliknutím na **Nasadit** na panelu nástrojů.
4. Zkontrolujte, jestli se služba **AzureSqlLinkedService** objevila v zobrazení stromu. 

> [!NOTE]
> Podrobnosti o vlastnostech JSON najdete v tématu [Přesun dat z/do Azure SQL Database](data-factory-azure-sql-connector.md#azure-sql-linked-service-properties).
> 
> 

## <a name="create-datasets"></a>Vytvoření datových sad
V předchozím kroku jste vytvořili propojené služby **AzureStorageLinkedService** a **AzureSqlLinkedService**, abyste propojili účet úložiště Azure a Azure SQL Database k objektu pro vytváření dat: **ADFTutorialDataFactory**. V tomto kroku nadefinujete dvě datové sady – **InputDataset** a **OutputDataset** – které představují vstupní a výstupní data uložená v úložištích dat, na která odkazují objekty AzureStorageLinkedService a AzureSqlLinkedService. Pro InputDataset zadáte kontejner objektů blob, který obsahuje objekt blob se zdrojovými daty, a pro OutputDataset zadáte tabulku SQL, do které se uloží výstupní data. 

### <a name="create-input-dataset"></a>Vytvoření vstupní datové sady
V tomto kroku vytvoříte datovou sadu s názvem **InputDataset**, která odkazuje na kontejner objektů blob ve službě Azure Storage reprezentovaný propojenou službou **AzureStorageLinkedService**.

1. V **editoru** služby Data Factory klikněte na **... Další**, klikněte na **Nová datová sada** a v rozevíracím seznamu klikněte na **Azure Blob Storage**. 
   
    ![Nabídka Nová datová sada](./media/data-factory-copy-activity-tutorial-using-azure-portal/new-dataset-menu.png)
2. Nahraďte kód JSON v pravém podokně následujícím fragmentem kódu JSON: 
   
        {
          "name": "InputDataset",
          "properties": {
            "structure": [
              {
                "name": "FirstName",
                "type": "String"
              },
              {
                "name": "LastName",
                "type": "String"
              }
            ],
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService",
            "typeProperties": {
              "folderPath": "adftutorial/",
              "fileName": "emp.txt",
              "format": {
                "type": "TextFormat",
                "columnDelimiter": ","
              }
            },
            "external": true,
            "availability": {
              "frequency": "Hour",
              "interval": 1
            }
          }
        }
   
     Je třeba počítat s následujícím: 
   
   * Vlastnost **type** datové sady je nastavená na **AzureBlob**.
   * Vlastnost **linkedServiceName** je nastavená na **AzureStorageLinkedService**. Tuto propojenou službu jste vytvořili v kroku 2.
   * Vlastnost **folderPath** je nastavená na kontejner **adftutorial**. Můžete také zadat název objektu blob ve složce pomocí vlastnosti **fileName**. Pokud neurčíte název objektu blob, budou za vstupní data považována data ze všech objektů blob v kontejneru.  
   * Vlastnost **type** formátu je nastavená na **TextFormat**.
   * V textovém souboru existují dvě pole, **FirstName** a **LastName**, oddělená čárkou (**columnDelimiter**).    
   * Vlastnost **availability** je nastavená na **hourly** (**frequency** je nastavená na **hour** a **interval** je nastavená na **1**). Proto služba Data Factory každou hodinu vyhledá vstupní data v kořenové složce kontejneru objektů blob (**adftutorial**), který jste zadali. 
     
     Pokud pro **vstupní** datovou sadu nezadáte vlastnost **fileName**, považují se za vstupy všechny soubory a objekty blob ze vstupní složky (**folderPath**). Pokud zadáte fileName v kódu JSON, bude se za vstup považovat jenom zadaný soubor nebo objekt blob.
     
     Pokud nezadáte **fileName** pro **výstupní tabulku**, generované soubory v **folderPath** se pojmenují podle následujícího formátu: Data.&lt;identifikátor GUID\&gt;.txt (například: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt).
     
     Pokud chcete nastavit **folderPath** a **fileName** dynamicky podle času **SliceStart**, použijte vlastnost **partitionedBy**. V následujícím příkladu folderPath používá rok, měsíc a den z vlastnosti SliceStart (čas zahájení zpracování řezu) a fileName používá hodinu z vlastnosti SliceStart. Pokud například začne být řez vytvářen v době 2016-09-20T08:00:00, vlastnost folderName je nastavená na wikidatagateway/wikisampledataout/2016/09/20 a vlastnost fileName je nastavená na 08.csv. 
     
           "folderPath": "wikidatagateway/wikisampledataout/{Year}/{Month}/{Day}",
           "fileName": "{Hour}.csv",
           "partitionedBy": 
           [
               { "name": "Year", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyy" } },
               { "name": "Month", "value": { "type": "DateTime", "date": "SliceStart", "format": "MM" } }, 
               { "name": "Day", "value": { "type": "DateTime", "date": "SliceStart", "format": "dd" } }, 
               { "name": "Hour", "value": { "type": "DateTime", "date": "SliceStart", "format": "hh" } } 
           ],
3. Datovou sadu **InputDataset** vytvoříte a nasadíte kliknutím na **Nasadit** na panelu nástrojů. Zkontrolujte, jestli se datová sada **InputDataset** objevila v zobrazení stromu.

> [!NOTE]
> Podrobnosti o vlastnostech JSON najdete v tématu [Přesun dat z/do Azure Blob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties).
> 
> 

### <a name="create-output-dataset"></a>Vytvoření výstupní datové sady
V této části kroku vytvoříte výstupní datovou sadu s názvem **OutputDataset**. Tato datová sada odkazuje na tabulku SQL ve službě Azure SQL Database, kterou reprezentuje **AzureSqlLinkedService**. 

1. V **editoru** služby Data Factory klikněte na **... Další**, klikněte na **Nová datová sada** a v rozevíracím seznamu klikněte na **Azure SQL**. 
2. Nahraďte kód JSON v pravém podokně následujícím fragmentem kódu JSON:
   
        {
          "name": "OutputDataset",
          "properties": {
            "structure": [
              {
                "name": "FirstName",
                "type": "String"
              },
              {
                "name": "LastName",
                "type": "String"
              }
            ],
            "type": "AzureSqlTable",
            "linkedServiceName": "AzureSqlLinkedService",
            "typeProperties": {
              "tableName": "emp"
            },
            "availability": {
              "frequency": "Hour",
              "interval": 1
            }
          }
        }
   
     Je třeba počítat s následujícím: 
   
   * Vlastnost **type** datové sady je nastavená na **AzureSQLTable**.
   * Vlastnost **linkedServiceName** je nastavená na **AzureSqlLinkedService** (tuto propojenou službu jste vytvořili v kroku 2).
   * Vlastnost **tablename** je nastavená na **emp**.
   * V tabulce emp v databázi jsou k dispozici tři sloupce – **ID**, **FirstName** a **LastName**. ID je sloupec identity, takže je zde třeba zadat pouze položky **FirstName** (Jméno) a **LastName** (Příjmení).
   * Vlastnost **availability** je nastavená na **hourly** (**frequency** je nastavená na **hour** a **interval** je nastavená na **1**).  Služba Data Factory bude generovat řez výstupních dat do tabulky **emp** ve službě Azure SQL Database každou hodinu.
3. Datovou sadu **OutputDataset** vytvoříte a nasadíte kliknutím na **Nasadit** na panelu nástrojů. Zkontrolujte, jestli se datová sada **OutputDataset** objevila v zobrazení stromu. 

> [!NOTE]
> Podrobnosti o vlastnostech JSON najdete v tématu [Přesun dat z/do Azure SQL Database](data-factory-azure-sql-connector.md#azure-sql-linked-service-properties).
> 
> 

## <a name="create-pipeline"></a>Vytvoření kanálu
V tomto kroku vytvoříte kanál s **aktivitou kopírování**, která používá **InputDataset** jako vstup a **OutputDataset** jako výstup.

1. V **editoru** služby Data Factory klikněte na **... Další** a poté na **Nový kanál**. Případně můžete ve stromovém zobrazení kliknout pravým tlačítkem na **anály** a pak kliknout na **Nový kanál**.
2. Nahraďte kód JSON v pravém podokně následujícím fragmentem kódu JSON: 
   
        {
          "name": "ADFTutorialPipeline",
          "properties": {
            "description": "Copy data from a blob to Azure SQL table",
            "activities": [
              {
                "name": "CopyFromBlobToSQL",
                "type": "Copy",
                "inputs": [
                  {
                    "name": "InputDataset"
                  }
                ],
                "outputs": [
                  {
                    "name": "OutputDataset"
                  }
                ],
                "typeProperties": {
                  "source": {
                    "type": "BlobSource"
                  },
                  "sink": {
                    "type": "SqlSink",
                    "writeBatchSize": 10000,
                    "writeBatchTimeout": "60:00:00"
                  }
                },
                "Policy": {
                  "concurrency": 1,
                  "executionPriorityOrder": "NewestFirst",
                  "retry": 0,
                  "timeout": "01:00:00"
                }
              }
            ],
            "start": "2016-07-12T00:00:00Z",
            "end": "2016-07-13T00:00:00Z"
          }
        } 
   
    Je třeba počítat s následujícím:
   
   * V části aktivit je jenom jedna aktivita, jejíž vlastnost **type** je nastavená na **Copy**.
   * Vstup aktivity je nastavený na **InputDataset** a výstup aktivity je nastavený na **OutputDataset**.
   * V části **typeProperties** je jako typ zdroje určen **BlobSource** a jako typ jímky **SqlSink**.
     
     Nahraďte hodnotu vlastnosti **start** aktuálním dnem a **end** následujícím dnem. Můžete zadat jenom část data a přeskočit část času. Například „2016-02-03“ je ekvivalentní hodnotě „2016-02-03T00:00:00Z“.
     
     Počáteční a koncové hodnoty data a času musí být ve [formátu ISO](http://en.wikipedia.org/wiki/ISO_8601). Například: 2016-10-14T16:32:41Z. Čas hodnoty **end** je nepovinný, ale my ho v tomto kurzu použijeme. 
     
     Pokud nezadáte hodnotu vlastnosti **end**, vypočítá se jako „**start + 48 hodin**“. Pokud chcete kanál spouštět bez omezení, zadejte vlastnosti **end** hodnotu **9999-09-09**.
     
     V předchozím příkladu je 24 datových řezů, protože se vytvářejí každou hodinu.
3. Kanál **ADFTutorialPipeline** vytvoříte a nasadíte kliknutím na **Nasadit** na panelu nástrojů. Zkontrolujte, jestli se kanál objevil v zobrazení stromu. 
4. Teď zavřete okno **Editor** kliknutím na **X**. Chcete-li zobrazit domovskou stránku **objektu pro vytváření dat** pro **ADFTutorialDataFactory**, znovu klikněte na **X**.

**Blahopřejeme!** Úspěšně jste vytvořili objekt pro vytváření dat Azure, propojené služby, tabulky a kanál a naplánovali jste kanál.   

### <a name="view-the-data-factory-in-a-diagram-view"></a>Zobrazit objektu pro vytváření dat v zobrazení diagramu
1. V okně **Objekt pro vytváření dat** klikněte na **Diagram**.
   
    ![Okno objekt pro vytváření dat – dlaždice Diagram](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-datafactoryblade-diagramtile.png)
2. Zobrazený diagram by měl vypadat přibližně jako na následujícím obrázku: 
   
    ![Zobrazení diagramu](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-diagram-blade.png)
   
    Můžete provést přiblížení, oddálení, zvětšení na 100 %, přizpůsobení zobrazení, automatické umísťování kanálů a tabulek a zobrazení informací o rodokmenu (zvýrazní nadřazené a podřízené položky vybraných položek).  Poklikáním na objekt (vstupní nebo výstupní tabulka nebo kanál) můžete zobrazit vlastnosti. 
3. Klikněte pravým tlačítkem na **ADFTutorialPipeline** v zobrazení diagramu a pak klikněte na **Otevřít kanál**. 
   
    ![Otevření kanálu](./media/data-factory-copy-activity-tutorial-using-azure-portal/DiagramView-OpenPipeline.png)
4. Měli by se zobrazit aktivity v kanálu spolu se vstupními a výstupními datovými sadami pro aktivity. V tomto kurzu máte v kanálu pouze jednu aktivitu (aktivita kopírování) se vstupní datovou sadou InputDataset a výstupní datovou sadou OutputDataset.   
   
    ![Otevřené zobrazení kanálu](./media/data-factory-copy-activity-tutorial-using-azure-portal/DiagramView-OpenedPipeline.png)
5. Pokud se chcete vrátit do zobrazení diagramu, klikněte v zobrazení cesty v levém horním rohu na **Objekt pro vytváření dat**. Zobrazení diagramu zobrazí všechny kanály. V tomto příkladu jste vytvořili jenom jeden kanál.   

## <a name="monitor-pipeline"></a>Monitorování kanálu
V tomto kroku budete pomocí webu Azure Portal monitorovat, co se děje v objektu pro vytváření dat Azure. 

### <a name="monitor-pipeline-using-diagram-view"></a>Monitorování kanálu pomocí Zobrazení diagramu
1. Kliknutím na **X** zavřete zobrazení **Diagram** a zobrazí se domovská stránka objektu pro vytváření dat služby Data Factory. Pokud jste zavřeli webový prohlížeč, proveďte následující kroky: 
   1. Přejděte na [Azure Portal](https://portal.azure.com/). 
   2. Na **Úvodním panelu** dvakrát klikněte na **ADFTutorialDataFactory** nebo klikněte na **Objekty pro vytváření dat** v nabídce vlevo a vyhledejte ADFTutorialDataFactory. 
2. V tomto okně byste měli vidět počet a názvy tabulek a kanálů, které jste vytvořili.
   
    ![Domovská stránka s názvy](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-datafactory-home-page-pipeline-tables.png)
3. Teď klikněte na dlaždici **Datové sady**.
4. V okně **Datové sady** klikněte na **InputDataset**. Tato datová sada je vstupní datovou sadou pro **ADFTutorialPipeline**.
   
    ![Datové sady s vybranou sadou InputDataset](./media/data-factory-copy-activity-tutorial-using-azure-portal/DataSetsWithInputDatasetFromBlobSelected.png)   
5. Kliknutím na **... (tři tečky)** zobrazíte všechny datové řezy.
   
    ![Všechny vstupní datové řezy](./media/data-factory-copy-activity-tutorial-using-azure-portal/all-input-slices.png)  
   
    Všimněte si, že všechny datové řezy až do aktuálního času jsou ve stavu **Připraveno**, protože soubor **emp.txt** celou dobu existuje v kontejneru objektů blob: **adftutorial\input**. Potvrďte, že se žádné řezy nezobrazují v části **Řezy, které v poslední době selhaly** dole.
   
    Oba seznamy **Řezy, které se v poslední době aktualizovaly** a **Řezy, které v poslední době selhaly** jsou řazené podle **DOBY POSLEDNÍ AKTUALIZACE**. 
   
    Řezy můžete filtrovat kliknutím na **Filtr** v panelu nástrojů.  
   
    ![Filtrování vstupních řezů](./media/data-factory-copy-activity-tutorial-using-azure-portal/filter-input-slices.png)
6. Zavřete všechna okna, dokud se nezobrazí okno **Datové sady**. Klikněte na **OutputDataset**. Tato datová sada je výstupní datovou sadou pro **ADFTutorialPipeline**.
   
    ![okno datové sady](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-datasets-blade.png)
7. Mělo by se zobrazit okno **OutputDataset**, jak je znázorněno na následujícím obrázku:
   
    ![okno tabulky](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-table-blade.png) 
8. Všimněte si, že se už vytvořily datové řezy až do aktuálního času a jsou ve stavu **Připraveno**. Žádné řezy nejsou uvedené v části **Problémové řezy** dole.
9. Klikněte na **... (tři tečky)**, abyste zobrazili všechny řezy.
   
    ![okno datové řezy](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-dataslices-blade.png)
10. Klikněte na libovolný datový řez ze seznamu. Mělo by se zobrazit okno **Datový řez**.
    
     ![okno datový řez](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-dataslice-blade.png)
    
     Není-li řez ve stavu **Připraveno**, zobrazí se v seznamu **Upstreamové datové řezy, které nejsou připraveny** upstreamové datové řezy, které nejsou připravené a které blokují spuštění aktuálního řezu.
11. V okně **DATOVÝ ŘEZ**byste měli v seznamu dole vidět všechna spuštění aktivit. Kliknutím na **spuštění aktivit** zobrazíte okno **Podrobnosti o spuštění aktivit**. 
    
    ![Podrobnosti o spuštění aktivit](./media/data-factory-copy-activity-tutorial-using-azure-portal/ActivityRunDetails.png)
12. Klikejte na tlačítko **X**, dokud nezavřete všechna okna a nevrátíte se zpátky do domovského okna pro **ADFTutorialDataFactory**.
13. (Volitelné) Klikněte na **Kanály** na domovské stránce pro **ADFTutorialDataFactory**, klikněte na **ADFTutorialPipeline** v okně **Kanály** a procházejte vstupní tabulky (**potřebováno**) nebo výstupní tabulky (**Vyprodukováno**).
14. Spusťte **SQL Server Management Studio**, připojte se ke službě Azure SQL Database a ověřte, že se řádky vložily do tabulky **emp** v databázi.
    
    ![Výsledky dotazu SQL](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-sql-query-results.png)

### <a name="monitor-pipeline-using-monitor-manage-app"></a>Monitorování kanálu pomocí aplikace pro monitorování a správu
K monitorování kanálů můžete také použít aplikaci pro monitorování a správu. Podrobnosti o použití této aplikace najdete v tématu [Monitorování a správa kanálů služby Azure Data Factory pomocí aplikace pro monitorování a správu](data-factory-monitor-manage-app.md).

1. Na domovské stránce svého objektu pro vytváření dat klikněte na dlaždici **Monitorování a správa**.
   
    ![Dlaždice Monitorování a správa](./media/data-factory-copy-activity-tutorial-using-azure-portal/monitor-manage-tile.png) 
2. Měla by se zobrazit **aplikace pro monitorování a správu**. Změňte hodnoty **Čas spuštění** a **Čas ukončení** tak, aby zahrnovaly časy spuštění (2016-07-12) a ukončení (2016-07-13) vašeho kanálu, a klikněte na **Použít**. 
   
    ![Aplikace pro monitorování a správu](./media/data-factory-copy-activity-tutorial-using-azure-portal/monitor-and-manage-app.png) 
3. Výběrem okna aktivity v seznamu **Okna aktivit** zobrazíte podrobnosti. 
    ![Podrobnosti o okně aktivity](./media/data-factory-copy-activity-tutorial-using-azure-portal/activity-window-details.png)

## <a name="summary"></a>Souhrn
V tomto kurzu jste vytvořili objekt pro vytváření dat Azure pro zkopírování dat z objektu blob Azure do Azure SQL Database. Použili jste web Azure Portal k vytvoření objektu pro vytváření dat, propojených služeb, datových sad a kanálu. Zde jsou základní kroky, které jste v tomto kurzu provedli:  

1. Vytvořili jste **objekt pro vytváření dat** Azure.
2. Vytvořili jste **propojené služby**:
   1. Propojená služba **Azure Storage** připojující účet úložiště Azure, který obsahuje vstupní data.     
   2. Propojená služba **Azure SQL** připojující službu Azure SQL Database, která obsahuje výstupní data. 
3. Vytvořili jste **datové sady**, které popisují vstupní data a výstupní data pro kanály.
4. Vytvořili jste **kanál** s **aktivitou kopírování**, která má jako zdroj **BlobSource** a jako jímku **SqlSink**.  

## <a name="see-also"></a>Viz také
| Téma | Popis |
|:--- |:--- |
| [Aktivity přesunu dat](data-factory-data-movement-activities.md) |Tento článek obsahuje podrobné informace o aktivitě kopírování, kterou jste v tomto kurzu použili. |
| [Plánování a provádění](data-factory-scheduling-and-execution.md) |Tento článek vysvětluje aspekty plánování a provádění aplikačního modelu služby Azure Data Factory. |
| [Kanály](data-factory-create-pipelines.md) |Tento článek vám pomůže pochopit kanály a aktivity ve službě Azure Data Factory. |
| [Datové sady](data-factory-create-datasets.md) |Tento článek vám pomůže pochopit datové sady ve službě Azure Data Factory. |
| [Monitorování a správa kanálů pomocí monitorovací aplikace](data-factory-monitor-manage-app.md) |Tento článek popisuje, jak monitorovat, spravovat a ladit kanály pomocí aplikace pro monitorování a správu. |




<!--HONumber=Nov16_HO2-->


