---
title: 'Kurz: Vytvoření kanálu s aktivitou kopírování pomocí sady Visual Studio | Microsoft Docs'
description: V tomto kurzu vytvoříte kanál služby Azure Data Factory s aktivitou kopírování pomocí sady Visual Studio.
services: data-factory
documentationcenter: ''
author: spelluru
manager: jhubbard
editor: monicar

ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/01/2016
ms.author: spelluru

---
# Kurz: Vytvoření kanálu s aktivitou kopírování pomocí sady Visual Studio
> [!div class="op_single_selector"]
> * [Přehled a požadavky](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
> * [Průvodce kopírováním](data-factory-copy-data-wizard-tutorial.md)
> * [portál Azure](data-factory-copy-activity-tutorial-using-azure-portal.md)
> * [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md)
> * [PowerShell](data-factory-copy-activity-tutorial-using-powershell.md)
> * [Šablona Azure Resource Manageru](data-factory-copy-activity-tutorial-using-azure-resource-manager-template.md)
> * [REST API](data-factory-copy-activity-tutorial-using-rest-api.md)
> * [.NET API](data-factory-copy-activity-tutorial-using-dotnet-api.md)
> 
> 

V tomto kurzu je uvedeno, jak vytvořit a monitorovat objekt pro vytváření dat Azure pomocí sady Visual Studio. Kanál v objektu pro vytváření dat využívá aktivitu kopírování, s jejíž pomocí kopíruje data ze služby Azure Blob Storage do služby Azure SQL Database.

Zde jsou kroky, které provedete v rámci tohoto kurzu:

1. Vytvoříte dvě propojené služby: **AzureStorageLinkedService1** a **AzureSqlLinkedService1**. 
   
    AzureStorageLinkedService1 propojuje službu Azure Storage a AzureSqlLinkedService1 propojuje službu Azure SQL Database k objektu pro vytváření dat **ADFTutorialDataFactoryVS**. Vstupní data pro kanál se nachází v kontejneru objektů blob v Azure Blob Storage a výstupní data jsou uložena v tabulce v Azure SQL Database. Proto přidáváte tyto dvě úložiště dat jako propojené služby objektu pro vytváření dat.
2. Vytvoříte dvě datové sady: **InputDataset** a **OutputDataset**, které představují vstupní a výstupní data uložená v úložištích dat. 
   
    Pro InputDataset zadáte kontejner objektů blob, který obsahuje objekt blob se zdrojovými daty. Pro OutputDataset zadáte tabulku SQL, do které se uloží výstupní data. Zadáte také další vlastnosti, jako jsou například struktura, dostupnost a zásady.
3. Vytvoříte v objektu ADFTutorialDataFactoryVS kanál s názvem **ADFTutorialPipeline**. 
   
    Kanál má **aktivitu kopírování**, která kopíruje vstupní data z objektu blob Azure do výstupní tabulky Azure SQL. Aktivita kopírování provádí přesun dat ve službě Azure Data Factory. Aktivita používá globálně dostupnou službu, která může kopírovat data mezi různými úložišti dat zabezpečeným, spolehlivým a škálovatelným způsobem. Podrobnosti o aktivitě kopírování najdete v článku [Aktivity přesunu dat](data-factory-data-movement-activities.md). 
4. Vytvořte objekt pro vytváření dat s názvem **VSTutorialFactory**. Nasaďte objekt pro vytváření dat a všechny entity služby Data Factory (propojené služby, tabulky a kanál).    

## Požadavky
1. Přečtěte si článek [Přehled kurzu](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) a proveďte **nutné** kroky. 
2. K publikování entit Data Factory do Azure Data Factory musíte být **správce předplatného Azure**.  
3. Na počítači musíte mít nainstalované tyto položky: 
   * Visual Studio 2013 nebo Visual Studio 2015.
   * Stáhněte si sadu Azure SDK pro Visual Studio 2013 nebo Visual Studio 2015. Přejděte na [stránku položek ke stažení pro Azure](https://azure.microsoft.com/downloads/) a klikněte na **VS 2013** nebo **VS 2015** v části **.NET**.
   * Stáhněte si nejnovější modul plug-in Azure Data Factory pro Visual Studio: [VS 2013](https://visualstudiogallery.msdn.microsoft.com/754d998c-8f92-4aa7-835b-e89c8c954aa5) nebo [VS 2015](https://visualstudiogallery.msdn.microsoft.com/371a4cf9-0093-40fa-b7dd-be3c74f49005). Modul plug-in můžete taky aktualizovat, a to pomocí tohoto postupu: V nabídce klikněte na **Nástroje** -> **Rozšíření a aktualizace** -> **Online** -> **Galerie sady Visual Studio** -> **Microsoft Azure Data Factory Tools for Visual Studio** (Nástroje Microsoft Azure Data Factory pro Visual Studio) -> **Aktualizovat**.

## Vytvoření projektu v sadě Visual Studio
1. Spusťte **Visual Studio 2013**. Klikněte na **Soubor**, přejděte na **Nový** a klikněte na **Projekt**. Mělo by se zobrazit dialogové okno **Nový projekt**.  
2. V dialogovém okně **Nový projekt** vyberte šablonu **DataFactory** a klikněte na **Empty Data Factory Project** (Prázdný projekt Data Factory). Pokud nevidíte šablonu DataFactory, zavřete Visual Studio, nainstalujte sadu Azure SDK pro Visual Studio 2013 a znovu otevřete Visual Studio.  
   
    ![Dialogové okno Nový projekt](./media/data-factory-copy-activity-tutorial-using-visual-studio/new-project-dialog.png)
3. Zadejte **název** projektu, **umístění** a název **řešení** a klikněte na **OK**.
   
    ![Průzkumník řešení](./media/data-factory-copy-activity-tutorial-using-visual-studio/solution-explorer.png) 

## Vytvoření propojených služeb
Propojené služby propojují úložiště dat nebo výpočetní služby s objektem pro vytváření dat Azure. Všechny zdroje a jímky podporované aktivitou kopírování najdete v tématu [podporovaná úložiště dat](data-factory-data-movement-activities.md##supported-data-stores-and-formats). Seznam výpočetních služeb podporovaných službou Data Factory najdete v tématu [Propojené výpočetní služby](data-factory-compute-linked-services.md). V tomto kurzu žádné výpočetní služby nepoužijete. 

V tomto kroku vytvoříte dvě propojené služby: **AzureStorageLinkedService1** a **AzureSqlLinkedService1**. Propojená služba AzureStorageLinkedService1 propojuje účet úložiště Azure a AzureSqlLinkedService1 propojuje službu Azure SQL Database k objektu pro vytváření dat **ADFTutorialDataFactory**. 

### Vytvoření propojené služby Azure Storage
1. V Průzkumníku řešení klikněte pravým tlačítkem myši na **Propojené služby**, přejděte na **Přidat** a klikněte na **Nová položka**.      
2. V dialogovém okně **Přidat novou položku** vyberte v seznamu možnost **Azure Storage Linked Service** (Propojená služba Azure Storage) a klikněte na **Přidat**. 
   
    ![Nová propojená služba](./media/data-factory-copy-activity-tutorial-using-visual-studio/new-linked-service-dialog.png)
3. Hodnoty `<accountname>` a `<accountkey>`* nahraďte názvem účtu služby Azure Storage a jeho klíčem. 
   
    ![Propojená služba Azure Storage](./media/data-factory-copy-activity-tutorial-using-visual-studio/azure-storage-linked-service.png)
4. Uložte soubor **AzureStorageLinkedService1.json**.

> Podrobnosti o vlastnostech JSON najdete v tématu [Přesun dat z/do Azure Blob](data-factory-azure-blob-connector.md#azure-storage-linked-service).
> 
> 

### Vytvoření propojené služby Azure SQL
1. V **Průzkumníku řešení** znovu klikněte pravým tlačítkem myši na uzel **Propojené služby**, přejděte na **Přidat** a klikněte na **Nová položka**. 
2. Tentokrát vyberte **Propojená služba Azure SQL** a klikněte na **Přidat**. 
3. V souboru **AzureSqlLinkedService1.json** nahraďte hodnoty `<servername>`, `<databasename>`, `<username@servername>` a `<password>` názvy svého serveru Azure SQL, databáze, uživatelského účtu a heslem.    
4. Uložte soubor **AzureSqlLinkedService1.json**. 

> [!NOTE]
> Podrobnosti o vlastnostech JSON najdete v tématu [Přesun dat z/do Azure SQL Database](data-factory-azure-sql-connector.md#azure-sql-linked-service-properties).
> 
> 

## Vytvoření datových sad
V předchozím kroku jste vytvořili propojené služby **AzureStorageLinkedService1** a **AzureSqlLinkedService1**, abyste propojili účet úložiště Azure a Azure SQL Database k objektu pro vytváření dat: **ADFTutorialDataFactory**. V tomto kroku nadefinujete dvě datové sady – **InputDataset** a **OutputDataset** – které představují vstupní a výstupní data uložená v úložištích dat, na která odkazují objekty AzureStorageLinkedService1 a AzureSqlLinkedService1. Pro InputDataset zadáte kontejner objektů blob, který obsahuje objekt blob se zdrojovými daty. Pro OutputDataset zadáte tabulku SQL, do které se uloží výstupní data.

### Vytvoření vstupní datové sady
V tomto kroku vytvoříte datovou sadu s názvem **InputDataset**, která odkazuje na kontejner objektů blob ve službě Azure Storage reprezentovaný propojenou službou **AzureStorageLinkedService1**. Tabulka je obdélníková datová sada a je jediným typem datové sady, který je nyní podporovaný. 

1. V **Průzkumníku řešení** klikněte pravým tlačítkem myši na **Tabulky**, přejděte na **Přidat** a klikněte na **Nová položka**.
2. V dialogovém okně **Přidat novou položku** vyberte v seznamu možnost **Azure Blob** a klikněte na **Přidat**.   
3. Nahraďte text JSON následujícím textem a uložte soubor **AzureBlobLocation1.json**. 
   
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
           "linkedServiceName": "AzureStorageLinkedService1",
           "typeProperties": {
             "folderPath": "adftutorial/",
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

> [!NOTE]
> Podrobnosti o vlastnostech JSON najdete v tématu [Přesun dat z/do Azure Blob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties).
> 
> 

### Vytvoření výstupní datové sady
V tomto kroku vytvoříte výstupní datovou sadu s názvem **OutputDataset**. Tato datová sada odkazuje na tabulku SQL ve službě Azure SQL Database, kterou reprezentuje **AzureSqlLinkedService1**. 

1. V **Průzkumníku řešení** klikněte opět pravým tlačítkem myši na **Tabulky**, přejděte na **Přidat** a klikněte na **Nová položka**.
2. V dialogovém okně **Přidat novou položku** vyberte **Azure SQL** a klikněte na **Přidat**. 
3. Nahraďte text JSON následujícím textem JSON a uložte soubor **AzureSqlTableLocation1.json**.
   
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
           "linkedServiceName": "AzureSqlLinkedService1",
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

> [!NOTE]
> Podrobnosti o vlastnostech JSON najdete v tématu [Přesun dat z/do Azure SQL Database](data-factory-azure-sql-connector.md#azure-sql-linked-service-properties).
> 
> 

## Vytvoření kanálu
Dosud jste vytvořili vstupní a výstupní propojené služby a tabulky. Teď vytvoříte kanál s **aktivitou kopírování** ke kopírování dat z objektu blob Azure do Azure SQL Database. 

1. V **Průzkumníku řešení** klikněte pravým tlačítkem myši na **Kanály**, přejděte na **Přidat** a klikněte na **Nová položka**.  
2. V dialogovém okně **Přidat novou položku** vyberte **Copy Data Pipeline** (Kanál kopírování dat) a klikněte na **Přidat**. 
3. Nahraďte text JSON následujícím textem JSON a uložte soubor **CopyActivity1.json**.
   
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
                 "style": "StartOfInterval",
                 "retry": 0,
                 "timeout": "01:00:00"
               }
             }
           ],
           "start": "2015-07-12T00:00:00Z",
           "end": "2015-07-13T00:00:00Z",
           "isPaused": false
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

## Publikování/nasazení entit služby Data Factory
V tomto kroku publikujete entity služby Data Factory (propojené služby, datové sady a kanál), které jste vytvořili dříve. Také zadáte název nově vytvořeného objektu pro vytváření dat, do kterého se tyto entity načtou.  

1. V Průzkumníku řešení klikněte pravým tlačítkem na požadovaný projekt a poté klikněte na **Publikovat**. 
2. Pokud se zobrazí dialogové okno **Přihlásit se pomocí účtu Microsoft**, zadejte přihlašovací údaje k účtu s předplatným Azure a klikněte na **Přihlásit**.
3. Mělo by se zobrazit následující dialogové okno:
   
   ![Dialogové okno publikování](./media/data-factory-copy-activity-tutorial-using-visual-studio/publish.png)
4. Na stránce Konfigurace objektu pro vytváření dat proveďte následující kroky: 
   
   1. Vyberte možnost **Create New Data Factory** (Vytvořit nový objekt pro vytváření dat).
   2. Zadejte **VSTutorialFactory** jako **Název**.  
      
      > [!IMPORTANT]
      > Název objektu pro vytváření dat Azure musí být globálně jedinečný. Pokud se při publikování zobrazí chyba týkající se názvu objektu pro vytváření dat, změňte název objektu pro vytváření dat (třeba na váš_název_VSTutorialFactory) a zkuste publikování znovu. V tématu [Objekty pro vytváření dat – pravidla pojmenování](data-factory-naming-rules.md) najdete pravidla pojmenování artefaktů služby Data Factory.     
      > 
      > 
   3. V poli **Předplatné** vyberte své předplatné Azure.
      
      > [!IMPORTANT]
      > Pokud nevidíte žádné předplatné, ujistěte se, že jste přihlášeni pomocí účtu, který je správce nebo spolusprávce předplatného.  
      > 
      > 
   4. Vyberte **skupinu prostředků** pro objekt pro vytváření dat, který se má vytvořit. 5. Vyberte **oblast** pro objekt pro vytváření dat. V rozevíracím seznamu jsou uvedené pouze oblasti podporované službou Data Factory.
5. Kliknutím na **Další** přejděte na stránku **Publish Items** (Publikovat položky).
   
        ![Stránka konfigurace služby Data Factory](media/data-factory-copy-activity-tutorial-using-visual-studio/configure-data-factory-page.png)   
6. Na stránce **Publish Items** (Publikovat položky) zkontrolujte, jestli jsou vybrané všechny entity služby Data Factory, a kliknutím na **Další** přejděte na stránku **Souhrn**.
   
   ![Stránka publikování položek](media/data-factory-copy-activity-tutorial-using-visual-studio/publish-items-page.png)     
7. Zkontrolujte souhrn a klikněte na **Další**. Spustí se proces nasazení a zobrazí se **Stav nasazení**.
   
   ![Stránka souhrnu publikování](media/data-factory-copy-activity-tutorial-using-visual-studio/publish-summary-page.png)
8. Na stránce **Stav nasazení** byste měli vidět stav procesu nasazení. Až se nasazení dokončí, klikněte na Dokončit. 
   ![Stránka stavu nasazení](media/data-factory-copy-activity-tutorial-using-visual-studio/deployment-status.png) Je třeba počítat s následujícím: 

* Pokud se zobrazí chyba „**Pro předplatné není zaregistrované používání oboru názvů Microsoft.DataFactory**“, proveďte některý z těchto kroků a znovu zkuste název publikovat: 
  
  * V prostředí Azure PowerShell zaregistrujte zprostředkovatele služby Data Factory pomocí následujícího příkazu. 
    
          Register-AzureRmResourceProvider -ProviderNamespace Microsoft.DataFactory
    
      Spuštěním následujícího příkazu si můžete ověřit, jestli je zprostředkovatel služby Data Factory zaregistrovaný. 
    
          Get-AzureRmResourceProvider
  * Přihlaste se na web [Azure Portal ](https://portal.azure.com) pomocí předplatného Azure a přejděte do okna Objekt pro vytváření dat nebo na webu Azure Portal vytvořte objekt pro vytváření dat. Zprostředkovatel se při takovém postupu zaregistruje automaticky.
* Název objektu pro vytváření dat se může v budoucnu zaregistrovat jako název DNS, takže pak bude veřejně viditelný.

> [!IMPORTANT]
> Chcete-li vytvářet instance služby Data Factory, musíte být správce nebo spolusprávce předplatného Azure.
> 
> 

## Souhrn
V tomto kurzu jste vytvořili objekt pro vytváření dat Azure pro zkopírování dat z objektu blob Azure do Azure SQL Database. Visual Studio jste použili k vytvoření objektu pro vytváření dat, propojených služeb, datových sad a kanálu. Zde jsou základní kroky, které jste v tomto kurzu provedli:  

1. Vytvořili jste **objekt pro vytváření dat** Azure.
2. Vytvořili jste **propojené služby**:
   1. Propojená služba **Azure Storage** připojující účet úložiště Azure, který obsahuje vstupní data.    
   2. Propojená služba **Azure SQL** připojující službu Azure SQL Database, která obsahuje výstupní data. 
3. Vytvořili jste **datové sady**, které popisují vstupní data a výstupní data pro kanály.
4. Vytvořili jste **kanál** s **aktivitou kopírování**, která má jako zdroj **BlobSource** a jako jímku **SqlSink**. 

## Zobrazení objektů pro vytváření dat pomocí Průzkumníka serveru
1. V sadě **Visual Studio** klikněte v nabídce na **Zobrazit** a potom klikněte na **Průzkumník serveru**.
2. V okně Průzkumníka serveru rozbalte položku **Azure** a potom **Data Factory**. Pokud se zobrazí text **Přihlásit se k Visual Studiu**, zadejte **účet** přidružený k vašemu předplatnému Azure a klikněte na **Pokračovat**. Zadejte **heslo** a klikněte na **Přihlásit**. Visual Studio se pokusí získat informace o všech objektech pro vytváření dat Azure v rámci vašeho předplatného. Stav této operace se zobrazuje v okně **Data Factory Task List** (Seznam úkolů služby Data Factory).
    ![Průzkumník serveru](./media/data-factory-copy-activity-tutorial-using-visual-studio/server-explorer.png)
3. Můžete na některý objekt pro vytváření dat kliknout pravým tlačítkem a výběrem možnosti Export Data Factory to New Project (Exportovat objekt pro vytváření dat do nového projektu) vytvořit projekt sady Visual Studio založený na existujícím objektu pro vytváření dat.
    ![Export objektu pro vytváření dat do projektu VS](./media/data-factory-copy-activity-tutorial-using-visual-studio/export-data-factory-menu.png)  

## Aktualizace nástrojů služby Data Factory pro Visual Studio
Chcete-li aktualizovat nástroje služby Azure Data Factory pro Visual Studio, proveďte následující kroky:

1. V nabídce klikněte na **Nástroje** a vyberte **Rozšíření a aktualizace**. 
2. V levém podokně vyberte **Aktualizace** a vyberte **Galerie sady Visual Studio**.
3. Vyberte možnost **Azure Data Factory tools for Visual Studio** (Nástroje služby Azure Data Factory pro Visual Studio) a klikněte na **Aktualizovat**. Pokud se tato položka nezobrazí, znamená to, že máte nejnovější verzi těchto nástrojů. 

Pokyny k monitorování kanálu a datových sad, které jste vytvořili v tomto kurzu, pomocí webu Azure Portal najdete v článku [Monitorování datových sad a kanálu](data-factory-copy-activity-tutorial-using-azure-portal.md#monitor-pipeline).

## Viz také
| Téma | Popis |
|:--- |:--- |
| [Aktivity přesunu dat](data-factory-data-movement-activities.md) |Tento článek obsahuje podrobné informace o aktivitě kopírování, kterou jste v tomto kurzu použili. |
| [Plánování a provádění](data-factory-scheduling-and-execution.md) |Tento článek vysvětluje aspekty plánování a provádění aplikačního modelu služby Azure Data Factory. |
| [Kanály](data-factory-create-pipelines.md) |Tento článek vám pomůže pochopit kanály a aktivity ve službě Azure Data Factory. |
| [Datové sady](data-factory-create-datasets.md) |Tento článek vám pomůže pochopit datové sady ve službě Azure Data Factory. |
| [Monitorování a správa kanálů pomocí monitorovací aplikace](data-factory-monitor-manage-app.md) |Tento článek popisuje, jak monitorovat, spravovat a ladit kanály pomocí aplikace pro monitorování a správu. |

<!--HONumber=Oct16_HO3-->


