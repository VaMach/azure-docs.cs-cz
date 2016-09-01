<properties 
    pageTitle="Kurz: Vytvoření kanálu s aktivitou kopírování pomocí sady Visual Studio" 
    description="V tomto kurzu vytvoříte kanál služby Azure Data Factory s aktivitou kopírování pomocí sady Visual Studio." 
    services="data-factory" 
    documentationCenter="" 
    authors="spelluru" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="get-started-article" 
    ms.date="08/01/2016" 
    ms.author="spelluru"/>

# Kurz: Vytvoření kanálu s aktivitou kopírování pomocí sady Visual Studio
> [AZURE.SELECTOR]
- [Přehled kurzu](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
- [Pomocí editoru služby Data Factory](data-factory-copy-activity-tutorial-using-azure-portal.md)
- [Pomocí prostředí PowerShell](data-factory-copy-activity-tutorial-using-powershell.md)
- [Pomocí sady Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md)
- [Pomocí rozhraní REST API](data-factory-copy-activity-tutorial-using-rest-api.md)
- [Pomocí průvodce kopírováním](data-factory-copy-data-wizard-tutorial.md)

V tomto kurzu provedete pomocí sady Visual Studio 2013 následující:

1. Vytvoříte dvě propojené služby: **AzureStorageLinkedService1** a **AzureSqlLinkedService1**. AzureStorageLinkedService1 propojuje službu Azure Storage a AzureSqlLinkedService1 propojuje službu Azure SQL Database k objektu pro vytváření dat **ADFTutorialDataFactoryVS**. Vstupní data pro kanál se nachází v kontejneru objektů blob v Azure Blob Storage a výstupní data jsou uložena v tabulce v Azure SQL Database. Proto přidáváte tyto dvě úložiště dat jako propojené služby objektu pro vytváření dat.
2. Vytvoříte dvě tabulky objektu pro vytváření dat, **EmpTableFromBlob** a **EmpSQLTable**, které představují vstupní a výstupní data, která jsou uložená v úložištích dat. Pro EmpTableFromBlob zadáte kontejner objektů blob, který obsahuje objekt blob se zdrojovými daty, a pro EmpSQLTable zadáte tabulku SQL, do které se uloží výstupní data. Zadejte také další vlastnosti, jako je například struktura dat, dostupnost dat a další.
3. Vytvoříte v objektu ADFTutorialDataFactoryVS kanál s názvem **ADFTutorialPipeline**. Kanál má **aktivitu kopírování**, která kopíruje vstupní data z objektu blob Azure do výstupní tabulky Azure SQL. Aktivita kopírování provádí přesun dat ve službě Azure Data Factory a aktivita používá globálně dostupnou službu, která může kopírovat data mezi různými úložišti dat zabezpečeným, spolehlivým a škálovatelným způsobem. Podrobnosti o aktivitě kopírování najdete v článku [Aktivity přesunu dat](data-factory-data-movement-activities.md). 
4. Vytvoříte objekt pro vytváření dat a nasadíte propojené služby, tabulky a kanál.    

## Požadavky

1. Než budete pokračovat, **musíte** si přečíst článek [Přehled kurzu](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) a provést nutné kroky.
2. K publikování entit Data Factory do Azure Data Factory musíte být **správce předplatného Azure**. To je momentální omezení.  
3. Na počítači musíte mít nainstalované tyto položky: 
    - Visual Studio 2013 nebo Visual Studio 2015.
    - Stáhněte si sadu Azure SDK pro Visual Studio 2013 nebo Visual Studio 2015. Přejděte na [stránku položek ke stažení pro Azure](https://azure.microsoft.com/downloads/) a klikněte na **VS 2013** nebo **VS 2015** v části **.NET**.
    - Stáhněte si nejnovější modul plug-in Azure Data Factory pro Visual Studio: [VS 2013](https://visualstudiogallery.msdn.microsoft.com/754d998c-8f92-4aa7-835b-e89c8c954aa5) nebo [VS 2015](https://visualstudiogallery.msdn.microsoft.com/371a4cf9-0093-40fa-b7dd-be3c74f49005). Pokud používáte sadu Visual Studio 2013, můžete modul plug-in taky aktualizovat, a to pomocí tohoto postupu: V nabídce klikněte na **Nástroje** -> **Rozšíření a aktualizace** -> **Online** -> **Galerie sady Visual Studio** -> **Microsoft Azure Data Factory Tools for Visual Studio** (Nástroje Microsoft Azure Data Factory pro Visual Studio) -> **Aktualizovat**. 
 


## Vytvoření projektu v sadě Visual Studio 
1. Spusťte **Visual Studio 2013**. Klikněte na **Soubor**, přejděte na **Nový** a klikněte na **Projekt**. Mělo by se zobrazit dialogové okno **Nový projekt**.  
2. V dialogovém okně **Nový projekt** vyberte šablonu **DataFactory** a klikněte na **Empty Data Factory Project** (Prázdný projekt Data Factory). Pokud nevidíte šablonu DataFactory, zavřete Visual Studio, nainstalujte sadu Azure SDK pro Visual Studio 2013 a znovu otevřete Visual Studio.  

    ![Dialogové okno Nový projekt](./media/data-factory-copy-activity-tutorial-using-visual-studio/new-project-dialog.png)

3. Zadejte **název** projektu, **umístění** a název **řešení** a klikněte na **OK**.

    ![Průzkumník řešení](./media/data-factory-copy-activity-tutorial-using-visual-studio/solution-explorer.png) 

## Vytvoření propojených služeb
Propojené služby propojují úložiště dat nebo výpočetní služby s objektem pro vytváření dat Azure. Úložištěm dat může být Azure Storage, Azure SQL Database nebo místní databáze SQL Serveru.

V tomto kroku vytvoříte dvě propojené služby: **AzureStorageLinkedService1** a **AzureSqlLinkedService1**. Propojená služba AzureStorageLinkedService1 propojuje účet úložiště Azure a AzureSqlLinkedService1 propojuje službu Azure SQL Database k objektu pro vytváření dat **ADFTutorialDataFactory**. 

### Vytvoření propojené služby Azure Storage

4. V Průzkumníku řešení klikněte pravým tlačítkem myši na **Propojené služby**, přejděte na **Přidat** a klikněte na **Nová položka**.      
5. V dialogovém okně **Přidat novou položku** vyberte v seznamu možnost **Azure Storage Linked Service** (Propojená služba Azure Storage) a klikněte na **Přidat**. 

    ![Nová propojená služba](./media/data-factory-copy-activity-tutorial-using-visual-studio/new-linked-service-dialog.png)
 
3. Položky **accountname** a **accountkey** nahraďte názvem účtu úložiště Azure a jeho klíčem. 

    ![Propojená služba Azure Storage](./media/data-factory-copy-activity-tutorial-using-visual-studio/azure-storage-linked-service.png)

4. Uložte soubor **AzureStorageLinkedService1.json**.

### Vytvoření propojené služby Azure SQL

5. V **Průzkumníku řešení** znovu klikněte pravým tlačítkem myši na uzel **Propojené služby**, přejděte na **Přidat** a klikněte na **Nová položka**. 
6. Tentokrát vyberte **Propojená služba Azure SQL** a klikněte na **Přidat**. 
7. V souboru **AzureSqlLinkedService1.json** nahraďte položky **servername**, **databasename**, **username@servername** a **password** názvem serveru SQL Azure, názvem databáze, uživatelským účtem a heslem.    
8.  Uložte soubor **AzureSqlLinkedService1.json**. 


## Vytvoření datových sad
V předchozím kroku jste vytvořili propojené služby **AzureStorageLinkedService1** a **AzureSqlLinkedService1**, abyste propojili účet úložiště Azure a Azure SQL Database k objektu pro vytváření dat: **ADFTutorialDataFactory**. V tomto kroku nadefinujete dvě tabulky objektu pro vytváření dat, **EmpTableFromBlob** a **EmpSQLTable**, které představují vstupní a výstupní data, která jsou uložená v úložištích dat, na která odkazují objekty AzureStorageLinkedService1 a AzureSqlLinkedService1. Pro EmpTableFromBlob zadáte kontejner objektů blob, který obsahuje objekt blob se zdrojovými daty, a pro EmpSQLTable zadáte tabulku SQL, do které se uloží výstupní data.

### Vytvoření vstupní datové sady

9. V **Průzkumníku řešení** klikněte pravým tlačítkem myši na **Tabulky**, přejděte na **Přidat** a klikněte na **Nová položka**.
10. V dialogovém okně **Přidat novou položku** vyberte v seznamu možnost **Azure Blob** a klikněte na **Přidat**.   
10. Nahraďte text JSON následujícím textem a uložte soubor **AzureBlobLocation1.json**. 

        {
          "name": "EmpTableFromBlob",
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

### Vytvoření výstupní datové sady

11. V **Průzkumníku řešení** klikněte opět pravým tlačítkem myši na **Tabulky**, přejděte na **Přidat** a klikněte na **Nová položka**.
12. V dialogovém okně **Přidat novou položku** vyberte **Azure SQL** a klikněte na **Přidat**. 
13. Nahraďte text JSON následujícím textem JSON a uložte soubor **AzureSqlTableLocation1.json**.

        {
          "name": "EmpSQLTable",
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

## Vytvoření kanálu 
Dosud jste vytvořili vstupní a výstupní propojené služby a tabulky. Teď vytvoříte kanál s **aktivitou kopírování** ke kopírování dat z objektu blob Azure do Azure SQL Database. 


1. V **Průzkumníku řešení** klikněte pravým tlačítkem myši na **Kanály**, přejděte na **Přidat** a klikněte na **Nová položka**.  
15. V dialogovém okně **Přidat novou položku** vyberte **Copy Data Pipeline** (Kanál kopírování dat) a klikněte na **Přidat**. 
16. Nahraďte text JSON následujícím textem JSON a uložte soubor **CopyActivity1.json**.
            
        {
          "name": "ADFTutorialPipeline",
          "properties": {
            "description": "Copy data from a blob to Azure SQL table",
            "activities": [
              {
                "name": "CopyFromBlobToSQL",
                "description": "Push Regional Effectiveness Campaign data to Azure SQL database",
                "type": "Copy",
                "inputs": [
                  {
                    "name": "EmpTableFromBlob"
                  }
                ],
                "outputs": [
                  {
                    "name": "EmpSQLTable"
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

## Publikování/nasazení entit služby Data Factory
  
18. V Průzkumníku řešení klikněte pravým tlačítkem na požadovaný projekt a poté klikněte na **Publikovat**. 
19. Pokud se zobrazí dialogové okno **Přihlásit se pomocí účtu Microsoft**, zadejte přihlašovací údaje k účtu s předplatným Azure a klikněte na **Přihlásit**.
20. Mělo by se zobrazit následující dialogové okno:

    ![Dialogové okno publikování](./media/data-factory-copy-activity-tutorial-using-visual-studio/publish.png)

21. Na stránce konfigurace objektu pro vytváření dat postupujte takto: 
    1. Vyberte možnost **Create New Data Factory** (Vytvořit nový objekt pro vytváření dat).
    2. Zadejte **VSTutorialFactory** jako **Název**.  
    
        > [AZURE.NOTE]  
        > Název objektu pro vytváření dat Azure musí být globálně jedinečný. Pokud se při publikování zobrazí chyba týkající se názvu objektu pro vytváření dat, změňte název objektu pro vytváření dat (třeba na váš_název_VSTutorialFactory) a zkuste publikování znovu. V tématu [Objekty pro vytváření dat – pravidla pojmenování](data-factory-naming-rules.md) najdete pravidla pojmenování artefaktů služby Data Factory.
        
    3. V poli **Předplatné** vyberte správné předplatné. 
    4. Vyberte **skupinu prostředků** pro objekt pro vytváření dat, který se má vytvořit. 
    5. Vyberte **oblast** pro objekt pro vytváření dat. 
    6. Kliknutím na **Další** přejděte na stránku **Publish Items** (Publikovat položky). 
23. Na stránce **Publish Items** (Publikovat položky) zkontrolujte, jestli jsou vybrané všechny entity služby Data Factory, a kliknutím na **Další** přejděte na stránku **Souhrn**.     
24. Zkontrolujte souhrn a klikněte na **Další**. Spustí se proces nasazení a zobrazí se **Stav nasazení**.
25. Na stránce **Stav nasazení** byste měli vidět stav procesu nasazení. Až se nasazení dokončí, klikněte na Dokončit. 

Je třeba počítat s následujícím: 

- Pokud se zobrazí chyba „**Pro předplatné není zaregistrované používání oboru názvů Microsoft.DataFactory**“, proveďte některý z těchto kroků a znovu zkuste název publikovat: 

    - V prostředí Azure PowerShell zaregistrujte zprostředkovatele služby Data Factory pomocí následujícího příkazu. 
        
            Register-AzureRmResourceProvider -ProviderNamespace Microsoft.DataFactory
    
        Spuštěním následujícího příkazu si můžete ověřit, jestli je zprostředkovatel služby Data Factory zaregistrovaný. 
    
            Get-AzureRmResourceProvider
    - Přihlaste se na web [Azure Portal ](https://portal.azure.com) pomocí předplatného Azure a přejděte do okna Objekt pro vytváření dat nebo na webu Azure Portal vytvořte objekt pro vytváření dat. Zprostředkovatel se při takovém postupu zaregistruje automaticky.
-   Název objektu pro vytváření dat se může v budoucnu zaregistrovat jako název DNS, takže pak bude veřejně viditelný.
-   Instance služby Data Factory můžete vytvářet jenom tehdy, když jste přispěvatelem/správcem předplatného Azure.

## Souhrn
V tomto kurzu jste vytvořili objekt pro vytváření dat Azure pro zkopírování dat z objektu blob Azure do Azure SQL Database. Visual Studio jste použili k vytvoření objektu pro vytváření dat, propojených služeb, datových sad a kanálu. Zde jsou základní kroky, které jste v tomto kurzu provedli:  

1.  Vytvořili jste **objekt pro vytváření dat** Azure.
2.  Vytvořili jste **propojené služby**:
    1. Propojená služba **Azure Storage** připojující účet úložiště Azure, který obsahuje vstupní data.    
    2. Propojená služba **Azure SQL** připojující službu Azure SQL Database, která obsahuje výstupní data. 
3.  Vytvořili jste **datové sady**, které popisují vstupní data a výstupní data pro kanály.
4.  Vytvořili jste **kanál** s **aktivitou kopírování**, která má jako zdroj **BlobSource** a jako jímku **SqlSink**. 


## Zobrazení objektů pro vytváření dat pomocí Průzkumníka serveru

1. V sadě **Visual Studio** klikněte v nabídce na **Zobrazit** a potom klikněte na **Průzkumník serveru**.
2. V okně Průzkumníka serveru rozbalte položku **Azure** a potom **Data Factory**. Pokud se zobrazí text **Přihlásit se k Visual Studiu**, zadejte **účet** přidružený k vašemu předplatnému Azure a klikněte na **Pokračovat**. Zadejte **heslo** a klikněte na **Přihlásit**. Visual Studio se pokusí získat informace o všech objektech pro vytváření dat Azure v rámci vašeho předplatného. Stav této operace se zobrazuje v okně **Data Factory Task List** (Seznam úkolů služby Data Factory).
    ![Průzkumník serveru](./media/data-factory-copy-activity-tutorial-using-visual-studio/server-explorer.png)
3. Můžete na některý objekt pro vytváření dat kliknout pravým tlačítkem a výběrem možnosti Export Data Factory to New Project (Exportovat objekt pro vytváření dat do nového projektu) vytvořit projekt sady Visual Studio založený na existujícím objektu pro vytváření dat.
    ![Export objektu pro vytváření dat do projektu VS](./media/data-factory-copy-activity-tutorial-using-visual-studio/export-data-factory-menu.png)  

## Aktualizace nástrojů služby Data Factory pro Visual Studio
Pokud chcete aktualizovat nástroje služby Azure Data Factory pro Visual Studio, postupujte takto:

1. V nabídce klikněte na **Nástroje** a vyberte **Rozšíření a aktualizace**. 
2. V levém podokně vyberte **Aktualizace** a vyberte **Galerie sady Visual Studio**.
4. Vyberte možnost **Azure Data Factory tools for Visual Studio** (Nástroje služby Azure Data Factory pro Visual Studio) a klikněte na **Aktualizovat**. Pokud se tato položka nezobrazí, znamená to, že máte nejnovější verzi těchto nástrojů. 

Pokyny k monitorování kanálu a datových sad, které jste vytvořili v tomto kurzu, pomocí webu Azure Portal najdete v článku [Monitorování datových sad a kanálu](data-factory-copy-activity-tutorial-using-azure-portal.md#monitor-pipeline).

## Viz také
| Téma | Popis |
| :---- | :---- |
| [Aktivity přesunu dat](data-factory-data-movement-activities.md) | Tento článek obsahuje podrobné informace o aktivitě kopírování, kterou jste v tomto kurzu použili. |
| [Plánování a provádění](data-factory-scheduling-and-execution.md) | Tento článek vysvětluje aspekty plánování a provádění aplikačního modelu služby Azure Data Factory. |
| [Kanály](data-factory-create-pipelines.md) | Tento článek vám pomůže pochopit kanály a aktivity ve službě Azure Data Factory a porozumět tomu, jak se dají ve vaší situaci nebo firmě použít k sestavení kompletních pracovních postupů založených na datech. |
| [Datové sady](data-factory-create-datasets.md) | Tento článek vám pomůže pochopit datové sady ve službě Azure Data Factory.
| [Monitorování a správa kanálů pomocí monitorovací aplikace](data-factory-monitor-manage-app.md) | Tento článek popisuje, jak monitorovat, spravovat a ladit kanály pomocí aplikace pro monitorování a správu. 



<!--HONumber=Aug16_HO4-->


