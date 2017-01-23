---
title: "Kurz: Vytvoření kanálu s aktivitou kopírování pomocí rozhraní REST API | Dokumentace Microsoftu"
description: "V tomto kurzu vytvoříte kanál Azure Data Factory s aktivitou kopírování pomocí rozhraní REST API."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: 1704cdf8-30ad-49bc-a71c-4057e26e7350
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 01/17/2017
ms.author: spelluru
translationtype: Human Translation
ms.sourcegitcommit: ebc5dbf790ca6012cfe9a7ea9ccee9fdacb46ffd
ms.openlocfilehash: ac1c60e04b42e2804ef17ba35368dd28c1d748a4


---
# <a name="tutorial-create-a-pipeline-with-copy-activity-using-rest-api"></a>Kurz: Vytvoření kanálu s aktivitou kopírování pomocí rozhraní REST API
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

V tomto kurzu je uvedeno, jak vytvořit a monitorovat objekt pro vytváření dat Azure pomocí rozhraní REST API. Kanál v objektu pro vytváření dat využívá aktivitu kopírování, s jejíž pomocí kopíruje data ze služby Azure Blob Storage do služby Azure SQL Database.

> [!NOTE]
> Tento článek nepopisuje všechny možnosti rozhraní REST API služby Data Factory. Úplnou dokumentaci o rutinách služby Data Factory najdete v článku [Rozhraní REST API služby Data Factory - referenční informace](https://msdn.microsoft.com/library/azure/dn906738.aspx).
> 
> 

## <a name="prerequisites"></a>Požadavky
* Projděte si [Přehled kurzu](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) a proveďte **nutné** kroky.
* Nainstalujte na svůj počítač nástroj [Curl](https://curl.haxx.se/dlwiz/). Pomocí nástroje Curl a příkazů REST vytvoříte objekt pro vytváření dat. 
* Postupujte podle pokynů v [tomto článku](../azure-resource-manager/resource-group-create-service-principal-portal.md) a proveďte následující: 
  1. V Azure Active Directory vytvořte webovou aplikaci s názvem **ADFCopyTutorialApp**.
  2. Získejte **ID klienta** a **tajný klíč**. 
  3. Získejte **ID tenanta**. 
  4. Přiřaďte aplikaci **ADFCopyTutorialApp** k roli **Přispěvatel Data Factory**.  
* Nainstalujte [Azure PowerShell](/powershell/azureps-cmdlets-docs).  
* Spusťte **PowerShell** a potom spusťte následující příkaz. Nechte prostředí Azure PowerShell otevřené až do konce tohoto kurzu. Pokud ho zavřete a znovu otevřete, bude potřeba tyto příkazy spustit znovu.
  
  1. Spusťte následující příkaz a zadejte uživatelské jméno a heslo, které používáte k přihlášení na web Azure Portal.
    
    ```PowerShell 
    Login-AzureRmAccount
    ```   
  2. Spuštěním následujícího příkazu zobrazíte všechna předplatná pro tento účet.

    ```PowerShell     
    Get-AzureRmSubscription
    ``` 
  3. Spuštěním následujícího příkazu vyberte předplatné, se kterým chcete pracovat. Místo **&lt;NameOfAzureSubscription**&gt; zadejte název svého předplatného Azure. 
     
    ```PowerShell
    Get-AzureRmSubscription -SubscriptionName <NameOfAzureSubscription> | Set-AzureRmContext
    ```
  4. Spuštěním následujícího příkazu v PowerShellu vytvořte skupinu prostředků Azure s názvem **ADFTutorialResourceGroup**.  

    ```PowerShell     
      New-AzureRmResourceGroup -Name ADFTutorialResourceGroup  -Location "West US"
    ```
     
      Pokud skupina prostředků už existuje, určete, jestli se má aktualizovat (Y), nebo ponechat tak, jak je (N). 
     
      Některé kroky v tomto kurzu vychází z předpokladu, že používáte skupinu prostředků s názvem ADFTutorialResourceGroup. Pokud používáte jinou skupinu prostředků, použijte v postupech v tomto kurzu místo skupiny ADFTutorialResourceGroup název vaší skupiny prostředků.

## <a name="create-json-definitions"></a>Vytvoření definic JSON
Vytvořte následující soubory JSON ve složce, ve které je umístěn soubor curl.exe. 

### <a name="datafactoryjson"></a>datafactory.json
> [!IMPORTANT]
> Název musí být globálně jedinečný, takže můžete přidat předponu nebo příponu k názvu ADFCopyTutorialDF tak, aby byl jedinečný. 
> 
> 

```JSON
{  
    "name": "ADFCopyTutorialDF",  
    "location": "WestUS"
}  
```

### <a name="azurestoragelinkedservicejson"></a>azurestoragelinkedservice.json
> [!IMPORTANT]
> Položky **accountname** a **accountkey** nahraďte názvem svého účtu Azure Storage a jeho klíčem. Informace o tom, jak získat přístupový klíč k úložišti, najdete v článku o [zobrazení, kopírování a opětovném vytváření přístupových klíčů úložiště](../storage/storage-create-storage-account.md#manage-your-storage-access-keys).
> 
> 

```JSON
{
    "name": "AzureStorageLinkedService",
    "properties": {
        "type": "AzureStorage",
        "typeProperties": {
            "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
        }
    }
}
```

### <a name="azuersqllinkedservicejson"></a>azuersqllinkedservice.json
> [!IMPORTANT]
> Položky **servername**, **databasename**, **username** a **password** nahraďte názvem serveru SQL Azure, názvem databáze SQL, uživatelským účtem a heslem pro daný účet.  
> 
> 

```JSON
{
    "name": "AzureSqlLinkedService",
    "properties": {
        "type": "AzureSqlDatabase",
        "description": "",
        "typeProperties": {
            "connectionString": "Data Source=tcp:<servername>.database.windows.net,1433;Initial Catalog=<databasename>;User ID=<username>;Password=<password>;Integrated Security=False;Encrypt=True;Connect Timeout=30"
        }
    }
}
```

### <a name="inputdatasetjson"></a>inputdataset.json

```JSON
{
  "name": "AzureBlobInput",
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
```

Definice JSON definuje datovou sadu s názvem **AzureBlobInput**, která představuje vstupní data pro aktivitu v kanálu. Kromě toho určuje, že vstupní data se nachází v souboru **emp.txt** v kontejneru objektů blob **adftutorial**. 

 Je třeba počítat s následujícím: 

* Vlastnost **type** datové sady je nastavená na **AzureBlob**.
* Vlastnost **linkedServiceName** je nastavená na **AzureStorageLinkedService**. 
* Vlastnost **folderPath** je nastavena na kontejner **adftutorial** a vlastnost **fileName** je nastavena na **emp.txt**.  
* Vlastnost **type** formátu je nastavená na **TextFormat**.
* V textovém souboru existují dvě pole, **FirstName** a **LastName**, oddělená čárkou (**columnDelimiter**).    
* Vlastnost **availability** je nastavená na **hourly** (frequency je nastavená na hour a interval je nastavená na 1). Proto služba Data Factory každou hodinu vyhledá vstupní data v kořenové složce zadaného kontejneru objektů blob (**adftutorial**). 

Pokud neurčíte **fileName** pro vstupní datovou sadu, všechny soubory nebo objekty ze vstupní složky (**folderPath**) se považují za vstupy. Pokud zadáte fileName v kódu JSON, bude se za vstup považovat jenom zadaný soubor nebo objekt blob.

Pokud nezadáte **fileName** pro **výstupní tabulku**, generované soubory v **folderPath** se pojmenují podle následujícího formátu: Data.&lt;identifikátor GUID&gt;.txt (například Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt).

Pokud chcete nastavit **folderPath** a **fileName** dynamicky podle času **SliceStart**, použijte vlastnost **partitionedBy**. V následujícím příkladu folderPath používá rok, měsíc a den z vlastnosti SliceStart (čas zahájení zpracování řezu) a fileName používá hodinu z vlastnosti SliceStart. Pokud například začne být řez vytvářen v době 2014-10-20T08:00:00, vlastnost folderName je nastavená na wikidatagateway/wikisampledataout/2014/10/20 a vlastnost fileName je nastavená na 08.csv. 

```JSON
  "folderPath": "wikidatagateway/wikisampledataout/{Year}/{Month}/{Day}",
"fileName": "{Hour}.csv",
"partitionedBy": 
[
    { "name": "Year", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyy" } },
    { "name": "Month", "value": { "type": "DateTime", "date": "SliceStart", "format": "MM" } }, 
    { "name": "Day", "value": { "type": "DateTime", "date": "SliceStart", "format": "dd" } }, 
    { "name": "Hour", "value": { "type": "DateTime", "date": "SliceStart", "format": "hh" } } 
],
```

### <a name="outputdatasetjson"></a>outputdataset.json

```JSON
{
  "name": "AzureSqlOutput",
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
```

Definice JSON definuje datovou sadu s názvem **AzureSqlOutput**, která představuje výstupní data pro aktivitu v kanálu. Kromě toho určuje, že výsledky jsou uloženy v tabulce **emp** v databázi reprezentované AzureSqlLinkedService. Oddíl **availability** určuje, že se výstupní datová sada vytváří každou hodinu (frekvence je hodina, interval 1).

Je třeba počítat s následujícím: 

* Vlastnost **type** datové sady je nastavená na **AzureSQLTable**.
* Vlastnost **linkedServiceName** je nastavená na **AzureSqlLinkedService**.
* Vlastnost **tablename** je nastavená na **emp**.
* V tabulce emp v databázi jsou k dispozici tři sloupce – **ID**, **FirstName** a **LastName**. ID je sloupec identity, takže je zde třeba zadat pouze položky **FirstName** (Jméno) a **LastName** (Příjmení).
* Vlastnost **availability** je nastavená na **hourly** (**frequency** je nastavená na **hour** a **interval** je nastavená na **1**).  Služba Data Factory bude generovat řez výstupních dat do tabulky **emp** ve službě Azure SQL Database každou hodinu.

### <a name="pipelinejson"></a>pipeline.json

```JSON
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
            "name": "AzureBlobInput"
          }
        ],
        "outputs": [
          {
            "name": "AzureSqlOutput"
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
    "start": "2016-08-12T00:00:00Z",
    "end": "2016-08-13T00:00:00Z"
  }
}
```

Je třeba počítat s následujícím:

* V části aktivit je jenom jedna aktivita, jejíž vlastnost **type** je nastavená na **CopyActivity**.
* Vstup aktivity je nastavený na **AzureBlobInput** a výstup aktivity je nastavený na **AzureSqlOutput**.
* V části **transformace** je vlastnost **BlobSource** určená jako typ zdroje a **SqlSink** jako typ jímky.

Nahraďte hodnotu vlastnosti **start** aktuálním dnem a **end** následujícím dnem. Můžete zadat jenom část data a přeskočit část času. Například „2015-02-03“ je ekvivalentní hodnotě „2015-02-03T00:00:00Z“.

Počáteční a koncové hodnoty data a času musí být ve [formátu ISO](http://en.wikipedia.org/wiki/ISO_8601). Například: 2014-10-14T16:32:41Z. Čas hodnoty **end** je nepovinný, ale my ho v tomto kurzu použijeme. 

Pokud nezadáte hodnotu vlastnosti **end**, vypočítá se jako „**start + 48 hodin**“. Pokud chcete kanál spouštět bez omezení, zadejte vlastnosti **end** hodnotu **9999-09-09**.

V příkladu je 24 datových řezů, protože se vytvářejí každou hodinu.

> [!NOTE]
> Podrobnosti o vlastnostech JSON použitých ve výše uvedeném příkladu najdete v článku [Anatomie kanálu](data-factory-create-pipelines.md).
> 
> 

## <a name="set-global-variables"></a>Nastavení globálních proměnných
V prostředí Azure PowerShell spusťte následující příkazy (po nahrazení ukázkových hodnot vašimi vlastními):

> [!IMPORTANT]
> Postup získání ID klienta, tajného klíče klienta, ID tenanta a ID předplatného naleznete v sekci [Požadavky](#prerequisites).   
> 
> 

```JSON
$client_id = "<client ID of application in AAD>"
$client_secret = "<client key of application in AAD>"
$tenant = "<Azure tenant ID>";
$subscription_id="<Azure subscription ID>";

$rg = "ADFTutorialResourceGroup"
$adf = "ADFCopyTutorialDF"
```

## <a name="authenticate-with-aad"></a>Ověření pomocí ADD
Spuštěním následujícího příkazu proveďte ověření pomocí služby Azure Active Directory (AAD). 

```PowerShell
$cmd = { .\curl.exe -X POST https://login.microsoftonline.com/$tenant/oauth2/token  -F grant_type=client_credentials  -F resource=https://management.core.windows.net/ -F client_id=$client_id -F client_secret=$client_secret };
$responseToken = Invoke-Command -scriptblock $cmd;
$accessToken = (ConvertFrom-Json $responseToken).access_token;

(ConvertFrom-Json $responseToken) 
```

## <a name="create-data-factory"></a>Vytvoření objektu pro vytváření dat
V tomto kroku vytvoříte objekt služby Azure Data Factory s názvem **ADFCopyTutorialDF**. Objekt pro vytváření dat může mít jeden nebo víc kanálů. Kanál může obsahovat jednu nebo víc aktivit. Například aktivita kopírování kopíruje data ze zdroje do cílového úložiště dat. Aktivita HDInsight Hive spustí skript Hive k transformaci vstupních dat na výstupní data produktu. Spuštěním následujícího příkazu vytvořte objekt pro vytváření dat: 

1. Přiřaďte příkaz k proměnné s názvem **cmd**. 
   
    Zkontrolujte, že název objektu pro vytváření dat zadaný zde (ADFCopyTutorialDF) odpovídá názvu zadanému v souboru **datafactory.json**. 
   
    ```PowerShell
    $cmd = {.\curl.exe -X PUT -H "Authorization: Bearer $accessToken" -H "Content-Type: application/json" --data “@datafactory.json” https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/ADFCopyTutorialDF?api-version=2015-10-01};
    ```
2. Příkaz spusťte pomocí **Invoke-Command**.
   
    ```PowerShell
    $results = Invoke-Command -scriptblock $cmd;
    ```
3. Zkontrolujte výsledky. Pokud byl objekt pro vytváření dat vytvořen úspěšně, v části **výsledky** se zobrazí JSON pro příslušný objekt pro vytváření dat. V opačném případě se zobrazí chybová zpráva.  
   
    ```
    Write-Host $results
    ```

Je třeba počítat s následujícím:

* Název objektu pro vytváření dat Azure musí být globálně jedinečný. Pokud se ve výsledcích zobrazí chyba **Název objektu pro vytváření dat „ADFCopyTutorialDF“ není k dispozici**, proveďte následující kroky:  
  
  1. Změňte název (například yournameADFCopyTutorialDF) v souboru **datafactory.json**.
  2. V prvním příkazu, kde je proměnné **$cmd** přiřazena hodnota, nahraďte ADFCopyTutorialDF novým názvem a spusťte příkaz. 
  3. Spuštěním následujících dvou příkazů vyvoláte rozhraní REST API za účelem vytvoření objektu pro vytváření dat a tisku výsledků operace. 
     
     V tématu [Objekty pro vytváření dat – pravidla pojmenování](data-factory-naming-rules.md) najdete pravidla pojmenování artefaktů služby Data Factory.
* Instance služby Data Factory můžete vytvářet jenom tehdy, když jste přispěvatelem/správcem předplatného Azure.
* Název objektu pro vytváření dat se může v budoucnu zaregistrovat jako název DNS, takže pak bude veřejně viditelný.
* Pokud se zobrazí chyba „**Pro předplatné není zaregistrované používání oboru názvů Microsoft.DataFactory**“, proveďte některý z těchto kroků a znovu zkuste název publikovat: 
  
  * V prostředí Azure PowerShell zaregistrujte zprostředkovatele služby Data Factory pomocí následujícího příkazu. 

    ```PowerShell    
    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.DataFactory
    ```
    Spuštěním následujícího příkazu si můžete ověřit, jestli je zprostředkovatel služby Data Factory zaregistrovaný. 
    
    ```PowerShell
    Get-AzureRmResourceProvider
    ```
  * Přihlaste se na web [Azure Portal ](https://portal.azure.com) pomocí předplatného Azure a přejděte do okna Objekt pro vytváření dat nebo na webu Azure Portal vytvořte objekt pro vytváření dat. Zprostředkovatel se při takovém postupu zaregistruje automaticky.

Před vytvořením kanálu je nejdřív potřeba vytvořit několik entit služby Data Factory. Nejprve vytvoříte propojené služby propojující zdrojové a cílové úložiště dat do vašeho úložiště dat. Poté definujte vstupní a výstupní datové sady pro reprezentaci dat v propojených úložištích dat. Nakonec vytvořte kanál s aktivitou, která používá tyto datové sady.

## <a name="create-linked-services"></a>Vytvoření propojených služeb
Propojené služby propojují úložiště dat nebo výpočetní služby s objektem pro vytváření dat Azure. Úložištěm dat může být Azure Storage, Azure SQL Database nebo místní databáze SQL Serveru, která obsahuje vstupní data nebo uložená výstupní data pro kanál služby Data Factory. Výpočetní služba je služba, která zpracovává vstupní data a vytváří výstupní data. 

V tomto kroku vytvoříte dvě propojené služby: **AzureStorageLinkedService** a **AzureSqlLinkedService**. Propojená služba AzureStorageLinkedService propojuje účet úložiště Azure a AzureSqlLinkedService propojuje službu Azure SQL Database s objektem pro vytváření dat **ADFCopyTutorialDF**. Později v tomto kurzu vytvoříte kanál, který kopíruje data z kontejneru objektů blob ve službě AzureStorageLinkedService do tabulky SQL ve službě AzureSqlLinkedService.

### <a name="create-azure-storage-linked-service"></a>Vytvoření propojené služby Azure Storage
V tomto kroku propojíte se svým objektem pro vytváření dat svůj účet služby Azure Storage. V tomto kurzu použijete účet služby Azure Storage k uložení vstupních dat. 

1. Přiřaďte příkaz k proměnné s názvem **cmd**. 

    ```PowerShell   
    $cmd = {.\curl.exe -X PUT -H "Authorization: Bearer $accessToken" -H "Content-Type: application/json" --data "@azurestoragelinkedservice.json" https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/$adf/linkedservices/AzureStorageLinkedService?api-version=2015-10-01};
    ```
2. Příkaz spusťte pomocí **Invoke-Command**.
    ```PowerShell   
    $results = Invoke-Command -scriptblock $cmd;
    ```
3. Zkontrolujte výsledky. Pokud byla propojená služba vytvořena úspěšně, v části **výsledky** se zobrazí JSON pro tuto propojenou službu. V opačném případě se zobrazí chybová zpráva.

    ```PowerShell   
    Write-Host $results
    ```

### <a name="create-azure-sql-linked-service"></a>Vytvoření propojené služby Azure SQL
V tomto kroku se svým objektem pro vytváření dat propojíte svou databázi SQL Azure. V tomto kurzu používáte stejnou službu Azure SQL Database k ukládání výstupních dat.

1. Přiřaďte příkaz k proměnné s názvem **cmd**. 
   
    ```PowerShell
    $cmd = {.\curl.exe -X PUT -H "Authorization: Bearer $accessToken" -H "Content-Type: application/json" --data “@azuresqllinkedservice.json” https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/$adf/linkedservices/AzureSqlLinkedService?api-version=2015-10-01};
    ```
2. Příkaz spusťte pomocí **Invoke-Command**.
   
    ```PowerShell
    $results = Invoke-Command -scriptblock $cmd;
    ```
3. Zkontrolujte výsledky. Pokud byla propojená služba vytvořena úspěšně, v části **výsledky** se zobrazí JSON pro tuto propojenou službu. V opačném případě se zobrazí chybová zpráva.
   
    ```PowerShell
    Write-Host $results
    ```

## <a name="create-datasets"></a>Vytvoření datových sad
V předchozím kroku jste vytvořili propojené služby **AzureStorageLinkedService** a **AzureSqlLinkedService**, abyste propojili účet Azure Storage a Azure SQL Database k objektu pro vytváření dat: **ADFCopyTutorialDF**. V tomto kroku vytvoříte datové sady, které představují vstupní a výstupní data pro aktivitu kopírování v kanálu, který vytvoříte v následujícím kroku. 

Vstupní datová sada v tomto kurzu odkazuje na kontejner objektů blob ve službě Azure Storage, na kterou odkazuje AzureStorageLinkedService. Výstupní datová sada odkazuje na tabulku SQL v Azure SQL Database, na kterou odkazuje AzureSqlLinkedService.  

### <a name="prepare-azure-blob-storage-and-azure-sql-database-for-the-tutorial"></a>Příprava služeb Azure Blob Storage a Azure SQL Database pro tento kurz
Abyste připravili služby Azure Blob Storage a Azure SQL Database pro tento kurz, proveďte následující kroky. 

* Vytvořte kontejner objektů blob s názvem **adftutorial** ve službě Azure Blob Storage, na kterou odkazuje objekt **AzureStorageLinkedService**. 
* Vytvořte a odešlete textový soubor **emp.txt** jako objekt blob do kontejneru **adftutorial**. 
* Vytvořte tabulku s názvem **emp** ve službě Azure SQL Database v databázi Azure SQL, na kterou odkazuje **AzureSqlLinkedService**.

1. Spusťte program Poznámkový blok, vložte následující text a uložte ho jako **emp.txt** do složky **C:\ADFGetStartedPSH** na pevném disku. 

    ```   
    John, Doe
    Jane, Doe
    ```
2. Pomocí nástrojů, jako je [Azure Storage Explorer](https://azurestorageexplorer.codeplex.com/), vytvořte kontejner **adftutorial** a odešlete soubor **emp.txt** do tohoto kontejneru.
   
    ![Azure Storage Explorer](media/data-factory-copy-activity-tutorial-using-powershell/getstarted-storage-explorer.png)
3. Pomocí následujícího skriptu SQL vytvořte tabulku **emp** v Azure SQL Database.  

    ```SQL
    CREATE TABLE dbo.emp 
    (
        ID int IDENTITY(1,1) NOT NULL,
        FirstName varchar(50),
        LastName varchar(50),
    )
    GO

    CREATE CLUSTERED INDEX IX_emp_ID ON dbo.emp (ID); 
    ```

    Pokud máte na počítači nainstalovaný SQL Server 2014: Postupujte podle pokynů v části [Krok 2: Připojení k SQL Database článku Správa Azure SQL Database pomocí aplikace SQL Server Management Studio][sql-management-studio], připojte se k serveru SQL Azure a spusťte skript SQL.

    Pokud klient nemá povolený přístup ke službě Azure SQL Server, budete muset nakonfigurovat bránu firewall pro Azure SQL Server tak, aby povolovala přístup z vašeho počítače (IP adresa). Postup konfigurace brány firewall pro server SQL Azure najdete v [tomto článku](../sql-database/sql-database-configure-firewall-settings.md).

### <a name="create-input-dataset"></a>Vytvoření vstupní datové sady
V tomto kroku vytvoříte datovou sadu s názvem **AzureBlobInput**, která odkazuje na kontejner objektů blob ve službě Azure Storage reprezentované propojenou službou **AzureStorageLinkedService**. Tento kontejner objektu blob (**adftutorial**) obsahuje vstupní data v souboru **emp.txt**. 

1. Přiřaďte příkaz k proměnné s názvem **cmd**. 

    ```PowerSHell   
    $cmd = {.\curl.exe -X PUT -H "Authorization: Bearer $accessToken" -H "Content-Type: application/json" --data "@inputdataset.json" https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/$adf/datasets/AzureBlobInput?api-version=2015-10-01};
    ```
2. Příkaz spusťte pomocí **Invoke-Command**.
   
    ```PowerShell
    $results = Invoke-Command -scriptblock $cmd;
    ```
3. Zkontrolujte výsledky. Pokud byla datová sada vytvořena úspěšně, v části **výsledky** se zobrazí JSON pro tuto datovou sadu. V opačném případě se zobrazí chybová zpráva.
   
    ```PowerShell
    Write-Host $results
    ```

### <a name="create-output-dataset"></a>Vytvoření výstupní datové sady
V tomto kroku vytvoříte vytvoří výstupní tabulku s názvem **AzureSqlOutput**. Tato datová sada odkazuje na tabulku SQL (**emp**) v Azure SQL Database, kterou reprezentuje **AzureSqlLinkedService**. Kanál kopíruje data z vstupního objektu blob do tabulky **emp**. 

1. Přiřaďte příkaz k proměnné s názvem **cmd**.

    ```PowerShell   
    $cmd = {.\curl.exe -X PUT -H "Authorization: Bearer $accessToken" -H "Content-Type: application/json" --data "@outputdataset.json" https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/$adf/datasets/AzureSqlOutput?api-version=2015-10-01};
    ```
2. Příkaz spusťte pomocí **Invoke-Command**.
    
    ```PowerShell   
    $results = Invoke-Command -scriptblock $cmd;
    ```
3. Zkontrolujte výsledky. Pokud byla datová sada vytvořena úspěšně, v části **výsledky** se zobrazí JSON pro tuto datovou sadu. V opačném případě se zobrazí chybová zpráva.
   
    ```PowerShell
    Write-Host $results
    ``` 

## <a name="create-pipeline"></a>Vytvoření kanálu
V tomto kroku vytvoříte kanál pomocí **aktivity kopírování**, který používá **AzureBlobInput** jako vstup a **AzureSqlOutput** jako výstup.

1. Přiřaďte příkaz k proměnné s názvem **cmd**.

    ```PowerShell   
    $cmd = {.\curl.exe -X PUT -H "Authorization: Bearer $accessToken" -H "Content-Type: application/json" --data "@pipeline.json" https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/$adf/datapipelines/MyFirstPipeline?api-version=2015-10-01};
    ```
2. Příkaz spusťte pomocí **Invoke-Command**.

    ```PowerShell   
    $results = Invoke-Command -scriptblock $cmd;
    ```
3. Zkontrolujte výsledky. Pokud byla datová sada vytvořena úspěšně, v části **výsledky** se zobrazí JSON pro tuto datovou sadu. V opačném případě se zobrazí chybová zpráva.  

    ```PowerShell   
    Write-Host $results
    ```

**Blahopřejeme!** Úspěšně jste vytvořili objekt pro vytváření dat Azure s kanálem, který kopíruje data ze služby Azure Blob Storage do databáze Azure SQL.

## <a name="monitor-pipeline"></a>Monitorování kanálu
V tomto kroku použijete rozhraní REST API služby Data Factory k monitorování řezů vytvářených kanálem.

```PowerShell
$ds ="AzureSqlOutput"
```

```PowerShell
$cmd = {.\curl.exe -X GET -H "Authorization: Bearer $accessToken" https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/$adf/datasets/$ds/slices?start=1970-01-01T00%3a00%3a00.0000000Z"&"end=2016-08-12T00%3a00%3a00.0000000Z"&"api-version=2015-10-01};
```

```PowerShell
$results2 = Invoke-Command -scriptblock $cmd;
```

```PowerShell
IF ((ConvertFrom-Json $results2).value -ne $NULL) {
    ConvertFrom-Json $results2 | Select-Object -Expand value | Format-Table
} else {
        (convertFrom-Json $results2).RemoteException
}
```

Spouštějte tyto příkazy, dokud se stav řezu nezobrazí jako **Připraveno** nebo **Chyba**. Pokud je řez ve stavu Připraveno, zkontrolujte výstupní data v tabulce **emp** ve vaší službě Azure SQL Database. 

Pro každý řez se ze zdrojového souboru do tabulky emp ve službě Azure SQL Database zkopírují dva řádky. Proto po úspěšném zpracování všech řezů (stav Připraveno) uvidíte v tabulce emp 24 nových záznamů. 

## <a name="summary"></a>Souhrn
V tomto kurzu jste pomocí rozhraní REST API vytvořili objekt pro vytváření dat Azure pro kopírování dat z objektu blob Azure do skužby Azure SQL Database. Zde jsou základní kroky, které jste v tomto kurzu provedli:  

1. Vytvořili jste **objekt pro vytváření dat** Azure.
2. Vytvořili jste **propojené služby**:
   1. Propojená služba Azure Storage připojující účet úložiště Azure, který obsahuje vstupní data.     
   2. Propojená služba Azure SQL připojující službu Azure SQL Database, která obsahuje výstupní data. 
3. Vytvořili jste **datové sady**, které popisují vstupní data a výstupní data pro kanály.
4. Vytvořili jste **kanál** s aktivitou kopírování, která má jako zdroj BlobSource a jako jímku SqlSink. 

## <a name="see-also"></a>Viz také
| Téma | Popis |
|:--- |:--- |
| [Aktivity přesunu dat](data-factory-data-movement-activities.md) |Tento článek obsahuje podrobné informace o aktivitě kopírování, kterou jste v tomto kurzu použili. |
| [Plánování a provádění](data-factory-scheduling-and-execution.md) |Tento článek vysvětluje aspekty plánování a provádění aplikačního modelu služby Azure Data Factory. |
| [Kanály](data-factory-create-pipelines.md) |Tento článek vám pomůže pochopit kanály a aktivity ve službě Azure Data Factory a porozumět tomu, jak se dají ve vaší situaci nebo firmě použít k sestavení kompletních pracovních postupů založených na datech. |
| [Datové sady](data-factory-create-datasets.md) |Tento článek vám pomůže pochopit datové sady ve službě Azure Data Factory. |
| [Monitorování a správa kanálů pomocí monitorovací aplikace](data-factory-monitor-manage-app.md) |Tento článek popisuje, jak monitorovat, spravovat a ladit kanály pomocí aplikace pro monitorování a správu. |

[use-custom-activities]: data-factory-use-custom-activities.md
[troubleshoot]: data-factory-troubleshoot.md
[developer-reference]: http://go.microsoft.com/fwlink/?LinkId=516908

[cmdlet-reference]: https://msdn.microsoft.com/library/azure/dn820234.aspx
[old-cmdlet-reference]: https://msdn.microsoft.com/library/azure/dn820234(v=azure.98).aspx
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[azure-portal]: http://portal.azure.com
[download-azure-powershell]: /powershell/azureps-cmdlets-docs
[data-factory-introduction]: data-factory-introduction.md

[image-data-factory-get-started-storage-explorer]: ./media/data-factory-copy-activity-tutorial-using-powershell/getstarted-storage-explorer.png

[sql-management-studio]: ../sql-database/sql-database-manage-azure-ssms.md



<!--HONumber=Dec16_HO4-->


