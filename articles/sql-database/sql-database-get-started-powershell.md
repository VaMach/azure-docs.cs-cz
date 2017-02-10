---
title: "Powershell: Začínáme s Azure SQL Database | Dokumentace Microsoftu"
description: "Naučíte se vytvářet logický server SQL Database, pravidlo brány firewall na úrovni serveru a databáze pomocí Powershellu. Naučíte se taky dotazovat pomocí PowerShellu."
keywords: "vytvoření nové databáze sql, nastavení databáze"
services: sql-database
documentationcenter: 
author: stevestein
manager: jhubbard
editor: cgronlun
ms.assetid: 7d99869b-cec5-4583-8c1c-4c663f4afd4d
ms.service: sql-database
ms.custom: single databases
ms.devlang: NA
ms.topic: hero-article
ms.tgt_pltfrm: powershell
ms.workload: data-management
ms.date: 12/09/2016
ms.author: sstein
translationtype: Human Translation
ms.sourcegitcommit: 2b55b6b4475abdbc1985d8ac370b3b612b77eb0e
ms.openlocfilehash: 62f29382a2b29c7e5b06126b0ab60c7136186103


---

# <a name="sql-database-tutorial-get-started-with-azure-sql-database-servers-databases-and-firewall-rules-using-powershell"></a>Kurz k SQL Database: Začínáme se servery, databázemi a pravidly brány firewall služby Azure SQL Database pomocí PowerShellu

V tomto úvodním kurzu se naučíte, jak pomocí prostředí PowerShell provádět tyto akce:

* Vytvoření nové skupiny prostředků Azure
* Vytvoření logického SQL serveru Azure
* Zobrazení vlastností serveru SQL Azure
* Vytvoření pravidla brány firewall na úrovni serveru
* Vytvoření ukázkové databáze AdventureWorksLT jako samostatné databáze
* Zobrazení vlastností ukázkové databáze AdventureWorksLT
* Vytvoření prázdné jedné databáze

V tomto kurzu provedete také následující:

* Připojení k logickému serveru a jeho hlavní databázi
* Zobrazení vlastností hlavní databáze
* Připojení k ukázkové databázi
* Zobrazení vlastností uživatelské databáze

Po dokončení tohoto kurzu budete mít ukázkovou databázi a prázdnou databázi, která je spuštěná ve skupině prostředků Azure a připojená k logickému serveru. Dál budete mít nakonfigurované pravidlo brány firewall na úrovni serveru, které objektu zabezpečení na úrovni serveru umožňuje přihlášení k serveru ze zadaná IP adresy (nebo rozsahu IP adres). 

**Časový odhad:** Tento kurz trvá přibližně 30 minut (za předpokladu, že už máte splněné požadavky).


> [!TIP]
> Stejné úlohy můžete v úvodním kurzu provádět i pomocí webu [Azure Portal](sql-database-get-started.md).
>


## <a name="prerequisites"></a>Požadavky

* Potřebujete mít účet Azure. Můžete si [zdarma otevřít účet Azure](/pricing/free-trial/?WT.mc_id=A261C142F) nebo [aktivovat výhody pro předplatitele sady Visual Studio](/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F). 

* Musíte být přihlášeni pomocí účtu, který je členem role přispěvatele nebo vlastníka předplatného. Další informace o řízení přístupu na základě role (RBAC) najdete v tématu [Začínáme se správou přístupu na webu Azure Portal](../active-directory/role-based-access-control-what-is.md).

* Musíte mít v Azure Blob Storage soubor .bacpac ukázkové databáze AdventureWorksLT

### <a name="download-the-adventureworkslt-sample-database-bacpac-file-and-save-it-in-azure-blob-storage"></a>Stažení souboru .bacpac ukázkové databáze AdventureWorksLT a jeho uložení do Azure Blob Storage

V tomto kurzu se vytvoří nová databáze AdventureWorksLT importováním souboru .bacpac ze služby Azure Storage. Prvním krokem je získání kopie souboru AdventureWorksLT.bacpac a její nahrání do úložiště objektů blob.
Následující kroky připraví ukázkovou databázi k importu:

1. [Stáhněte soubor AdventureWorksLT.bacpac](https://sqldbbacpacs.blob.core.windows.net/bacpacs/AdventureWorksLT.bacpac) a uložte jej s příponou .bacpac.
2. [Vytvořte účet úložiště](../storage/storage-create-storage-account.md#create-a-storage-account) – k vytvoření účtu úložiště můžete použít výchozí nastavení.
3. Vytvořte nový **Kontejner** tak, že přejdete do účtu úložiště, vyberete **Objekty blob** a kliknete na **+Kontejner**.
4. Nahrajte soubor .bacpac do kontejneru objektů blob ve svém účtu úložiště. Můžete k tomu použít tlačítko **Nahrát** v horní části stránky kontejneru nebo [použijte AzCopy](../storage/storage-use-azcopy.md#blob-upload). 
5. Po uložení souboru AdventureWorksLT.bacpac budete potřebovat adresu URL a klíč účtu úložiště, které později v tomto kurzu použijete ve fragmentu kódu pro import. 
   * Vyberte soubor bacpac a zkopírujte adresu URL. Bude podobná jako https://{název_účtu_úložiště}.blob.core.windows.net/{název_kontejneru}/AdventureWorksLT.bacpac. Na stránce účtu úložiště klikněte na **Přístupové klíče** a zkopírujte hodnotu **key1**.


[!INCLUDE [Start your PowerShell session](../../includes/sql-database-powershell.md)]


## <a name="create-a-new-logical-sql-server-using-azure-powershell"></a>Vytvoření nového logického serveru SQL pomocí prostředí Azure PowerShell

Potřebujete skupinu prostředků, která bude obsahovat server. Prvním krokem je tedy vytvoření nové skupiny prostředků a serveru (rutiny [New-AzureRmResourceGroup](https://docs.microsoft.com/powershell/resourcemanager/azurerm.resources/v3.3.0/new-azurermresourcegroup), [New-AzureRmSqlServer](https://docs.microsoft.com/powershell/resourcemanager/azurerm.sql/v2.3.0/new-azurermsqlserver)), nebo získání odkazu na existující skupinu prostředků a server (rutiny [Get-AzureRmResourceGroup](https://docs.microsoft.com/powershell/resourcemanager/azurerm.resources/v3.3.0/get-azurermresourcegroup), [Get-AzureRmSqlServer](https://docs.microsoft.com/powershell/resourcemanager/azurerm.sql/v2.3.0/get-azurermsqlserver)).
Následující fragmenty kódu vytvoří skupinu prostředků a server SQL Azure, pokud ještě neexistují:

Seznam platných umístění Azure a formát řetězce najdete v části [Fragmenty pomocných rutin](#helper-snippets) dále v tomto kurzu.
```
# Create new, or get existing resource group
############################################

$resourceGroupName = "{resource-group-name}"
$resourceGroupLocation = "{resource-group-location}"

$myResourceGroup = Get-AzureRmResourceGroup -Name $resourceGroupName -ea SilentlyContinue

if(!$myResourceGroup)
{
   Write-Output "Creating resource group: $resourceGroupName"
   $myResourceGroup = New-AzureRmResourceGroup -Name $resourceGroupName -Location $resourceGroupLocation
}
else
{
   Write-Output "Resource group $resourceGroupName already exists:"
}
$myResourceGroup



# Create a new, or get existing server
######################################

$serverName = "{server-name}"
$serverVersion = "12.0"
$serverLocation = $resourceGroupLocation
$serverResourceGroupName = $resourceGroupName

$serverAdmin = "{server-admin}"
$serverAdminPassword = "{server-admin-password}"

$securePassword = ConvertTo-SecureString -String $serverAdminPassword -AsPlainText -Force
$serverCreds = New-Object -TypeName System.Management.Automation.PSCredential -ArgumentList $serverAdmin, $securePassword

$myServer = Get-AzureRmSqlServer -ServerName $serverName -ResourceGroupName $serverResourceGroupName -ea SilentlyContinue
if(!$myServer)
{
   Write-Output "Creating SQL server: $serverName"
   $myServer = New-AzureRmSqlServer -ResourceGroupName $serverResourceGroupName -ServerName $serverName -Location $serverLocation -ServerVersion $serverVersion -SqlAdministratorCredentials $serverCreds
}
else
{
   Write-Output "SQL server $serverName already exists:"
}
$myServer

```


## <a name="view-the-logical-sql-server-properties-using-azure-powershell"></a>Zobrazení vlastností logického serveru SQL pomocí prostředí Azure PowerShell

```
#$serverResourceGroupName = "{resource-group-name}"
#$serverName = "{server-name}"

$myServer = Get-AzureRmSqlServer -ServerName $serverName -ResourceGroupName $serverResourceGroupName 

Write-Host "Server name: " $myServer.ServerName
Write-Host "Fully qualified server name: $serverName.database.windows.net"
Write-Host "Server location: " $myServer.Location
Write-Host "Server version: " $myServer.ServerVersion
Write-Host "Server administrator login: " $myServer.SqlAdministratorLogin
```


## <a name="create-a-server-level-firewall-rule-using-azure-powershell"></a>Vytvoření pravidla brány firewall na úrovni serveru pomocí prostředí Azure PowerShell

Pokud chcete nastavit pravidlo brány firewall, musíte znát svou veřejnou IP adresu. Veřejnou IP adresu zjistíte pomocí prohlížeče podle vašeho výběru (zadejte dotaz „jaká je moje IP adresa“). Podrobnosti najdete v tématu o [pravidlech brány firewall](sql-database-firewall-configure.md).

Následující kód pomocí rutin [Get-AzureRmSqlServerFirewallRule](https://docs.microsoft.com/powershell/resourcemanager/azurerm.sql/v2.3.0/get-azurermsqlserverfirewallrule) a [New-AzureRmSqlServerFirewallRule](https://docs.microsoft.com/powershell/resourcemanager/azurerm.sql/v2.3.0/new-azurermsqlserverfirewallrule) získá odkaz na pravidlo, nebo vytvoří nové. Pokud pravidlo již existuje, tento fragment kódu na něj pouze získá odkaz a neprovede aktualizaci počáteční ani koncové IP adresy. Klauzuli **else** můžete kdykoli upravit a použít v ní rutinu [Set-AzureRmSqlServerFirewallRule](https://docs.microsoft.com/powershell/resourcemanager/azurerm.sql/v2.3.0/set-azurermsqlserverfirewallrule), která poskytuje funkce vytvoření nebo aktualizace.

> [!NOTE] 
> Bránu firewall služby SQL Database na serveru můžete otevřít pro jednu IP adresu nebo pro celý rozsah adres. Otevření brány firewall umožňuje uživatelům a správcům SQL přihlásit se k jakékoli databázi na serveru, ke kterému mají platné přihlašovací údaje.
>

```
#$serverName = "{server-name}"
#$serverResourceGroupName = "{resource-group-name}"
$serverFirewallRuleName = "{server-firewall-rule-name}"
$serverFirewallStartIp = "{server-firewall-rule-startIp}"
$serverFirewallEndIp = "{server-firewall-rule-endIp}"

$myFirewallRule = Get-AzureRmSqlServerFirewallRule -FirewallRuleName $serverFirewallRuleName -ServerName $serverName -ResourceGroupName $serverResourceGroupName -ea SilentlyContinue

if(!$myFirewallRule)
{
   Write-host "Creating server firewall rule: $serverFirewallRuleName"
   $myFirewallRule = New-AzureRmSqlServerFirewallRule -ResourceGroupName $serverResourceGroupName -ServerName $serverName -FirewallRuleName $serverFirewallRuleName -StartIpAddress $serverFirewallStartIp -EndIpAddress $serverFirewallEndIp
}
else
{
   Write-host "Server firewall rule $serverFirewallRuleName already exists:"
}
$myFirewallRule
```


## <a name="connect-to-sql-server-using-azure-powershell"></a>Připojení k serveru SQL pomocí prostředí Azure PowerShell

Spusťme rychlý dotaz na hlavní databázi pro ověření, že se můžeme připojit k serveru. Následující fragment kódu se pomocí [zprostředkovatele rozhraní .NET Framework pro SQL Server (System.Data.SqlClient)](https://msdn.microsoft.com/library/system.data.sqlclient(v=vs.110).aspx) připojí k hlavní databázi serveru a odešle na ní dotaz. Na základě hodnot, které jsme použili v předchozích fragmentech kódu, vytvoří připojovací řetězec. Zástupné symboly nahraďte přihlašovacím jménem a heslem správce serveru SQL, které jste v předchozích krocích použili k vytvoření serveru.


```
#$serverName = "{server-name}"
#$serverAdmin = "{server-admin}"
#$serverAdminPassword = "{server-admin-password}"
$databaseName = "master"

$connectionString = "Server=tcp:" + $serverName + ".database.windows.net" + ",1433;Initial Catalog=" + $databaseName + ";Persist Security Info=False;User ID=$serverAdmin;Password=$serverAdminPassword" + ";MultipleActiveResultSets=False;Encrypt=True;TrustServerCertificate=False;Connection Timeout=30;"



$connection = New-Object System.Data.SqlClient.SqlConnection
$connection.ConnectionString = $connectionString
$connection.Open()
$command = New-Object System.Data.SQLClient.SQLCommand("select * from sys.objects", $connection)

$command.Connection = $connection
$reader = $command.ExecuteReader()


$sysObjects = ""
while ($reader.Read()) {
    $sysObjects += $reader["name"] + "`n"
}
$sysObjects

$connection.Close()
```


## <a name="create-new-adventureworkslt-sample-database-using-azure-powershell"></a>Vytvoření nové ukázkové databáze AdventureWorksLT pomocí prostředí Azure PowerShell

Následující fragment kódu pomocí rutiny [New-AzureRmSqlDatabaseImport](https://docs.microsoft.com/powershell/resourcemanager/azurerm.sql/v2.3.0/new-azurermsqldatabaseimport) naimportuje soubor bacpac ukázkové databáze AdventureWorksLT. Soubor bacpac se nachází v Azure Blob Storage. Po spuštění rutiny importu můžete sledovat průběh operace importu pomocí rutiny [Get-AzureRmSqlDatabaseImportExportStatus](https://docs.microsoft.com/powershell/resourcemanager/azurerm.sql/v2.3.0/get-azurermsqldatabaseimportexportstatus).
$storageUri je vlastnost adresy URL souboru bacpac, který jste nahráli na portál v předchozích krocích, a měla by být podobná jako https://{účet_úložiště}.blob.core.windows.net/{kontejner}/AdventureWorksLT.bacpac.

```
#$resourceGroupName = "{resource-group-name}"
#$serverName = "{server-name}"

$databaseName = "AdventureWorksLT"
$databaseEdition = "Basic"
$databaseServiceLevel = "Basic"

$storageKeyType = "StorageAccessKey"
$storageUri = "{storage-uri}" # URL of bacpac file you uploaded to your storage account
$storageKey = "{storage-key}" # key1 in the Access keys setting of your storage account

$importRequest = New-AzureRmSqlDatabaseImport -ResourceGroupName $resourceGroupName -ServerName $serverName -DatabaseName $databaseName -StorageKeytype $storageKeyType -StorageKey $storageKey -StorageUri $storageUri -AdministratorLogin $serverAdmin -AdministratorLoginPassword $securePassword -Edition $databaseEdition -ServiceObjectiveName $databaseServiceLevel -DatabaseMaxSizeBytes 5000000


Do {
     $importStatus = Get-AzureRmSqlDatabaseImportExportStatus -OperationStatusLink $importRequest.OperationStatusLink
     Write-host "Importing database..." $importStatus.StatusMessage
     Start-Sleep -Seconds 30
     $importStatus.Status
   }
   until ($importStatus.Status -eq "Succeeded")
$importStatus
```



## <a name="view-database-properties-using-azure-powershell"></a>Zobrazení vlastností databáze pomocí prostředí Azure PowerShell

Po vytvoření databáze zobrazíme některé její vlastnosti.

```
#$resourceGroupName = "{resource-group-name}"
#$serverName = "{server-name}"
#$databaseName = "{database-name}"

$myDatabase = Get-AzureRmSqlDatabase -ResourceGroupName $resourceGroupName -ServerName $serverName -DatabaseName $databaseName


Write-Host "Database name: " $myDatabase.DatabaseName
Write-Host "Server name: " $myDatabase.ServerName
Write-Host "Creation date: " $myDatabase.CreationDate
Write-Host "Database edition: " $myDatabase.Edition
Write-Host "Database performance level: " $myDatabase.CurrentServiceObjectiveName
Write-Host "Database status: " $myDatabase.Status
```

## <a name="connect-and-query-the-sample-database-using-azure-powershell"></a>Připojení k ukázkové databázi a její dotazování pomocí prostředí Azure PowerShell

Spusťme rychlý dotaz na databázi AdventureWorksLT pro ověření, že se k ní můžeme připojit. Následující fragment kódu se pomocí [zprostředkovatele rozhraní .NET Framework pro SQL Server (System.Data.SqlClient)](https://msdn.microsoft.com/library/system.data.sqlclient(v=vs.110).aspx) připojí k databázi a odešle na ní dotaz. Na základě hodnot, které jsme použili v předchozích fragmentech kódu, vytvoří připojovací řetězec. Zástupný symbol nahraďte heslem správce serveru SQL.

```
#$serverName = {server-name}
#$serverAdmin = "{server-admin}"
#$serverAdminPassword = "{server-admin-password}"
#$databaseName = {database-name}

$connectionString = "Server=tcp:" + $serverName + ".database.windows.net" + ",1433;Initial Catalog=" + $databaseName + ";Persist Security Info=False;User ID=$serverAdmin;Password=$serverAdminPassword" + ";MultipleActiveResultSets=False;Encrypt=True;TrustServerCertificate=False;Connection Timeout=30;"


$connection = New-Object System.Data.SqlClient.SqlConnection
$connection.ConnectionString = $connectionString
$connection.Open()
$command = New-Object System.Data.SQLClient.SQLCommand("select * from sys.objects", $connection)

$command.Connection = $connection
$reader = $command.ExecuteReader()


$sysObjects = ""
while ($reader.Read()) {
    $sysObjects += $reader["name"] + "`n"
}
$sysObjects

$connection.Close()
```

## <a name="create-a-new-blank-database-using-azure-powershell"></a>Vytvoření nové prázdné databáze pomocí prostředí Azure PowerShell

```
#$resourceGroupName = {resource-group-name}
#$serverName = {server-name}

$blankDatabaseName = "blankdb"
$blankDatabaseEdition = "Basic"
$blankDatabaseServiceLevel = "Basic"


$myBlankDatabase = New-AzureRmSqlDatabase -DatabaseName $blankDatabaseName -ServerName $serverName -ResourceGroupName $resourceGroupName -Edition $blankDatabaseEdition -RequestedServiceObjectiveName $blankDatabaseServiceLevel
$myBlankDatabase
```


## <a name="complete-azure-powershell-script-to-create-a-server-firewall-rule-and-database"></a>Dokončení skriptu prostředí Azure PowerShell pro vytvoření serveru, pravidla brány firewall a databáze



```
# Sign in to Azure and set the subscription to work with
########################################################

$SubscriptionId = "{subscription-id}"

Add-AzureRmAccount
Set-AzureRmContext -SubscriptionId $SubscriptionId

# User variables
################

$myResourceGroupName = "{resource-group-name}"
$myResourceGroupLocation = "{resource-group-location}"

$myServerName = "{server-name}"
$myServerVersion = "12.0"
$myServerLocation = $myResourceGroupLocation
$myServerResourceGroupName = $myResourceGroupName
$myServerAdmin = "{server-admin}"
$myServerAdminPassword = "{server-admin-password}" 

$myServerFirewallRuleName = "{server-firewall-rule-name}"
$myServerFirewallStartIp = "{start-ip}"
$myServerFirewallEndIp = "{end-ip}"

$myDatabaseName = "AdventureWorksLT"
$myDatabaseEdition = "Basic"
$myDatabaseServiceLevel = "Basic"

$myStorageKeyType = "StorageAccessKey"
# Get these values from your Azure storage account:
$myStorageUri = "{http://your-storage-account.blob.core.windows.net/your-container/AdventureWorksLT.bacpac}"
$myStorageKey = "{your-storage-key}"


# Create new, or get existing resource group
############################################

$resourceGroupName = $myResourceGroupName
$resourceGroupLocation = $myResourceGroupLocation

$myResourceGroup = Get-AzureRmResourceGroup -Name $resourceGroupName -ea SilentlyContinue

if(!$myResourceGroup)
{
   Write-host "Creating resource group: $resourceGroupName"
   $myResourceGroup = New-AzureRmResourceGroup -Name $resourceGroupName -Location $resourceGroupLocation
}
else
{
   Write-host "Resource group $resourceGroupName already exists:"
}
$myResourceGroup


# Create a new, or get existing server
######################################

$serverName = $myServerName
$serverVersion = $myServerVersion
$serverLocation = $myServerLocation
$serverResourceGroupName = $myServerResourceGroupName

$serverAdmin = $myServerAdmin
$serverAdminPassword = $myServerAdminPassword

$securePassword = ConvertTo-SecureString -String $serverAdminPassword -AsPlainText -Force
$serverCreds = New-Object -TypeName System.Management.Automation.PSCredential -ArgumentList $serverAdmin, $securePassword

$myServer = Get-AzureRmSqlServer -ServerName $serverName -ResourceGroupName $serverResourceGroupName -ea SilentlyContinue
if(!$myServer)
{
   Write-host "Creating SQL server: $serverName"
   $myServer = New-AzureRmSqlServer -ResourceGroupName $serverResourceGroupName -ServerName $serverName -Location $serverLocation -ServerVersion $serverVersion -SqlAdministratorCredentials $serverCreds
}
else
{
   Write-host "SQL server $serverName already exists:"
}
$myServer


# View server properties
##########################

$resourceGroupName = $myResourceGroupName
$serverName = $myServerName

$myServer = Get-AzureRmSqlServer -ServerName $serverName -ResourceGroupName $serverResourceGroupName 

Write-Host "Server name: " $myServer.ServerName
Write-Host "Fully qualified server name: $serverName.database.windows.net"
Write-Host "Server location: " $myServer.Location
Write-Host "Server version: " $myServer.ServerVersion
Write-Host "Server administrator login: " $myServer.SqlAdministratorLogin


# Create or update server firewall rule
#######################################

$serverFirewallRuleName = $myServerFirewallRuleName
$serverFirewallStartIp = $myServerFirewallStartIp
$serverFirewallEndIp = $myServerFirewallEndIp

$myFirewallRule = Get-AzureRmSqlServerFirewallRule -FirewallRuleName $serverFirewallRuleName -ServerName $serverName -ResourceGroupName $serverResourceGroupName -ea SilentlyContinue

if(!$myFirewallRule)
{
   Write-host "Creating server firewall rule: $serverFirewallRuleName"
   $myFirewallRule = New-AzureRmSqlServerFirewallRule -ResourceGroupName $serverResourceGroupName -ServerName $serverName -FirewallRuleName $serverFirewallRuleName -StartIpAddress $serverFirewallStartIp -EndIpAddress $serverFirewallEndIp
}
else
{
   Write-host "Server firewall rule $serverFirewallRuleName already exists:"
}
$myFirewallRule


# Connect to the server and master database
###########################################
$databaseName = "master"

$connectionString = "Server=tcp:" + $serverName + ".database.windows.net" + ",1433;Initial Catalog=" + $databaseName + ";Persist Security Info=False;User ID=" + $myServer.SqlAdministratorLogin + ";Password=" + $myServerAdminPassword + ";MultipleActiveResultSets=False;Encrypt=True;TrustServerCertificate=False;Connection Timeout=30;"


$connection = New-Object System.Data.SqlClient.SqlConnection
$connection.ConnectionString = $connectionString
$connection.Open()
$command = New-Object System.Data.SQLClient.SQLCommand("select * from sys.objects", $connection)

$command.Connection = $connection
$reader = $command.ExecuteReader()

$sysObjects = ""
while ($reader.Read()) {
    $sysObjects += $reader["name"] + "`n"
}
$sysObjects

$connection.Close()


# Create the AdventureWorksLT database from a bacpac
####################################################

$resourceGroupName = $myResourceGroupName
$serverName = $myServerName

$databaseName = $myDatabaseName
$databaseEdition = $myDatabaseEdition
$databaseServiceLevel = $myDatabaseServiceLevel

$storageKeyType = $myStorageKeyType
$storageUri = $myStorageUri
$storageKey = $myStorageKey

$importRequest = New-AzureRmSqlDatabaseImport -ResourceGroupName $resourceGroupName -ServerName $serverName -DatabaseName $databaseName -StorageKeytype $storageKeyType -StorageKey $storageKey -StorageUri $storageUri -AdministratorLogin $serverAdmin -AdministratorLoginPassword $securePassword -Edition $databaseEdition -ServiceObjectiveName $databaseServiceLevel -DatabaseMaxSizeBytes 5000000

Do {
     $importStatus = Get-AzureRmSqlDatabaseImportExportStatus -OperationStatusLink $importRequest.OperationStatusLink
     Write-host "Importing database..." $importStatus.StatusMessage
     Start-Sleep -Seconds 30
     $importStatus.Status
   }
   until ($importStatus.Status -eq "Succeeded")
$importStatus


# View database properties
##########################

$resourceGroupName = $myResourceGroupName
$serverName = $myServerName
$databaseName = $myDatabaseName

$myDatabase = Get-AzureRmSqlDatabase -ResourceGroupName $resourceGroupName -ServerName $serverName -DatabaseName $databaseName

Write-Host "Database name: " $myDatabase.DatabaseName
Write-Host "Server name: " $myDatabase.ServerName
Write-Host "Creation date: " $myDatabase.CreationDate
Write-Host "Database edition: " $myDatabase.Edition
Write-Host "Database performance level: " $myDatabase.CurrentServiceObjectiveName
Write-Host "Database status: " $myDatabase.Status


# Query the database
####################

$connectionString = "Server=tcp:" + $serverName + ".database.windows.net" + ",1433;Initial Catalog=" + $databaseName + ";Persist Security Info=False;User ID=" + $myServer.SqlAdministratorLogin + ";Password=" + $myServerAdminPassword + ";MultipleActiveResultSets=False;Encrypt=True;TrustServerCertificate=False;Connection Timeout=30;"

$connection = New-Object System.Data.SqlClient.SqlConnection
$connection.ConnectionString = $connectionString
$connection.Open()
$command = New-Object System.Data.SQLClient.SQLCommand("select * from sys.objects", $connection)

$command.Connection = $connection
$reader = $command.ExecuteReader()

$sysObjects = ""
while ($reader.Read()) {
    $sysObjects += $reader["name"] + "`n"
}
$sysObjects

$connection.Close()


# Create a blank database
#########################

$blankDatabaseName = "blankdb"
$blankDatabaseEdition = "Basic"
$blankDatabaseServiceLevel = "Basic"


$myBlankDatabase = New-AzureRmSqlDatabase -DatabaseName $blankDatabaseName -ServerName $serverName -ResourceGroupName $resourceGroupName -Edition $blankDatabaseEdition -RequestedServiceObjectiveName $blankDatabaseServiceLevel
$myBlankDatabase
```



> [!TIP]
> Během výuky můžete ušetřit tím, že odstraníte databáze, které nepoužíváte. Databáze edice Basic můžete obnovit do sedmi dnů. Neodstraňujte ale server. Pokud to uděláte, nebude možné obnovit server ani žádnou z jeho odstraněných databází.

## <a name="helper-snippets"></a>Fragmenty pomocných rutin

```
# Get a list of Azure regions where you can create SQL resources

$sqlRegions = (Get-AzureRmLocation | Where-Object { $_.Providers -eq "Microsoft.Sql" })
foreach ($region in $sqlRegions)
{
   $region.Location
}

# Clean up
# Delete a resource group (and all contained resources)
Remove-AzureRmResourceGroup -Name {resource-group-name}
```

> [!TIP]
> Během výuky můžete ušetřit tím, že odstraníte databáze, které nepoužíváte. Databáze edice Basic můžete obnovit do sedmi dnů. Neodstraňujte ale server. Pokud to uděláte, nebude možné obnovit server ani žádnou z jeho odstraněných databází.
>

## <a name="next-steps"></a>Další kroky
Právě jste dokončili tento první úvodní kurz a vytvořili jste databázi s ukázkovými daty. K dispozici je celá řada dalších kurzů, které staví na tom, co jste se v tomto kurzu naučili. 

* Pokud se chcete začít seznamovat se zabezpečením Azure SQL Database, přečtěte si téma [Začínáme se zabezpečením](sql-database-get-started-security.md).
* Pokud znáte Excel, zjistěte, jak se [připojit k databázi SQL v Azure pomocí aplikace Excel](sql-database-connect-excel.md).
* Jste-li připraveni na psaní kódu, zvolte si programovací jazyk v tématu [Knihovny pro připojení ke službě SQL Database a systému SQL Server](sql-database-libraries.md).
* Pokud chcete do Azure přesunout databáze z místního systému SQL Server, přečtěte si téma [Migrace databáze do služby SQL Database](sql-database-cloud-migrate.md).
* Chcete-li do nové tabulky načíst data ze souboru CSV pomocí nástroje příkazového řádku BCP, více zjistíte v tématu [Načtení dat do služby SQL Database ze souboru CSV pomocí BCP](sql-database-load-from-csv-with-bcp.md).
* Pokud chcete začít vytvářet tabulky a další objekty, přečtěte si v článku [Vytvoření tabulky](https://msdn.microsoft.com/library/ms365315.aspx) téma věnované postupu při vytváření tabulek.

## <a name="additional-resources"></a>Další zdroje
[Co je SQL Database?](sql-database-technical-overview.md)


<!--HONumber=Jan17_HO1-->


