---
title: "Kopírování dat z SQL Serveru do BLob Storage s využitím Azure Data Factory | Dokumentace Microsoftu"
description: "Zjistěte, jak kopírovat data z místního úložiště dat do cloudu Azure s využitím místního prostředí Integration Runtime ve službě Azure Data Factory."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 11/14/2017
ms.author: jingwang
ms.openlocfilehash: afd7735712d03110a67509a7e94d336219a65b34
ms.sourcegitcommit: afc78e4fdef08e4ef75e3456fdfe3709d3c3680b
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/16/2017
---
# <a name="tutorial-copy-data-from-on-premises-sql-server-to-azure-blob-storage"></a>Kurz: Kopírování dat z místního SQL Serveru do Azure Blob Storage
V tomto kurzu použijete Azure PowerShell k vytvoření kanálu Data Factory, který kopíruje data z místní databáze SQL Serveru do úložiště objektů blob v Azure. Vytvoříte a použijete místní prostředí Integration Runtime (IR) služby Azure Data Factory. To umožňuje integraci místních a cloudových úložišť dat.  Další informace o vytvoření datové továrny pomocí jiných nástrojů nebo sad SDK najdete v tématu [Šablony Rychlý start](quickstart-create-data-factory-dot-net.md).

Tento článek neposkytuje podrobný úvod do služby Data Factory. Úvod do služby Azure Data Factory najdete v tématu [Úvod do Azure Data Factory](introduction.md). 

> [!NOTE]
> Tento článek se týká verze 2 služby Data Factory, která je aktuálně ve verzi Preview. Pokud používáte verzi 1 služby Data Factory, který je všeobecně dostupná (GA), prostudujte si [dokumentaci služby Data Factory verze 1](v1/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).

V tomto kurzu provedete následující kroky:

> [!div class="checklist"]
> * Vytvoření datové továrny
> * Vytvořte místní prostředí Integration Runtime.
> * Vytvoření propojených služeb SQL Server a Azure Storage 
> * Vytvoření datových sad SQL Serveru a Azure Blob
> * Vytvoření kanálu s aktivitou kopírování pro přesun dat
> * Zahájení spuštění kanálu
> * Monitorování spuštění kanálu

Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/) před tím, než začnete.

## <a name="prerequisites"></a>Požadavky

### <a name="sql-server-2014-or-2016"></a>SQL Server 2014 nebo 2016. 
V tomto kurzu použijete místní databázi SQL Serveru jako **zdrojové** úložiště dat. Ve své databázi SQL Serveru vytvořte tabulku **emp** a vložte do ní několik ukázkových záznamů.

1. Spusťte aplikaci **SQL Server Management Studio**. Pokud používáte SQL Server 2016, možná budete muset aplikaci SQL Server Management Studio nainstalovat samostatně z webu [Download Center](https://docs.microsoft.com/en-us/sql/ssms/download-sql-server-management-studio-ssms). 
2. Pomocí svých přihlašovacích údajů se přihlaste se ke svému SQL Serveru. 
3. Vytvořte ukázkovou databázi. Ve stromovém zobrazení klikněte pravým tlačítkem na **Databáze** a klikněte na **Nová databáze**. V dialogovém okně **Nová databáze** zadejte **název** databáze a klikněte na **OK**. 
4. Spusťte proti databázi následující skript dotazu, který vytvoří tabulku **emp**. Ve stromovém zobrazení klikněte pravým tlačítkem na **databázi**, kterou jste vytvořili, a klikněte na **Nový dotaz**. 

    ```sql   
    CREATE TABLE dbo.emp
    (
        ID int IDENTITY(1,1) NOT NULL,
        FirstName varchar(50),
        LastName varchar(50),
        CONSTRAINT PK_emp PRIMARY KEY (ID)
    )
    GO
    ```
2. Spusťte proti databázi následující příkazy, které do tabulky vloží ukázková data:

    ```sql
    INSERT INTO emp VALUES ('John', 'Doe')
    INSERT INTO emp VALUES ('Jane', 'Doe')
    ```

### <a name="azure-storage-account"></a>Účet služby Azure Storage
V tomto kurzu použijete účet služby Azure Storage (konkrétně služby Blob Storage) pro obecné účely jako **cílové úložiště dat nebo úložiště dat jímky**. Pokud nemáte účet úložiště Azure pro obecné účely, přečtěte si téma popisující [Vytvoření účtu úložiště](../storage/common/storage-create-storage-account.md#create-a-storage-account).

#### <a name="get-storage-account-name-and-account-key"></a>Získání názvu a klíče účtu úložiště
V tomto rychlém startu použijete název a klíč svého účtu úložiště Azure. Následující postup předvádí kroky k získání názvu a klíče vašeho účtu úložiště. 

1. Spusťte webový prohlížeč a přejděte na [Azure Portal](https://portal.azure.com). Přihlaste se pomocí svého uživatelského jména a hesla Azure. 
2. V nabídce vlevo klikněte na **Další služby >**, použijte filtr s klíčovým slovem **úložiště** a vyberte **Účty úložiště**.

    ![Vyhledání účtu úložiště](media/tutorial-hybrid-copy-powershell/search-storage-account.png)
3. V seznamu účtů úložiště vyfiltrujte váš účet úložiště (pokud je to potřeba) a pak vyberte **váš účet úložiště**. 
4. Na stránce **Účet úložiště** vyberte v nabídce **Přístupové klíče**.

    ![Získání názvu a klíče účtu úložiště](media/tutorial-hybrid-copy-powershell/storage-account-name-key.png)
5. Zkopírujte do schránky hodnoty z polí **Název účtu úložiště** a **klíč1**. Vložte je do Poznámkového bloku nebo jiného editoru a uložte je. Název a klíč účtu úložiště použijete v průběhu tohoto kurzu. 

#### <a name="create-the-adftutorial-container"></a>Vytvoření kontejneru adftutorial 
V této části vytvoříte ve svém úložišti objektů blob v Azure kontejner objektů blob s názvem adftutorial. 

1. Pokud jej ve svém počítači ještě nemáte, nainstalujte [Průzkumníka služby Azure Storage](https://azure.microsoft.com/features/storage-explorer/). 
2. Spusťte na svém počítači **Průzkumníka služby Microsoft Azure Storage**.   
3. V okně **Připojit ke službě Azure Storage** vyberte **Použít název a klíč účtu úložiště** a klikněte na **Další**. Pokud se okno **Připojit ke službě Azure Storage** nezobrazí, ve stromovém zobrazení klikněte pravým tlačítkem na **Účty úložiště** a pak klikněte na **Připojit k úložišti Azure**. 

    ![Připojení k úložišti Azure](media/tutorial-hybrid-copy-powershell/storage-explorer-connect-azure-storage.png)
4. Do okna **Připojit s použitím názvu a klíče** vložte **Název účtu** a **Klíč účtu**, které jste uložili v předchozím kroku. Pak klikněte na **Další**. 
5. V okně **Souhrn připojení** klikněte na **Připojit**.
6. Ověřte, že se váš účet úložiště zobrazí ve stromovém zobrazení v části **(Místní a připojené)** -> **Účty úložiště**. 
7. Rozbalte **Kontejnery objektů blob** a ověřte, že neexistuje kontejner objektů blob s názvem **adftutorial**. Pokud již existuje, přeskočte další kroky pro vytvoření tohoto kontejneru. 
8. Klikněte pravým tlačítkem na **Kontejnery objektů blob** a vyberte **Vytvořit kontejner objektů blob**.

    ![Vytvoření kontejneru objektů blob](media/tutorial-hybrid-copy-powershell/stroage-explorer-create-blob-container-menu.png)
9. Jako název zadejte **adftutorial** a stiskněte **ENTER**. 
10. Potvrďte, že je ve stromovém zobrazení vybraný kontejner **adftutorial**. Data Factory v tomto kontejneru vytvoří výstupní složku automaticky, takže ji nemusíte vytvářet. 

### <a name="azure-powershell"></a>Azure PowerShell

#### <a name="install-azure-powershell"></a>Instalace prostředí Azure PowerShell
Pokud jej ve svém počítači ještě nemáte, nainstalujte nejnovější Azure PowerShell. 

1. Ve webovém prohlížeči přejděte na stránku [Sady Azure SDK ke stažení a sady SDK](https://azure.microsoft.com/downloads/). 
2. V části **Nástroje příkazového řádku** -> **PowerShell** klikněte na **Instalace pro Windows**. 
3. Pokud chcete nainstalovat Azure PowerShell, spusťte soubor **MSI**. 

Podrobné pokyny najdete v tématu [Instalace a konfigurace prostředí Azure PowerShell](/powershell/azure/install-azurerm-ps). 

#### <a name="log-in-to-azure-powershell"></a>Přihlášení do prostředí Azure PowerShell
Spusťte na svém počítači **PowerShell**. Nechte prostředí Azure PowerShell otevřené až do konce tohoto kurzu Rychlý start. Pokud ho zavřete a znovu otevřete, bude potřeba tyto příkazy spustit znovu.

1. Spusťte následující příkaz a zadejte uživatelské jméno a heslo Azure, které používáte k přihlášení na Azure Portal:
       
    ```powershell
    Login-AzureRmAccount
    ```        
2. Pokud máte více předplatných Azure, spuštěním následujícího příkazu zobrazíte všechna předplatná pro tento účet:

    ```powershell
    Get-AzureRmSubscription
    ```
3. Spuštěním následujícího příkazu vyberte předplatné, se kterým chcete pracovat. Místo **SubscriptionId** použijte ID vašeho předplatného Azure:

    ```powershell
    Select-AzureRmSubscription -SubscriptionId "<SubscriptionId>"       
    ```

## <a name="create-a-data-factory"></a>Vytvoření datové továrny

1. Definujte proměnnou pro název skupiny prostředků, kterou použijete později v příkazech PowerShellu. Zkopírujte do PowerShellu následující text příkazu, zadejte název [skupiny prostředků Azure](../azure-resource-manager/resource-group-overview.md) v uvozovkách a pak příkaz spusťte. 
   
     ```powershell
    $resourceGroupName = "<Specify a name for the Azure resource group>";
    ```
2. Definujte proměnnou pro název datové továrny, kterou můžete později použít v příkazech prostředí PowerShell. 

    ```powershell
    $dataFactoryName = "<Specify a name for the data factory. It must be globally unique.>";
    ```
1. Definujte proměnnou pro umístění datové továrny: 

    ```powershell
    $location = "East US"
    ```
4. Pokud chcete vytvořit skupinu prostředků Azure, spusťte následující příkaz: 

    ```powershell
    New-AzureRmResourceGroup $resourceGroupName $location
    ``` 

    Pokud již skupina prostředků existuje, nepřepisujte ji. Přiřaďte proměnné `$resourceGroupName` jinou hodnotu a zkuste to znovu. Pokud chcete skupinu prostředků sdílet s ostatními, pokračujte k dalšímu kroku.  
5. Pokud chcete vytvořit datovou továrnu, spusťte následující rutinu **Set-AzureRmDataFactoryV2**: 
    
    ```powershell       
    Set-AzureRmDataFactoryV2 -ResourceGroupName $resourceGroupName -Location "East US" -Name $dataFactoryName 
    ```

Je třeba počítat s následujícím:

* Název objektu pro vytváření dat Azure musí být globálně jedinečný. Pokud se zobrazí následující chyba, změňte název a zkuste to znovu.

    ```
    The specified Data Factory name 'ADFv2QuickStartDataFactory' is already in use. Data Factory names must be globally unique.
    ```

* Instance služby Data Factory můžete vytvářet jenom tehdy, když jste **přispěvatelem** nebo **správcem** předplatného Azure.
* Data Factory verze 2 v současné době umožňuje vytváření datových továren jenom v oblastech Východní USA, Východní USA 2 a Západní Evropa. Úložiště dat (Azure Storage, Azure SQL Database atd.) a výpočetní prostředí (HDInsight atd.) používané datovou továrnou mohou být v jiných oblastech.

## <a name="create-a-self-hosted-ir"></a>Vytvoření místního prostředí IR

V této části můžete vytvořit místní prostředí Integration Runtime a přidružit ho k místnímu uzlu (počítači).

1. Vytvořte proměnnou pro název prostředí Integration Runtime. 

    ```powershell
   $integrationRuntimeName = "<your integration runtime name>"
    ```
1. Vytvořte místní prostředí Integration Runtime. Pokud už existuje jiné prostředí Integration Runtime se stejným názvem, použijte jedinečný název.

   ```powershell
   Set-AzureRmDataFactoryV2IntegrationRuntime -Name $integrationRuntimeName -Type SelfHosted -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName
   ```

   Zde je ukázkový výstup:

   ```json
    Id                : /subscriptions/<subscription ID>/resourceGroups/ADFTutorialResourceGroup/providers/Microsoft.DataFactory/factories/onpremdf0914/integrationruntimes/myonpremirsp0914
    Type              : SelfHosted
    ResourceGroupName : ADFTutorialResourceGroup
    DataFactoryName   : onpremdf0914
    Name              : myonpremirsp0914
    Description       :
    ```
 

2. Spusťte následující příkaz, který načte stav vytvořeného prostředí Integration Runtime.

   ```powershell
   Get-AzureRmDataFactoryV2IntegrationRuntime -name $integrationRuntimeName -ResourceGroupName $resourceGroupName -DataFactoryName $dataFactoryName -Status
   ```

   Zde je ukázkový výstup:

   ```json
   Nodes                     : {}
   CreateTime                : 9/14/2017 10:01:21 AM
   InternalChannelEncryption :
   Version                   :
   Capabilities              : {}
   ScheduledUpdateDate       :
   UpdateDelayOffset         :
   LocalTimeZoneOffset       :
   AutoUpdate                :
   ServiceUrls               : {eu.frontend.clouddatahub.net, *.servicebus.windows.net}
   ResourceGroupName         : <ResourceGroup name>
   DataFactoryName           : <DataFactory name>
   Name                      : <Integration Runtime name>
   State                     : NeedRegistration
   ```

3. Spusťte následující příkaz, který načte ověřovací klíče pro registraci místního prostředí Integration Runtime ve službě Data Factory v cloudu. Jeden z těchto klíčů zkopírujte pro registraci místního prostředí Integration Runtime.

   ```powershell
   Get-AzureRmDataFactoryV2IntegrationRuntimeKey -Name $integrationRuntimeName -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName | ConvertTo-Json
   ```

   Zde je ukázkový výstup:

   ```json
   {
       "AuthKey1":  "IR@0000000000-0000-0000-0000-000000000000@ab1@eu@VDnzgySwUfaj3pfSUxpvfsXXXXXXx4GHiyF4wboad0Y=",
       "AuthKey2":  "IR@0000000000-0000-0000-0000-000000000000@ab1@eu@sh+k/QNJGBltXL46vXXXXXXXXOf/M1Gne5aVqPtbweI="
   }
   ```

## <a name="install-integration-runtime"></a>Instalace prostředí Integration Runtime
1. Na místním počítači s Windows [stáhněte](https://www.microsoft.com/download/details.aspx?id=39717) místní prostředí Integration Runtime a spusťte instalaci. 
2. Na stránce **Vítá vás průvodce instalací prostředí Microsoft Integration Runtime** klikněte na **Další**.  
3. Na stránce **Licenční smlouva s koncovým uživatelem (EULA)** přijměte podmínky a licenční smlouvu a klikněte na **Další**. 
4. Na stránce **Cílová složka** klikněte na **Další**. 
5. Na stránce **Připraveno k instalaci prostředí Microsoft Integration Runtime** klikněte na **Nainstalovat**. 
6. Pokud se zobrazí zpráva upozorňující na konfiguraci přechodu počítače do režimu spánku nebo hibernace, když se zrovna nepoužívá, klikněte na **OK**. 
7. Na stránce **Dokončení průvodce instalací prostředí Microsoft Integration Runtime** klikněte na **Dokončit**.
8. Na stránce **Registrace prostředí Integration Runtime (v místním prostředí)** vložte klíč, který jste uložili v předchozí části, a klikněte na **Zaregistrovat**. 

   ![Registrace prostředí Integration Runtime](media/tutorial-hybrid-copy-powershell/register-integration-runtime.png)
2. Po úspěšném dokončení registrace místního prostředí Integration Runtime se zobrazí následující zpráva:

   ![Úspěšně zaregistrováno](media/tutorial-hybrid-copy-powershell/registered-successfully.png)

3. Na stránce **Nový uzel prostředí Integration Runtime (v místním prostředí)** klikněte na **Další**. 

    ![Stránka Nový uzel prostředí Integration Runtime](media/tutorial-hybrid-copy-powershell/new-integration-runtime-node-page.png)
4. Na stránce **Komunikační kanál intranetu** klikněte na **Přeskočit**. V prostředí Integration Runtime s více uzly můžete pro zabezpečení komunikace v rámci uzlů vybrat certifikaci TLS/SSL. 

    ![Stránka Komunikační kanál intranetu](media/tutorial-hybrid-copy-powershell/intranet-communication-channel-page.png)
5. Na stránce **Registrace prostředí Integration Runtime (v místním prostředí)** klikněte na **Spustit Správce konfigurace**. 
6. Jakmile se uzel připojí ke cloudové službě, zobrazí se následující stránka:

   ![Uzel je připojen](media/tutorial-hybrid-copy-powershell/node-is-connected.png)

## <a name="create-linked-services"></a>Vytvoření propojených služeb

### <a name="create-an-azure-storage-linked-service-destinationsink"></a>Vytvoření propojené služby Azure Storage (cíl/jímka)

1. Vytvořte soubor JSON s názvem **AzureStorageLinkedService.json** ve složce **C:\ADFv2Tutorial** s následujícím obsahem. Pokud složka ADFv2Tutorial ještě neexistuje, vytvořte ji.  

    > [!IMPORTANT]
    > Než soubor uložíte, položky &lt;accountName&gt; a &lt;accountKey&gt; nahraďte názvem svého účtu úložiště Azure a jeho klíčem.

   ```json
    {
        "properties": {
            "type": "AzureStorage",
            "typeProperties": {
                "connectionString": {
                    "type": "SecureString",
                    "value": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
                }
            }
        },
        "name": "AzureStorageLinkedService"
    }
   ```
2. V **Azure PowerShellu** přejděte do složky **ADFv2Tutorial**.

   Spuštěním rutiny **Set-AzureRmDataFactoryV2LinkedService** vytvořte propojenou službu **AzureStorageLinkedService**. Rutiny používané v tomto kurzu přebírají hodnoty pro parametry **ResourceGroupName** a **DataFactoryName**. Alternativně můžete předat objekt **DataFactory** vrácený rutinou Set-AzureRmDataFactoryV2, abyste nemuseli při každém spuštění rutiny zadávat hodnoty parametrů ResourceGroupName a DataFactoryName.

   ```powershell
   Set-AzureRmDataFactoryV2LinkedService -DataFactoryName $dataFactoryName -ResourceGroupName $ResourceGroupName -Name "AzureStorageLinkedService" -File ".\AzureStorageLinkedService.json"
   ```

   Zde je ukázkový výstup:

    ```json
    LinkedServiceName : AzureStorageLinkedService
    ResourceGroupName : ADFTutorialResourceGroup
    DataFactoryName   : onpremdf0914
    Properties        : Microsoft.Azure.Management.DataFactory.Models.AzureStorageLinkedService
    ```

### <a name="create-and-encrypt-a-sql-server-linked-service-source"></a>Vytvoření a šifrování propojené služby SQL Serveru (zdroj)

1. Ve složce **C:\ADFv2Tutorial** vytvořte soubor JSON s názvem **SqlServerLinkedService.json** s následujícím obsahem: Než soubor uložíte, položky **&lt;servername>** (název serveru), **&lt;databasename>** (název databáze), **&lt;username>** (uživatelské jméno), **&lt;servername>** (název serveru) a **&lt;password>** (heslo) nahraďte hodnotami vašeho SQL Serveru. 

    > [!IMPORTANT]
    > Položku **&lt;integration** **runtime** **name>** nahraďte názvem vašeho prostředí Integration Runtime.

    ```json
    {
        "properties": {
            "type": "SqlServer",
            "typeProperties": {
                "connectionString": {
                    "type": "SecureString",
                    "value": "Server=<servername>;Database=<databasename>;User ID=<username>;Password=<password>;Timeout=60"
                }
            },
            "connectVia": {
                "type": "integrationRuntimeReference",
                "referenceName": "<integration runtime name>"
            }
        },
        "name": "SqlServerLinkedService"
    }
   ```
2. Pro zajištění šifrování citlivých dat z datové části JSON v místním prostředí Integration Runtime spusťte rutinu **New-AzureRmDataFactoryV2LinkedServiceEncryptedCredential** a předejte jí výše uvedenou datovou část JSON. Toto šifrování zajišťuje šifrování přihlašovacích údajů pomocí rozhraní Data Protection API. Zašifrované přihlašovací údaje jsou místně uložené v uzlu místního prostředí Integration Runtime (místní počítač). Výstupní datovou část je možné přesměrovat do jiného souboru JSON (v tomto případě encryptedLinkedService.json), který obsahuje zašifrované přihlašovací údaje.
    
   ```powershell
   New-AzureRmDataFactoryV2LinkedServiceEncryptedCredential -DataFactoryName $dataFactoryName -ResourceGroupName $ResourceGroupName -IntegrationRuntimeName $integrationRuntimeName -File ".\SQLServerLinkedService.json" > encryptedSQLServerLinkedService.json
   ```

3. Spuštěním následujícího příkazu s použitím souboru JSON z předchozího kroku vytvořte propojenou službu **SqlServerLinkedService**:

   ```powershell
   Set-AzureRmDataFactoryV2LinkedService -DataFactoryName $dataFactoryName -ResourceGroupName $ResourceGroupName -Name "EncryptedSqlServerLinkedService" -File ".\encryptedSqlServerLinkedService.json"
   ```


## <a name="create-datasets"></a>Vytvoření datových sad
V tomto kroku vytvoříte vstupní a výstupní datové sady, které představují vstupní a výstupní data pro operaci kopírování (místní databáze SQL Serveru => úložiště objektů blob v Azure).

### <a name="create-a-dataset-for-source-sql-database"></a>Vytvoření datové sady pro zdrojovou databázi SQL Database

1. Ve složce **C:\ADFv2Tutorial** vytvořte soubor JSON s názvem **SqlServerDataset.json** s následujícím obsahem:  

    ```json
    {
       "properties": {
            "type": "SqlServerTable",
            "typeProperties": {
                "tableName": "dbo.emp"
            },
            "structure": [
                 {
                    "name": "ID",
                    "type": "String"
                },
                {
                    "name": "FirstName",
                    "type": "String"
                },
                {
                    "name": "LastName",
                    "type": "String"
                }
            ],
            "linkedServiceName": {
                "referenceName": "EncryptedSqlServerLinkedService",
                "type": "LinkedServiceReference"
            }
        },
        "name": "SqlServerDataset"
    }
    ```

2. Spuštěním rutiny **Set-AzureRmDataFactoryV2Dataset** vytvořte datovou sadu **SqlServerDataset**.

    ```powershell
    Set-AzureRmDataFactoryV2Dataset -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName -Name "SqlServerDataset" -File ".\SqlServerDataset.json"
    ```

    Zde je ukázkový výstup:

    ```json
    DatasetName       : SqlServerDataset
    ResourceGroupName : ADFTutorialResourceGroup
    DataFactoryName   : onpremdf0914
    Structure         : {"name": "ID" "type": "String", "name": "FirstName" "type": "String", "name": "LastName" "type": "String"}
    Properties        : Microsoft.Azure.Management.DataFactory.Models.SqlServerTableDataset
    ```

### <a name="create-a-dataset-for-sink-azure-blob-storage"></a>Vytvoření datové sady pro službu Azure Blob Storage pro jímku

1. Ve složce **C:\ADFv2Tutorial** vytvořte soubor JSON s názvem **AzureBlobDataset.json** s následujícím obsahem:

    > [!IMPORTANT]
    > Tento vzorový kód předpokládá, že ve službě Azure Blob Storage máte kontejner s názvem **adftutorial**.

    ```json
    {
        "properties": {
            "type": "AzureBlob",
            "typeProperties": {
                "folderPath": "adftutorial/fromonprem",
                "format": {
                    "type": "TextFormat"
                }
            },
            "linkedServiceName": {
                "referenceName": "AzureStorageLinkedService",
                "type": "LinkedServiceReference"
            }
        },
        "name": "AzureBlobDataset"
    }
    ```

2. Spuštěním rutiny **Set-AzureRmDataFactoryV2Dataset** vytvořte datovou sadu **AzureBlobDataset**.

    ```powershell
    Set-AzureRmDataFactoryV2Dataset -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName -Name "AzureBlobDataset" -File ".\AzureBlobDataset.json"
    ```

    Zde je ukázkový výstup:

    ```json
    DatasetName       : AzureBlobDataset
    ResourceGroupName : ADFTutorialResourceGroup
    DataFactoryName   : onpremdf0914
    Structure         :
    Properties        : Microsoft.Azure.Management.DataFactory.Models.AzureBlobDataset
    ```

## <a name="create-pipelines"></a>Vytvoření kanálů

### <a name="create-the-pipeline-sqlservertoblobpipeline"></a>Vytvoření kanálu SqlServerToBlobPipeline

1. Ve složce **C:\ADFv2Tutorial** vytvořte soubor JSON s názvem **SqlServerToBlobPipeline.json** s následujícím obsahem:

    ```json
    {
       "name": "SQLServerToBlobPipeline",
        "properties": {
            "activities": [       
                {
                    "type": "Copy",
                    "typeProperties": {
                        "source": {
                            "type": "SqlSource"
                        },
                        "sink": {
                            "type":"BlobSink"
                        }
                    },
                    "name": "CopySqlServerToAzureBlobActivity",
                    "inputs": [
                        {
                            "referenceName": "SqlServerDataset",
                            "type": "DatasetReference"
                        }
                    ],
                    "outputs": [
                        {
                            "referenceName": "AzureBlobDataset",
                            "type": "DatasetReference"
                        }
                    ]
                }
            ]
        }
    }
    ```

2. Spuštěním rutiny **Set-AzureRmDataFactoryV2Pipeline** vytvořte kanál **SQLServerToBlobPipeline**.

    ```powershell
    Set-AzureRmDataFactoryV2Pipeline -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName -Name "SQLServerToBlobPipeline" -File ".\SQLServerToBlobPipeline.json"
    ```

    Zde je ukázkový výstup:

    ```json
    PipelineName      : SQLServerToBlobPipeline
    ResourceGroupName : ADFTutorialResourceGroup
    DataFactoryName   : onpremdf0914
    Activities        : {CopySqlServerToAzureBlobActivity}
    Parameters        :  
    ```



## <a name="start-and-monitor-a-pipeline-run"></a>Spuštění kanálu a jeho monitorování

1. Zahajte spuštění pro kanál SQLServerToBlobPipeline a zaznamenejte ID spuštění kanálu pro budoucí monitorování.  

    ```powershell
    $runId = Invoke-AzureRmDataFactoryV2Pipeline -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName -PipelineName 'SQLServerToBlobPipeline'
    ```

2. Spusťte následující skript, který bude nepřetržitě kontrolovat stav spuštění kanálu **SQLServerToBlobPipeline** a vytiskne konečný výsledek.

    ```powershell
    while ($True) {
        $result = Get-AzureRmDataFactoryV2ActivityRun -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName -PipelineRunId $runId -RunStartedAfter (Get-Date).AddMinutes(-30) -RunStartedBefore (Get-Date).AddMinutes(30)

        if (($result | Where-Object { $_.Status -eq "InProgress" } | Measure-Object).count -ne 0) {
            Write-Host "Pipeline run status: In Progress" -foregroundcolor "Yellow"
            Start-Sleep -Seconds 30
        }
        else {
            Write-Host "Pipeline 'SQLServerToBlobPipeline' run finished. Result:" -foregroundcolor "Yellow"
            $result
            break
        }
    }
    ```

    Zde je výstup tohoto ukázkového spuštění:

    ```jdon
    ResourceGroupName : <resourceGroupName>
    DataFactoryName   : <dataFactoryName>
    ActivityName      : copy
    PipelineRunId     : 4ec8980c-62f6-466f-92fa-e69b10f33640
    PipelineName      : SQLServerToBlobPipeline
    Input             :  
    Output            :  
    LinkedServiceName :
    ActivityRunStart  : 9/13/2017 1:35:22 PM
    ActivityRunEnd    : 9/13/2017 1:35:42 PM
    DurationInMs      : 20824
    Status            : Succeeded
    Error             : {errorCode, message, failureType, target}
    ```

3. Následujícím způsobem můžete získat ID spuštění kanálu **SQLServerToBlobPipeline** a zkontrolovat podrobné výsledky spuštění aktivit.

    ```powershell
    Write-Host "Pipeline 'SQLServerToBlobPipeline' run result:" -foregroundcolor "Yellow"
    ($result | Where-Object {$_.ActivityName -eq "CopySqlServerToAzureBlobActivity"}).Output.ToString()
    ```

    Zde je výstup tohoto ukázkového spuštění:

    ```json
    {
      "dataRead": 36,
      "dataWritten": 24,
      "rowsCopied": 2,
      "copyDuration": 3,
      "throughput": 0.01171875,
      "errors": [],
      "effectiveIntegrationRuntime": "MyIntegrationRuntime",
      "billedDuration": 3
    }
    ```
## <a name="verify-the-output"></a>Ověření výstupu
Kanál v kontejneru objektů blob `adftutorial` automaticky vytvoří výstupní složku `fromonprem`. Zkontrolujte, že výstupní složka obsahuje soubor **dbo.emp.txt**. Pomocí [Průzkumníka služby Azure Storage](https://azure.microsoft.com/features/storage-explorer/) ověřte vytvoření výstupu. 

## <a name="next-steps"></a>Další kroky
Kanál v této ukázce kopíruje data z jednoho umístění do jiného umístění v úložišti objektů blob v Azure. Naučili jste se tyto postupy:

> [!div class="checklist"]
> * Vytvoření datové továrny
> * Vytvořte místní prostředí Integration Runtime.
> * Vytvoření propojených služeb SQL Server a Azure Storage 
> * Vytvoření datových sad SQL Serveru a Azure Blob
> * Vytvoření kanálu s aktivitou kopírování pro přesun dat
> * Zahájení spuštění kanálu
> * Monitorování spuštění kanálu

Seznam úložišť dat podporovaných službou Azure Data Factory najdete v tématu popisujícím [podporovaná úložiště dat](copy-activity-overview.md#supported-data-stores-and-formats).

Pokud se chcete dozvědět víc o hromadném kopírování dat ze zdroje do cíle, přejděte k následujícímu kurzu:

> [!div class="nextstepaction"]
>[Hromadné kopírování dat](tutorial-bulk-copy.md)
