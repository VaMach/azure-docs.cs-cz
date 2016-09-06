<properties 
    pageTitle="Kurz: Vytvoření kanálu s aktivitou kopírování pomocí Azure PowerShellu | Microsoft Azure" 
    description="V tomto kurzu vytvoříte kanál služby Azure Data Factory s aktivitou kopírování pomocí Azure PowerShellu." 
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

# Kurz: Vytvoření kanálu s aktivitou kopírování pomocí Azure PowerShellu
> [AZURE.SELECTOR]
- [Přehled kurzu](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
- [Pomocí editoru služby Data Factory](data-factory-copy-activity-tutorial-using-azure-portal.md)
- [Pomocí prostředí PowerShell](data-factory-copy-activity-tutorial-using-powershell.md)
- [Pomocí sady Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md)
- [Pomocí rozhraní REST API](data-factory-copy-activity-tutorial-using-rest-api.md)
- [Pomocí rozhraní .NET API](data-factory-copy-activity-tutorial-using-dotnet-api.md)
- [Pomocí průvodce kopírováním](data-factory-copy-data-wizard-tutorial.md)

Kurz [Kopírování dat z Blob Storage do SQL Database](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) ukazuje, jak můžete vytvořit a monitorovat objekt pro vytváření dat Azure pomocí webu [Azure Portal][azure-portal]. V tomto kurzu vytvoříte a budete monitorovat objekt pro vytváření dat Azure pomocí rutin Azure PowerShellu. Kanál v objektu pro vytváření dat, který vytvoříte v tomto kurzu, používá aktivitu kopírování ke kopírování dat z objektu blob Azure do Azure SQL Database.

Aktivita kopírování provádí přesun dat ve službě Azure Data Factory. Aktivita používá globálně dostupnou službu, která může kopírovat data mezi různými úložišti dat zabezpečeným, spolehlivým a škálovatelným způsobem. Podrobnosti o aktivitě kopírování najdete v článku [Aktivity přesunu dat](data-factory-data-movement-activities.md).   

> [AZURE.IMPORTANT] 
> Než se pustíte do tohoto kurzu, přečtěte si článek [Přehled kurzu](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) a proveďte nutné kroky.
>   
> Tento článek nepopisuje všechny rutiny služby Data Factory. Úplnou dokumentaci o rutinách služby Data Factory najdete v článku [Referenční informace o rutinách služby Data Factory](https://msdn.microsoft.com/library/dn820234.aspx).
  

##Požadavky
Vedle požadavků uvedených v tématu Přehled kurzu musíte nainstalovat tyto položky:

- **Azure PowerShell**. Podle pokynů v článku [Instalace a konfigurace prostředí Azure PowerShell](../powershell-install-configure.md) si na počítač nainstalujte prostředí Azure PowerShell.

##V tomto kurzu
Následující tabulka uvádí kroky, které budete provádět v rámci kurzu, a jejich popisy. 

Krok | Popis
-----| -----------
[Vytvoření služby Azure Data Factory](#create-data-factory) | V tomto kroku vytvoříte objekt pro vytváření dat Azure s názvem **ADFTutorialDataFactoryPSH**. 
[Vytvoření propojených služeb](#create-linked-services) | V tomto kroku vytvoříte dvě propojené služby: **StorageLinkedService** a **AzureSqlLinkedService**. StorageLinkedService propojuje službu Azure Storage a AzureSqlLinkedService propojuje službu Azure SQL Database k objektu ADFTutorialDataFactoryPSH.
[Vytvoření vstupní a výstupní datové sady](#create-datasets) | V tomto kroku definujete dvě sady dat (**EmpTableFromBlob** a **EmpSQLTable**). Tyto datové sady se používají jako vstupní a výstupní tabulka pro **Aktivitu kopírování** v objektu ADFTutorialPipeline, který vytvoříte v následujícím kroku.
[Vytvoření a spuštění kanálu](#create-pipeline) | V tomto kroku vytvoříte v objektu pro vytváření dat **ADFTutorialDataFactoryPSH** kanál s názvem **ADFTutorialPipeline**. Kanál bude mít **aktivitu kopírování**, která kopíruje data z objektu blob Azure do výstupní tabulky databáze Azure.
[Monitorování datových sad a kanálu](#monitor-pipeline) | V tomto kroku budete monitorovat datové sady a kanál pomocí Azure PowerShellu.

## Vytvoření objektu pro vytváření dat
V tomto kroku vytvoříte pomocí Azure PowerShellu objekt pro vytváření dat Azure s názvem **ADFTutorialDataFactoryPSH**.

1. Spusťte **PowerShell** a potom spusťte následující příkaz. Nechte prostředí Azure PowerShell otevřené až do konce tohoto kurzu. Pokud ho zavřete a znovu otevřete, bude potřeba tyto příkazy spustit znovu.
    1. Spusťte následující příkaz a zadejte uživatelské jméno a heslo, které používáte k přihlášení na web Azure Portal.
    
            Login-AzureRmAccount   
    2. Spuštěním následujícího příkazu zobrazíte všechna předplatná pro tento účet.

            Get-AzureRmSubscription 
    3. Spuštěním následujícího příkazu vyberte předplatné, se kterým chcete pracovat. Místo **&lt;NameOfAzureSubscription**&gt; zadejte název svého předplatného Azure. 

            Get-AzureRmSubscription -SubscriptionName <NameOfAzureSubscription> | Set-AzureRmContext

3. Spuštěním následujícího příkazu vytvořte skupinu prostředků Azure s názvem **ADFTutorialResourceGroup**.
   
        New-AzureRmResourceGroup -Name ADFTutorialResourceGroup  -Location "West US"

    Některé kroky v tomto kurzu vychází z předpokladu, že používáte skupinu prostředků s názvem **ADFTutorialResourceGroup**. Pokud máte jinou skupinu prostředků, použijte ji v postupech v tomto kurzu místo skupiny ADFTutorialResourceGroup. 
4. Spuštěním rutiny **New-AzureRmDataFactory** vytvořte objekt pro vytváření dat s názvem **ADFTutorialDataFactoryPSH**.  

        New-AzureRmDataFactory -ResourceGroupName ADFTutorialResourceGroup -Name ADFTutorialDataFactoryPSH –Location "West US"

    
Je třeba počítat s následujícím:
 
- Název objektu pro vytváření dat Azure musí být globálně jedinečný. Pokud se zobrazí následující chyba, změňte název (třeba na váš_název_ADFTutorialDataFactoryPSH). Při provádění postupů v tomto kurzu potom tento název používejte místo názvu ADFTutorialFactoryPSH. V tématu [Objekty pro vytváření dat – pravidla pojmenování](data-factory-naming-rules.md) najdete pravidla pojmenování artefaktů služby Data Factory.
    
        Data factory name “ADFTutorialDataFactoryPSH” is not available
- Instance služby Data Factory můžete vytvářet jenom tehdy, když jste přispěvatelem/správcem předplatného Azure.
- Název objektu pro vytváření dat se může v budoucnu zaregistrovat jako název DNS, takže pak bude veřejně viditelný.
- Pokud se zobrazí chyba „**Pro předplatné není zaregistrované používání oboru názvů Microsoft.DataFactory**“, proveďte některý z těchto kroků a znovu zkuste název publikovat: 

    - V prostředí Azure PowerShell zaregistrujte zprostředkovatele služby Data Factory pomocí následujícího příkazu. 
        
            Register-AzureRmResourceProvider -ProviderNamespace Microsoft.DataFactory
    
        Spuštěním následujícího příkazu si můžete ověřit, jestli je zprostředkovatel služby Data Factory zaregistrovaný. 
    
            Get-AzureRmResourceProvider
    - Přihlaste se na web [Azure Portal ](https://portal.azure.com) pomocí předplatného Azure a přejděte do okna Objekt pro vytváření dat nebo na webu Azure Portal vytvořte objekt pro vytváření dat. Zprostředkovatel se při takovém postupu zaregistruje automaticky.

## Vytvoření propojených služeb
Propojené služby propojují úložiště dat nebo výpočetní služby s objektem pro vytváření dat Azure. Úložištěm dat může být Azure Storage, Azure SQL Database nebo místní databáze SQL Serveru, která obsahuje vstupní data nebo uložená výstupní data pro kanál služby Data Factory. Výpočetní služba je služba, která zpracovává vstupní data a vytváří výstupní data. 

V tomto kroku vytvoříte dvě propojené služby: **StorageLinkedService** a **AzureSqlLinkedService**. Propojená služba StorageLinkedService propojuje účet úložiště Azure a AzureSqlLinkedService1 propojuje službu Azure SQL Database k objektu pro vytváření dat **ADFTutorialDataFactoryPSH**. Později v tomto kurzu vytvoříte kanál, který kopíruje data z kontejneru objektů blob ve službě StorageLinkedService do tabulky SQL ve službě AzureSqlLinkedService.

### Vytvoření propojené služby pro účet úložiště Azure
1.  Ve složce **C:\ADFGetStartedPSH** vytvořte soubor JSON s názvem **StorageLinkedService.json** s následujícím obsahem. Pokud složka ADFGetStartedPSH neexistuje, vytvořte ji.

            {
                "name": "StorageLinkedService",
                "properties": {
                    "type": "AzureStorage",
                    "typeProperties": {
                        "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
                    }
                }
            }

    Položky **accountname** a **accountkey** nahraďte názvem svého účtu Azure Storage a jeho klíčem.
2.  V prostředí **Azure PowerShell** přejděte do složky **ADFGetStartedPSH**. 
3.  K vytvoření propojené služby můžete použít rutinu **New-AzureRmDataFactoryLinkedService**. Tato rutina a další rutiny služby Data Factory používané v tomto kurzu vyžadují, abyste zadali hodnoty parametrů **ResourceGroupName** a **DataFactoryName**. Alternativně můžete pomocí rutiny **Get-AzureRmDataFactory** načíst objekt DataFactory a tento objekt předat, abyste nemuseli při každém spouštění rutiny zadávat hodnoty parametrů ResourceGroupName a DataFactoryName. Spuštěním následujícího příkazu přiřaďte výstup rutiny **Get-AzureRmDataFactory** k proměnné **$df**. 

        $df=Get-AzureRmDataFactory -ResourceGroupName ADFTutorialResourceGroup -Name ADFTutorialDataFactoryPSH

4.  Dál spuštěním rutiny **New-AzureRmDataFactoryLinkedService** vytvořte propojenou službu **StorageLinkedService**. 

        New-AzureRmDataFactoryLinkedService $df -File .\StorageLinkedService.json

    Pokud jste nespustili rutinu **Get-AzureRmDataFactory** a nepřiřadili výstup k proměnné **$df**, bude potřeba zadat následující hodnoty parametrů ResourceGroupName a DataFactoryName.   
        
        New-AzureRmDataFactoryLinkedService -ResourceGroupName ADFTutorialResourceGroup -DataFactoryName ADFTutorialDataFactoryPSH -File .\StorageLinkedService.json

    Pokud v průběhu kurzu zavřete prostředí Azure PowerShell, bude při dalším spuštění prostředí Azure PowerShell potřeba znovu spustit rutinu Get-AzureRmDataFactory, abyste mohli kurz dokončit.

### Vytvoření propojené služby pro Azure SQL Database
1.  Vytvořte soubor JSON s názvem AzureSqlLinkedService.json s následujícím obsahem.

            {
                "name": "AzureSqlLinkedService",
                "properties": {
                    "type": "AzureSqlDatabase",
                    "typeProperties": {
                        "connectionString": "Server=tcp:<server>.database.windows.net,1433;Database=<databasename>;User ID=user@server;Password=<password>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
                    }
                }
            }

    Položky **servername**, **databasename**, **username@servername** a **password** nahraďte názvem serveru SQL Azure, názvem databáze, uživatelským účtem a heslem.

2.  Spuštěním následujícího příkazu vytvoříte propojenou službu. 
    
        New-AzureRmDataFactoryLinkedService $df -File .\AzureSqlLinkedService.json

    Ujistěte se, že nastavení **Povolit přístup ke službám Azure** je pro server SQL Azure zapnuté. Chcete-li to ověřit a zapnout ho, udělejte toto:

    1. Klikněte na **PROCHÁZET** na levé straně a klikněte na **Servery SQL**.
    2. Vyberte server a klikněte na **NASTAVENÍ** v okně SQL SERVER.
    3. V okně **NASTAVENÍ** klikněte na **Brána firewall**.
    4. V okně **Nastavení brány firewall** klikněte na **ZAPNUTO** u možnosti **Povolit přístup ke službám Azure**.
    5. Klikněte na **AKTIVNÍ** na levé straně, abyste se přepnuli do okna**Data Factory**, kde jste byli.
    

## Vytvoření datových sad

V předchozím kroku jste vytvořili propojené služby **StorageLinkedService** a **AzureSqlLinkedService**, abyste propojili účet úložiště Azure a Azure SQL Database k objektu pro vytváření dat: **ADFTutorialDataFactoryPSH**. V tomto kroku vytvoříte datové sady, které představují vstupní a výstupní data pro aktivitu kopírování v kanálu, který vytvoříte v následujícím kroku. 

Tabulka je obdélníková datová sada a je jediným typem datové sady, který je nyní podporovaný. Vstupní tabulka v tomto kurzu odkazuje na kontejner objektů blob v úložišti Azure, na které odkazuje AzureStorageLinkedService. Výstupní tabulka odkazuje na tabulku SQL v Azure SQL Database, na kterou odkazuje AzureSqlLinkedService.  

### Příprava služeb Azure Blob Storage a Azure SQL Database pro tento kurz
Pokud jste prošli kurzem z článku [Kopírování dat z Blob Storage do SQL Database](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md), tento krok přeskočte. 

Abyste připravili služby Azure Blob Storage a Azure SQL Database pro tento kurz, je potřeba provést následující kroky. 
 
* Vytvořte kontejner objektů blob s názvem **adftutorial** ve službě Azure Blob Storage, na kterou odkazuje **StorageLinkedService**. 
* Vytvořte a odešlete textový soubor **emp.txt** jako objekt blob do kontejneru **adftutorial**. 
* Vytvořte tabulku s názvem **emp** ve službě Azure SQL Database v databázi Azure SQL, na kterou odkazuje **AzureSqlLinkedService**.


1. Spusťte program Poznámkový blok, vložte následující text a uložte ho jako **emp.txt** do složky **C:\ADFGetStartedPSH** na pevném disku. 

        John, Doe
        Jane, Doe
                
2. Pomocí nástrojů, jako je [Azure Storage Explorer](https://azurestorageexplorer.codeplex.com/), vytvořte kontejner **adftutorial** a odešlete soubor **emp.txt** do tohoto kontejneru.

    ![Azure Storage Explorer](media/data-factory-copy-activity-tutorial-using-powershell/getstarted-storage-explorer.png)
3. Pomocí následujícího skriptu SQL vytvořte tabulku **emp** v Azure SQL Database.  


        CREATE TABLE dbo.emp 
        (
            ID int IDENTITY(1,1) NOT NULL,
            FirstName varchar(50),
            LastName varchar(50),
        )
        GO

        CREATE CLUSTERED INDEX IX_emp_ID ON dbo.emp (ID); 

    Pokud máte na počítači nainstalovaný SQL Server 2014: Postupujte podle pokynů v části [Krok 2: Připojení k SQL Database článku Správa Azure SQL Database pomocí aplikace SQL Server Management Studio](../sql-database/sql-database-manage-azure-ssms.md), připojte se k serveru SQL Azure a spusťte skript SQL.

    Pokud klient nemá povolený přístup ke službě Azure SQL Server, budete muset nakonfigurovat bránu firewall pro Azure SQL Server tak, aby povolovala přístup z vašeho počítače (IP adresa). Postup konfigurace brány firewall pro server SQL Azure najdete v [tomto článku](../sql-database/sql-database-configure-firewall-settings.md).
        
### Vytvoření vstupní datové sady 
Tabulka je obdélníková datová sada a má schéma. V tomto kroku vytvoříte tabulku s názvem **EmpBlobTable**, která odkazuje na kontejner objektů blob ve službě Azure Storage reprezentované propojenou službou **StorageLinkedService**. Tento kontejner objektu blob (**adftutorial**) obsahuje vstupní data v souboru **emp.txt**. 

1.  Ve složce **C:\ADFGetStartedPSH** vytvořte soubor JSON s názvem **EmpBlobTable.json** s následujícím obsahem:

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
                "linkedServiceName": "StorageLinkedService",
                "typeProperties": {
                  "fileName": "emp.txt",
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
    
    - Vlastnost **type** datové sady je nastavená na **AzureBlob**.
    - Vlastnost **linkedServiceName** je nastavená na **StorageLinkedService**. 
    - Vlastnost **folderPath** je nastavená na kontejner **adftutorial**. 
    - Vlastnost **filename** je nastavená na **emp.txt**. Pokud neurčíte název objektu blob, budou se za vstupní data považovat data ze všech objektů blob v kontejneru.  
    - Vlastnost **type** formátu je nastavená na **TextFormat**.
    - V textovém souboru existují dvě pole, **FirstName** a **LastName**, oddělená čárkou (**columnDelimiter**). 
    - Vlastnost **availability** je nastavená na **hourly** (**frequency** je nastavená na **hour** a **interval** je nastavená na **1**). Proto služba Data Factory každou hodinu vyhledá vstupní data v kořenové složce v kontejneru objektů blob (**adftutorial**), který jste zadali.

    Pokud neurčíte **fileName** pro **vstupní** **tabulku**, všechny soubory nebo objekty blob ze vstupní složky (**folderPath**) se považují za vstupy. Pokud zadáte fileName v kódu JSON, bude se za vstup považovat jenom zadaný soubor nebo objekt blob. 
 
    Pokud nezadáte **fileName** pro **výstupní tabulku**, generované soubory v **folderPath** se pojmenují podle následujícího formátu: Data.<identifikátor GUID\>.txt (například Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt).

    Chcete-li nastavit **folderPath** a **fileName** dynamicky podle času **SliceStart**, použijte vlastnost **partitionedBy**. V následujícím příkladu folderPath používá rok, měsíc a den z vlastnosti SliceStart (čas zahájení zpracování řezu) a fileName používá hodinu z vlastnosti SliceStart. Pokud například začne být řez vytvářen v době 2014-10-20T08:00:00, vlastnost folderName je nastavená na wikidatagateway/wikisampledataout/2014/10/20 a vlastnost fileName je nastavená na 08.csv. 

            "folderPath": "wikidatagateway/wikisampledataout/{Year}/{Month}/{Day}",
            "fileName": "{Hour}.csv",
            "partitionedBy": 
            [
                { "name": "Year", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyy" } },
                { "name": "Month", "value": { "type": "DateTime", "date": "SliceStart", "format": "MM" } }, 
                { "name": "Day", "value": { "type": "DateTime", "date": "SliceStart", "format": "dd" } }, 
                { "name": "Hour", "value": { "type": "DateTime", "date": "SliceStart", "format": "hh" } } 
            ],

    Podrobné informace o vlastnostech JSON najdete v tématu [JSON Scripting Reference](http://go.microsoft.com/fwlink/?LinkId=516971) (Referenční příručka skriptování JSON).

2.  Spuštěním následujícího příkazu vytvořte datovou sadu služby Data Factory.

        New-AzureRmDataFactoryDataset $df -File .\EmpBlobTable.json

### Vytvoření výstupní datové sady
V tomto kroku vytvoříte výstupní datovou sadu s názvem **EmpSQLTable**. Tato datová sada odkazuje na tabulku SQL (**emp**) v Azure SQL Database, kterou reprezentuje **AzureSqlLinkedService**. Kanál kopíruje data z vstupního objektu blob do tabulky **emp**. 

1.  Ve složce **C:\ADFGetStartedPSH** vytvořte soubor JSON s názvem **EmpSQLTable.json** s následujícím obsahem:
        
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
    
    * Vlastnost **type** datové sady je nastavená na **AzureSqlTable**.
    * Vlastnost **linkedServiceName** je nastavená na **AzureSqlLinkedService**.
    * Vlastnost **tablename** je nastavená na **emp**.
    * V tabulce emp v databázi jsou k dispozici tři sloupce – **ID**, **FirstName** a **LastName**. ID je sloupec identity, takže je zde třeba zadat pouze položky **FirstName** (Jméno) a **LastName** (Příjmení).
    * Vlastnost **availability** je nastavená na **hourly** (**frequency** je nastavená na **hour** a **interval** je nastavená na **1**).  Služba Data Factory bude generovat řez výstupních dat do tabulky **emp** ve službě Azure SQL Database každou hodinu.

2.  Spuštěním následujícího příkazu vytvořte datovou sadu služby Data Factory. 
    
        New-AzureRmDataFactoryDataset $df -File .\EmpSQLTable.json


## Vytvoření kanálu
V tomto kroku vytvoříte kanál pomocí **aktivity kopírování**, který používá **EmpTableFromBlob** jako vstup a **EmpSQLTable** jako výstup.

1.  Ve složce **C:\ADFGetStartedPSH** vytvořte soubor JSON s názvem **ADFTutorialPipeline.json** s následujícím obsahem: 
    
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
                        "type": "SqlSink"
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
                "start": "2016-08-09T00:00:00Z",
                "end": "2016-08-10T00:00:00Z",
                "isPaused": false
              }
            }

    Je třeba počítat s následujícím:

    - V části aktivit je jenom jedna aktivita, jejíž vlastnost **type** je nastavená na **Copy**.
    - Vstup aktivity je nastavený na **EmpTableFromBlob** a výstup aktivity je nastavený na **EmpSQLTable**.
    - V části **transformace** je vlastnost **BlobSource** určená jako typ zdroje a **SqlSink** jako typ jímky.

    Nahraďte hodnotu vlastnosti **start** aktuálním dnem a **end** následujícím dnem. Počáteční a koncové hodnoty data a času musí být ve [formátu ISO](http://en.wikipedia.org/wiki/ISO_8601). Například: 2014-10-14T16:32:41Z. Čas hodnoty **end** je nepovinný, ale my ho v tomto kurzu použijeme. 
    
    Pokud nezadáte hodnotu vlastnosti **end**, vypočítá se jako „**start + 48 hodin**“. Chcete-li kanál spouštět bez omezení, zadejte vlastnosti **end** hodnotu **9/9/9999**.
    
    V příkladu je 24 datových řezů, protože se vytvářejí každou hodinu.
    
    Podrobné informace o vlastnostech JSON najdete v tématu [JSON Scripting Reference](http://go.microsoft.com/fwlink/?LinkId=516971) (Referenční příručka skriptování JSON).
2.  Spuštěním následujícího příkazu vytvořte tabulku služby Data Factory. 
        
        New-AzureRmDataFactoryPipeline $df -File .\ADFTutorialPipeline.json

**Blahopřejeme!** Úspěšně jste vytvořili objekt pro vytváření dat Azure, propojené služby, tabulky a kanál a naplánovali jste kanál.

## Monitorování kanálu
V tomto kroku budete pomocí prostředí Azure PowerShell monitorovat, co se děje v objektu pro vytváření dat Azure.

1.  Spusťte rutinu **Get-AzureRmDataFactory** a přiřaďte výstup k proměnné $df.

        $df=Get-AzureRmDataFactory -ResourceGroupName ADFTutorialResourceGroup -Name ADFTutorialDataFactoryPSH
 
2.  Spuštěním rutiny **Get-AzureRmDataFactorySlice** získejte podrobnosti o všech řezech tabulky **EmpSQLTable**, která je výstupní tabulkou kanálu.  

        Get-AzureRmDataFactorySlice $df -DatasetName EmpSQLTable -StartDateTime 2015-03-03T00:00:00

    Rok, měsíc a den v parametru **StartDateTime** nahraďte aktuálním rokem, měsícem a dnem. Toto nastavení by mělo odpovídat hodnotě **Start** v kódu JSON kanálu. 

    Měli byste vidět 24 řezů, pro každou hodinu od 12:00 aktuálního dne do 12:00 následujícího dne jeden. 
    
    **První řez:**

        ResourceGroupName : ADFTutorialResourceGroup
        DataFactoryName   : ADFTutorialDataFactoryPSH
        TableName         : EmpSQLTable
        Start             : 3/3/2015 12:00:00 AM
        End               : 3/3/2015 1:00:00 AM
        RetryCount        : 0
        Status            : Waiting
        LatencyStatus     :
        LongRetryCount    : 0

    **Druhý řez:**

        ResourceGroupName : ADFTutorialResourceGroup
        DataFactoryName   : ADFTutorialDataFactoryPSH
        TableName         : EmpSQLTable
        Start             : 3/4/2015 11:00:00 PM
        End               : 3/4/2015 12:00:00 AM
        RetryCount        : 0
        Status            : Waiting
        LatencyStatus     : 
        LongRetryCount    : 0

3.  Spuštěním rutiny **Get-AzureRmDataFactoryRun** získáte podrobnosti o spouštění aktivity pro **konkrétní** řez. Změňte hodnotu parametru **StartDateTime** tak, aby odpovídala času **Start** řezu ve výstupu. Hodnota **StartDateTime** musí být ve [formátu ISO](http://en.wikipedia.org/wiki/ISO_8601). Příklad: 2014-03-03T22:00:00Z.

        Get-AzureRmDataFactoryRun $df -DatasetName EmpSQLTable -StartDateTime 2015-03-03T22:00:00

    Zobrazený výstup by měl vypadat přibližně takto:

        Id                  : 3404c187-c889-4f88-933b-2a2f5cd84e90_635614488000000000_635614524000000000_EmpSQLTable
        ResourceGroupName   : ADFTutorialResourceGroup
        DataFactoryName     : ADFTutorialDataFactoryPSH
        TableName           : EmpSQLTable
        ProcessingStartTime : 3/3/2015 11:03:28 PM
        ProcessingEndTime   : 3/3/2015 11:04:36 PM
        PercentComplete     : 100
        DataSliceStart      : 3/8/2015 10:00:00 PM
        DataSliceEnd        : 3/8/2015 11:00:00 PM
        Status              : Succeeded
        Timestamp           : 3/8/2015 11:03:28 PM
        RetryAttempt        : 0
        Properties          : {}
        ErrorMessage        :
        ActivityName        : CopyFromBlobToSQL
        PipelineName        : ADFTutorialPipeline
        Type                : Copy

Úplnou dokumentaci o rutinách služby Data Factory najdete v článku [Referenční informace o rutinách služby Data Factory][cmdlet-reference]. 

## Souhrn
V tomto kurzu jste vytvořili objekt pro vytváření dat Azure pro zkopírování dat z objektu blob Azure do Azure SQL Database. PowerShell jste použili k vytvoření objektu pro vytváření dat, propojených služeb, datových sad a kanálu. Zde jsou základní kroky, které jste v tomto kurzu provedli:  

1.  Vytvořili jste **objekt pro vytváření dat** Azure.
2.  Vytvořili jste **propojené služby**:
    1. Propojená služba **Azure Storage** připojující účet úložiště Azure, který obsahuje vstupní data.    
    2. Propojená služba **Azure SQL** připojující službu Azure SQL Database, která obsahuje výstupní data. 
3.  Vytvořili jste **datové sady**, které popisují vstupní data a výstupní data pro kanály.
4.  Vytvořili jste **kanál** s **aktivitou kopírování**, která má jako zdroj **BlobSource** a jako jímku **SqlSink**. 

## Viz také
| Téma | Popis |
| :---- | :---- |
| [Aktivity přesunu dat](data-factory-data-movement-activities.md) | Tento článek obsahuje podrobné informace o aktivitě kopírování, kterou jste v tomto kurzu použili. |
| [Plánování a provádění](data-factory-scheduling-and-execution.md) | Tento článek vysvětluje aspekty plánování a provádění aplikačního modelu služby Azure Data Factory. |
| [Kanály](data-factory-create-pipelines.md) | Tento článek vám pomůže pochopit kanály a aktivity ve službě Azure Data Factory. |
| [Datové sady](data-factory-create-datasets.md) | Tento článek vám pomůže pochopit datové sady ve službě Azure Data Factory.
| [Monitorování a správa kanálů pomocí monitorovací aplikace](data-factory-monitor-manage-app.md) | Tento článek popisuje, jak monitorovat, spravovat a ladit kanály pomocí aplikace pro monitorování a správu. 



[use-custom-activities]: data-factory-use-custom-activities.md
[troubleshoot]: data-factory-troubleshoot.md
[developer-reference]: http://go.microsoft.com/fwlink/?LinkId=516908

[cmdlet-reference]: https://msdn.microsoft.com/library/azure/dn820234.aspx
[old-cmdlet-reference]: https://msdn.microsoft.com/library/azure/dn820234(v=azure.98).aspx
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[azure-portal]: http://portal.azure.com
[download-azure-powershell]: ../powershell-install-configure.md
[data-factory-introduction]: data-factory-introduction.md

[image-data-factory-get-started-storage-explorer]: ./media/data-factory-copy-activity-tutorial-using-powershell/getstarted-storage-explorer.png

[sql-management-studio]: ../sql-database/sql-database-manage-azure-ssms.md
 


<!--HONumber=ago16_HO5-->


