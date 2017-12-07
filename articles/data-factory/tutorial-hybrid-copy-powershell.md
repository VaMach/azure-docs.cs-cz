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
ms.date: 11/30/2017
ms.author: jingwang
ms.openlocfilehash: ba47f3e3f331929b884f27f49bf6e484ff363765
ms.sourcegitcommit: be0d1aaed5c0bbd9224e2011165c5515bfa8306c
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/01/2017
---
# <a name="tutorial-copy-data-from-on-premises-sql-server-to-azure-blob-storage"></a>Kurz: Kopírování dat z místního SQL Serveru do Azure Blob Storage
V tomto kurzu použijete Azure PowerShell k vytvoření kanálu Data Factory, který kopíruje data z místní databáze SQL Serveru do úložiště objektů blob v Azure. Vytvoříte a použijete místní prostředí Integration Runtime, které přesouvá data mezi místním a cloudovým úložištěm dat. 

> [!NOTE]
> Tento článek se týká verze 2 služby Data Factory, která je aktuálně ve verzi Preview. Pokud používáte verzi 1 služby Data Factory, který je všeobecně dostupná (GA), prostudujte si [dokumentaci služby Data Factory verze 1](v1/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).
> 
> Tento článek neposkytuje podrobný úvod do služby Data Factory. Úvod do služby Azure Data Factory najdete v tématu [Úvod do Azure Data Factory](introduction.md). 

V tomto kurzu provedete následující kroky:

> [!div class="checklist"]
> * Vytvoření datové továrny
> * Vytvořte místní prostředí Integration Runtime.
> * Vytvoření propojených služeb SQL Server a Azure Storage 
> * Vytvoření datových sad SQL Serveru a Azure Blob
> * Vytvoření kanálu s aktivitou kopírování pro přesun dat
> * Zahájení spuštění kanálu
> * Monitorování spuštění kanálu

## <a name="prerequisites"></a>Požadavky
### <a name="azure-subscription"></a>Předplatné Azure
Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/) před tím, než začnete.

### <a name="azure-roles"></a>Role Azure
Pro vytvoření instancí Data Factory musí být uživatelský účet, který použijete pro přihlášení k Azure, členem rolí **přispěvatel** nebo **vlastník** nebo **správcem** předplatného Azure. Na webu Azure Portal klikněte na **uživatelské jméno** v pravém horním rohu a vyberte **Oprávnění**. Zobrazí se oprávnění, která v příslušném předplatném máte. Pokud máte přístup k několika předplatným, vyberte odpovídající předplatné. Ukázku pokynů pro přidání uživatele k roli najdete v článku věnovaném [přidání rolí](../billing/billing-add-change-azure-subscription-administrator.md).

### <a name="sql-server-201420162017"></a>SQL Server 2014/2016/2017
V tomto kurzu použijete místní databázi SQL Serveru jako **zdrojové** úložiště dat. Kanál v datové továrně, který vytvoříte v tomto kurzu, kopíruje data z této místní databáze SQL Serveru (zdroj) do úložiště objektů blob v Azure (jímka). Ve své databázi SQL Serveru vytvořte tabulku **emp** a vložte do ní několik ukázkových záznamů. 

1. Na svém počítači spusťte **SQL Server Management Studio**. Pokud na svém počítači nemáte SQL Server Management Studio, nainstalujte ho z webu [Download Center](https://docs.microsoft.com/en-us/sql/ssms/download-sql-server-management-studio-ssms). 
2. Pomocí svých přihlašovacích údajů se přihlaste se ke svému SQL Serveru. 
3. Vytvořte ukázkovou databázi. Ve stromovém zobrazení klikněte pravým tlačítkem na **Databáze** a klikněte na **Nová databáze**. V dialogovém okně **Nová databáze** zadejte **název** databáze a klikněte na **OK**. 
4. Spusťte proti databázi následující skript dotazu, který vytvoří tabulku **emp** a vloží do ní nějaká ukázková data. Ve stromovém zobrazení klikněte pravým tlačítkem na **databázi**, kterou jste vytvořili, a klikněte na **Nový dotaz**. 

    ```sql   
    CREATE TABLE dbo.emp
    (
        ID int IDENTITY(1,1) NOT NULL,
        FirstName varchar(50),
        LastName varchar(50),
        CONSTRAINT PK_emp PRIMARY KEY (ID)
    )

    INSERT INTO emp VALUES ('John', 'Doe')
    INSERT INTO emp VALUES ('Jane', 'Doe')
    GO
    ```

### <a name="azure-storage-account"></a>Účet služby Azure Storage
V tomto kurzu použijete účet služby Azure Storage (konkrétně služby Blob Storage) pro obecné účely jako **cílové úložiště dat nebo úložiště dat jímky**. Pokud nemáte účet úložiště Azure pro obecné účely, přečtěte si téma popisující [Vytvoření účtu úložiště](../storage/common/storage-create-storage-account.md#create-a-storage-account). Kanál v datové továrně, který vytvoříte v tomto kurzu, kopíruje data z místní databáze SQL Serveru (zdroj) do tohoto úložiště objektů blob v Azure (jímka). 

#### <a name="get-storage-account-name-and-account-key"></a>Získání názvu a klíče účtu úložiště
V tomto kurzu použijete název a klíč svého účtu úložiště Azure. Následující postup předvádí kroky k získání názvu a klíče vašeho účtu úložiště. 

1. Spusťte webový prohlížeč a přejděte na [Azure Portal](https://portal.azure.com). Přihlaste se pomocí svého uživatelského jména a hesla Azure. 
2. V nabídce vlevo klikněte na **Další služby >**, použijte filtr s klíčovým slovem **úložiště** a vyberte **Účty úložiště**.

    ![Vyhledání účtu úložiště](media/tutorial-hybrid-copy-powershell/search-storage-account.png)
3. V seznamu účtů úložiště vyfiltrujte váš účet úložiště (pokud je to potřeba) a pak vyberte **váš účet úložiště**. 
4. Na stránce **Účet úložiště** vyberte v nabídce **Přístupové klíče**.

    ![Získání názvu a klíče účtu úložiště](media/tutorial-hybrid-copy-powershell/storage-account-name-key.png)
5. Zkopírujte do schránky hodnoty z polí **Název účtu úložiště** a **klíč1**. Vložte je do Poznámkového bloku nebo jiného editoru a uložte je. Název a klíč účtu úložiště použijete v průběhu tohoto kurzu. 

#### <a name="create-the-adftutorial-container"></a>Vytvoření kontejneru adftutorial 
V této části vytvoříte ve svém úložišti objektů blob v Azure kontejner objektů blob s názvem **adftutorial**. 

1. Na stránce **Účet úložiště** přepněte na **Přehled** a potom klikněte na **Objekty blob**. 

    ![Výběr možnosti Objekty blob](media/tutorial-hybrid-copy-powershell/select-blobs.png)
1. Na stránce **Blob Service** klikněte na panelu nástrojů na **+ Kontejner**. 

    ![Tlačítko pro přidání kontejneru](media/tutorial-hybrid-copy-powershell/add-container-button.png)
3. V dialogovém okně **Nový kontejner** jako název zadejte **adftutorial** a klikněte na **OK**. 

    ![Zadání názvu kontejneru](media/tutorial-hybrid-copy-powershell/new-container-dialog.png)
4. V seznamu kontejnerů klikněte na **adftutorial**.  

    ![Výběr kontejneru](media/tutorial-hybrid-copy-powershell/seelct-adftutorial-container.png)
5. Stránku **Kontejner** pro **adftutorial** nechte otevřenou. Použijete ji k ověření výstupu na konci tohoto kurzu. Data Factory v tomto kontejneru vytvoří výstupní složku automaticky, takže ji nemusíte vytvářet.

    ![Stránka Kontejner](media/tutorial-hybrid-copy-powershell/container-page.png)

### <a name="windows-powershell"></a>Windows PowerShell

#### <a name="install-powershell"></a>Instalace PowerShellu
Pokud jej ve svém počítači ještě nemáte, nainstalujte nejnovější PowerShell. 

1. Ve webovém prohlížeči přejděte na stránku [Sady Azure SDK ke stažení a sady SDK](https://azure.microsoft.com/downloads/). 
2. V části **Nástroje příkazového řádku** -> **PowerShell** klikněte na **Instalace pro Windows**. 
3. Pokud chcete nainstalovat Azure PowerShell, spusťte soubor **MSI**. 

Podrobné pokyny najdete v tématu [Instalace a konfigurace prostředí Azure PowerShell](/powershell/azure/install-azurerm-ps). 

#### <a name="log-in-to-powershell"></a>Přihlášení do PowerShellu

1. Spusťte na svém počítači **PowerShell**. Nechte okno PowerShellu otevřené až do konce tohoto kurzu Rychlý start. Pokud ho zavřete a znovu otevřete, tyto příkazy bude potřeba znovu spustit.

    ![Spuštění PowerShellu](media/tutorial-hybrid-copy-powershell/search-powershell.png)
1. Spusťte následující příkaz a zadejte uživatelské jméno a heslo Azure, které používáte k přihlášení na Azure Portal:
       
    ```powershell
    Login-AzureRmAccount
    ```        
2. Pokud máte více předplatných Azure, spuštěním následujícího příkazu zobrazíte všechna předplatná pro tento účet:

    ```powershell
    Get-AzureRmSubscription
    ```
3. Pokud máte několik předplatných Azure, spuštěním následujícího příkazu vyberte předplatné, se kterým chcete pracovat. Místo **SubscriptionId** použijte ID vašeho předplatného Azure:

    ```powershell
    Select-AzureRmSubscription -SubscriptionId "<SubscriptionId>"       
    ```

## <a name="create-a-data-factory"></a>Vytvoření datové továrny

1. Definujte proměnnou pro název skupiny prostředků, kterou použijete později v příkazech PowerShellu. Zkopírujte do PowerShellu následující text příkazu, zadejte název [skupiny prostředků Azure](../azure-resource-manager/resource-group-overview.md) v uvozovkách a pak příkaz spusťte. Například: `"adfrg"`. 
   
     ```powershell
    $resourceGroupName = "ADFTutorialResourceGroup"
    ```
2. Pokud chcete vytvořit skupinu prostředků Azure, spusťte následující příkaz: 

    ```powershell
    New-AzureRmResourceGroup $resourceGroupName $location
    ``` 

    Pokud již skupina prostředků existuje, nepřepisujte ji. Přiřaďte proměnné `$resourceGroupName` jinou hodnotu a spusťte tento příkaz znovu.
3. Definujte proměnnou pro název datové továrny, kterou můžete později použít v příkazech prostředí PowerShell. Název musí začínat písmenem nebo číslicí a smí obsahovat jenom písmena, číslice a spojovníky (-).

    > [!IMPORTANT]
    >  Aktualizujte název datové továrny tak, aby byl globálně jedinečný. Například ADFTutorialFactorySP1127. 

    ```powershell
    $dataFactoryName = "ADFTutorialFactory"
    ```
1. Definujte proměnnou pro umístění datové továrny: 

    ```powershell
    $location = "East US"
    ```  
5. Pokud chcete vytvořit datovou továrnu, spusťte následující rutinu **Set-AzureRmDataFactoryV2**: 
    
    ```powershell       
    Set-AzureRmDataFactoryV2 -ResourceGroupName $resourceGroupName -Location "East US" -Name $dataFactoryName 
    ```

Je třeba počítat s následujícím:

* Název objektu pro vytváření dat Azure musí být globálně jedinečný. Pokud se zobrazí následující chyba, změňte název a zkuste to znovu.

    ```
    The specified Data Factory name 'ADFv2TutorialDataFactory' is already in use. Data Factory names must be globally unique.
    ```

* Pro vytvoření instancí Data Factory musí být uživatelský účet, který použijete pro přihlášení k Azure, členem rolí **přispěvatel** nebo **vlastník** nebo **správcem** předplatného Azure.
* Data Factory verze 2 v současné době umožňuje vytváření datových továren jenom v oblastech Východní USA, Východní USA 2 a Západní Evropa. Úložiště dat (Azure Storage, Azure SQL Database atd.) a výpočetní prostředí (HDInsight atd.) používané datovou továrnou mohou být v jiných oblastech.

## <a name="create-a-self-hosted-ir"></a>Vytvoření místního prostředí IR

V této části můžete vytvořit místní prostředí Integration Runtime a přidružit ho k místnímu počítači s databází SQL Serveru. Místní prostředí Integration Runtime místně je komponenta, která zkopíruje data z SQL Serveru ve vašem počítači do úložiště objektů blob v Azure. 

1. Vytvořte proměnnou pro název prostředí Integration Runtime. Použijte jedinečný název, který si poznamenejte. Použijete ho později v tomto kurzu. 

    ```powershell
   $integrationRuntimeName = "ADFTutorialIR"
    ```
1. Vytvořte místní prostředí Integration Runtime. 

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

3. Spusťte následující příkaz, který načte **ověřovací klíče** pro registraci místního prostředí Integration Runtime ve službě Data Factory v cloudu. 

   ```powershell
   Get-AzureRmDataFactoryV2IntegrationRuntimeKey -Name $integrationRuntimeName -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName | ConvertTo-Json
   ```

   Zde je ukázkový výstup:

   ```json
   {
       "AuthKey1":  "IR@0000000000-0000-0000-0000-000000000000@xy0@xy@xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx=",
       "AuthKey2":  "IR@0000000000-0000-0000-0000-000000000000@xy0@xy@yyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyy="
   }
   ```    
4. Pro registraci místního prostředí Integration Runtime, který nainstalujete na počítači v dalším kroku, zkopírujte jeden z klíčů (bez dvojitých uvozovek).  

## <a name="install-integration-runtime"></a>Instalace prostředí Integration Runtime
1. Na místním počítači s Windows [stáhněte](https://www.microsoft.com/download/details.aspx?id=39717) místní prostředí Integration Runtime a spusťte instalaci. 
2. Na stránce **Vítá vás průvodce instalací prostředí Microsoft Integration Runtime** klikněte na **Další**.  
3. Na stránce **Licenční smlouva s koncovým uživatelem (EULA)** přijměte podmínky a licenční smlouvu a klikněte na **Další**. 
4. Na stránce **Cílová složka** klikněte na **Další**. 
5. Na stránce **Připraveno k instalaci prostředí Microsoft Integration Runtime** klikněte na **Nainstalovat**. 
6. Pokud se zobrazí zpráva upozorňující na konfiguraci přechodu počítače do režimu spánku nebo hibernace, když se zrovna nepoužívá, klikněte na **OK**. 
7. Pokud se zobrazí okno **Možnosti napájení**, zavřete ho a přepněte do okna nastavení. 
8. Na stránce **Dokončení průvodce instalací prostředí Microsoft Integration Runtime** klikněte na **Dokončit**.
9. Na stránce **Registrace prostředí Integration Runtime (v místním prostředí)** vložte klíč, který jste uložili v předchozí části, a klikněte na **Zaregistrovat**. 

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
7. Teď otestujte připojení k databázi SQL Serveru.

    ![Karta Diagnostika](media/tutorial-hybrid-copy-powershell/config-manager-diagnostics-tab.png)   

    - V okně **Configuration Manager** přepněte na kartu **Diagnostika**.
    - V poli **Typ zdroje dat** vyberte **SqlServer**.
    - Zadejte název **serveru**.
    - Zadejte název **databáze**. 
    - Vyberte režim **ověřování**. 
    - Zadejte **uživatelské jméno**. 
    - Zadejte **heslo** pro toto uživatelské jméno.
    - Kliknutím na **Test** potvrďte, že Integration Runtime může připojit k SQL Serveru. Pokud je připojení úspěšné, zobrazí se zelená značka zaškrtnutí. V opačném případě se zobrazí chybová zpráva přidružená k tomuto selhání. Opravte všechny problémy a ověřte, že se Integration Runtime může připojit k SQL Serveru.
    - Poznamenejte si tyto hodnoty (typ ověřování, server, databáze, uživatel, heslo). Použijete je později v tomto kurzu. 
    
      
## <a name="create-linked-services"></a>Vytvoření propojených služeb
V datové továrně vytvořte propojené služby, abyste svá úložiště dat a výpočetní služby spojili s datovou továrnou. V tomto kurzu propojíte účet služby Azure Storage a místní SQL Server k úložišti dat. Propojené služby mají informace o připojení, které služba Data Factory používá pro připojení za běhu. 

### <a name="create-an-azure-storage-linked-service-destinationsink"></a>Vytvoření propojené služby Azure Storage (cíl/jímka)
V tomto kroku s datovou továrnou propojíte svůj účet služby Azure Storage.

1. Vytvořte soubor JSON s názvem **AzureStorageLinkedService.json** ve složce **C:\ADFv2Tutorial** s následujícím obsahem. Pokud složka ADFv2Tutorial ještě neexistuje, vytvořte ji.  

    > [!IMPORTANT]
    > Než soubor uložíte, položky &lt;accountName&gt; a &lt;accountKey&gt; nahraďte názvem svého **účtu úložiště Azure** a jeho klíčem. Poznamenali jste si je v rámci [požadavků](#get-storage-account-name-and-account-key).

   ```json
    {
        "properties": {
            "type": "AzureStorage",
            "typeProperties": {
                "connectionString": {
                    "type": "SecureString",
                    "value": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>;EndpointSuffix=core.windows.net"
                }
            }
        },
        "name": "AzureStorageLinkedService"
    }
   ```

    Pokud používáte Poznámkový blok, v dialogovém okně **Uložit jako** v poli **Uložit jako typ** vyberte **Všechny soubory**. Jinak se k souboru může přidat přípona `.txt`. Například, `AzureStorageLinkedService.json.txt`. Pokud soubor před otevřením v Poznámkovém bloku vytvoříte v Průzkumníku souborů, přípona `.txt` se možná nezobrazí, protože ve výchozím nastavení je nastavená možnost **Skrýt příponu souborů známých typů**. Než budete pokračovat k dalšímu kroku, odeberte příponu `.txt`. 
2. V **Azure PowerShellu** přejděte do složky **C:\ADFv2Tutorial**.

   Spuštěním rutiny **Set-AzureRmDataFactoryV2LinkedService** vytvořte propojenou službu **AzureStorageLinkedService**. 

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

    Pokud se zobrazí chyba Soubor nenalezen, spusťte příkaz `dir` a ověřte, že soubor existuje. Pokud má název souboru příponu `.txt` (například AzureStorageLinkedService.json.txt), odeberte ji a pak spusťte příkaz PowerShellu znovu. 

### <a name="create-and-encrypt-a-sql-server-linked-service-source"></a>Vytvoření a šifrování propojené služby SQL Serveru (zdroj)
V tomto kroku s datovou továrnou propojíte místní SQL Server.

1. Ve složce **C:\ADFv2Tutorial** vytvořte soubor JSON s názvem **SqlServerLinkedService.json** s následujícím obsahem: Vyberte správnou sekci na základě **ověřování**, které požíváte pro připojení k SQL Serveru.  

    > [!IMPORTANT]
    > Vyberte správnou sekci na základě **ověřování**, které požíváte pro připojení k SQL Serveru.

    **Pokud používáte ověřování SQL (sa), zkopírujte následující definici JSON:**

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
    **Pokud používáte ověřování Windows, zkopírujte následující definici JSON:**

    ```json
    {
        "properties": {
            "type": "SqlServer",
            "typeProperties": {
                "connectionString": {
                    "type": "SecureString",
                    "value": "Server=<server>;Database=<database>;Integrated Security=True"
                },
                "userName": "<user> or <domain>\\<user>",
                "password": {
                    "type": "SecureString",
                    "value": "<password>"
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
    > [!IMPORTANT]
    > - Vyberte správnou sekci na základě **ověřování**, které požíváte pro připojení k SQL Serveru.
    > - Položku **&lt;integration** **runtime** **name>** nahraďte názvem vašeho prostředí Integration Runtime.
    > - Před uložením tohoto souboru položky **&lt;servername>**, **&lt;databasename>**, **&lt;username>** a **&lt;password>** nahraďte odpovídajícími hodnotami pro SQL Server.
    > - Pokud v názvu uživatelského účtu nebo serveru potřebujete použít znak lomítko (`\`), použijte řídicí znak (`\`). Například, `mydomain\\myuser`. 

2. Pokud chcete šifrovat důvěrné osobní údaje (uživatelské jméno, heslo atd.), spusťte rutinu **New-AzureRmDataFactoryV2LinkedServiceEncryptedCredential**. Toto šifrování zajišťuje šifrování přihlašovacích údajů pomocí rozhraní Data Protection API. Zašifrované přihlašovací údaje jsou uložené místně v uzlu místního prostředí Integration Runtime (místní počítač). Výstupní datovou část je možné přesměrovat do jiného souboru JSON (v tomto případě encryptedLinkedService.json), který obsahuje zašifrované přihlašovací údaje.
    
   ```powershell
   New-AzureRmDataFactoryV2LinkedServiceEncryptedCredential -DataFactoryName $dataFactoryName -ResourceGroupName $ResourceGroupName -IntegrationRuntimeName $integrationRuntimeName -File ".\SQLServerLinkedService.json" > encryptedSQLServerLinkedService.json
   ```

3. Spusťte následující příkaz, který vytvoří **EncryptedSqlServerLinkedService**:

   ```powershell
   Set-AzureRmDataFactoryV2LinkedService -DataFactoryName $dataFactoryName -ResourceGroupName $ResourceGroupName -Name "EncryptedSqlServerLinkedService" -File ".\encryptedSqlServerLinkedService.json"
   ```


## <a name="create-datasets"></a>Vytvoření datových sad
V tomto kroku vytvoříte vstupní a výstupní datové sady, které představují vstupní a výstupní data pro operaci kopírování (místní databáze SQL Serveru => úložiště objektů blob v Azure).

### <a name="create-a-dataset-for-source-sql-database"></a>Vytvoření datové sady pro zdrojovou databázi SQL Database
V tomto kroku definujete datovou sadu, která představuje data v databázi SQL Serveru. Tato datová sada je typu **SqlServerTable**. Odkazuje na **propojenou službu SQL Serveru**, kterou jste vytvořili v předchozím kroku. Propojená služba má **informace o připojení**, které služba Data Factory používá pro připojení k SQL Serveru za běhu. Tato datová sada určuje **tabulku SQL** v databázi, která obsahuje data. V tomto kurzu zdrojová data obsahuje tabulka `emp`. 

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
V tomto kroku definujete datovou sadu představující data, která se mají zkopírovat do služby Azure Blob Storage. Tato datová sada je typu **AzureBlob**. Odkazuje na **propojenou službu Azure Storage**, kterou jste v tomto kurzu vytvořili dřív. Propojená služba má **informace o připojení**, které služba Data Factory používá pro připojení k účtu služby Azure Storage za běhu. Tato **datová sada** určuje **složku** v úložišti Azure, do které se kopírují data z databáze SQL Serveru. V tomto kurzu je touto složkou `adftutorial/fromonprem`, kde `adftutorial` je kontejner objektů blob a `fromonprem` je složka. 

1. Ve složce **C:\ADFv2Tutorial** vytvořte soubor JSON s názvem **AzureBlobDataset.json** s následujícím obsahem:

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

## <a name="create-a-pipeline"></a>Vytvoření kanálu
V tomto kurzu pomocí aktivity kopírování vytvoříte kanál. Aktivita kopírování používá **SqlServerDataset** jako vstupní datovou sadu a **AzureBlobDataset** jako výstupní datovou sadu. Typ zdroje je nastavený na **SqlSource** a typ jímky je nastavený na **BlobSink**.

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


## <a name="create-a-pipeline-run"></a>Vytvoření spuštění kanálu
Zahajte spuštění pro kanál SQLServerToBlobPipeline a zaznamenejte ID spuštění kanálu pro budoucí monitorování.

```powershell
$runId = Invoke-AzureRmDataFactoryV2Pipeline -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName -PipelineName 'SQLServerToBlobPipeline'
```

## <a name="monitor-the-pipeline-run"></a>Monitorování spuštění kanálu

1. Spusťte následující skript, který bude nepřetržitě kontrolovat stav spuštění kanálu **SQLServerToBlobPipeline** a vytiskne konečný výsledek. Zkopírujte/vložte následující skript v okně PowerShellu a stiskněte klávesu ENTER.

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

3. Spuštěním následujícího příkazu můžete získat ID spuštění kanálu **SQLServerToBlobPipeline** a zkontrolovat podrobné výsledky spuštění aktivit: 

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
Kanál v kontejneru objektů blob `adftutorial` automaticky vytvoří výstupní složku `fromonprem`. Zkontrolujte, že výstupní složka obsahuje soubor **dbo.emp.txt**. 

1. Na webu Azure Portal na stránce kontejneru **adftutorial** klikněte na **Obnovit**. Zobrazí se výstupní složka.

    ![Vytvořená výstupní složka](media/tutorial-hybrid-copy-powershell/fromonprem-folder.png)
2. V seznamu složek klikněte na `fromonprem`. 
3. Potvrďte, že se zobrazuje soubor s názvem `dbo.emp.txt`.

    ![Výstupní soubor](media/tutorial-hybrid-copy-powershell/fromonprem-file.png)


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
