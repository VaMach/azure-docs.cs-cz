---
title: "Používání databáze SQL v Azure zásobníku | Microsoft Docs"
description: "Zjistěte, jak můžete nasadit databází SQL jako služba na Azure zásobníku a rychlé kroky k nasazení adaptér zprostředkovatele prostředků SQL serveru"
services: azure-stack
documentationCenter: 
author: JeffGoldner
manager: bradleyb
editor: 
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/15/2017
ms.author: JeffGo
ms.openlocfilehash: 4d6683a1a80dfdccdc5d46e9bac095a0d9f4d3e1
ms.sourcegitcommit: d6984ef8cc057423ff81efb4645af9d0b902f843
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/05/2018
---
# <a name="use-sql-databases-on-microsoft-azure-stack"></a>Databáze SQL pro použití v zásobníku Microsoft Azure

*Platí pro: Azure zásobníku integrované systémy a Azure zásobníku Development Kit*

Používat adaptér zprostředkovatele prostředků systému SQL Server jako službu vystavit databází SQL [zásobník Azure](azure-stack-poc.md). Po instalaci poskytovatele prostředků a připojte ho k jedné nebo více instancí systému SQL Server, můžete vytvořit vás a uživatele:
- databáze pro nativní cloudové aplikace
- weby, které jsou založené na SQL
- úlohy, které jsou založené na SQL nemusí pokaždé, když zřízení virtuálního počítače (VM) který je hostitelem systému SQL Server.

Zprostředkovatel prostředků nepodporuje všechny možnosti správy databáze z [Azure SQL Database](https://azure.microsoft.com/services/sql-database/). Například fondů elastické databáze a schopnost automaticky vytočit výkonu databáze nahoru či dolů nejsou k dispozici. Však nepodporuje Podpora zprostředkovatele prostředků podobné vytvářet, číst, aktualizovat a odstranit operace. Rozhraní API není kompatibilní s databázi SQL.

## <a name="sql-resource-provider-adapter-architecture"></a>Architektura adaptér zprostředkovatele prostředků SQL
Zprostředkovatel prostředků se skládá ze tří součástí:

- **Adaptér pro poskytovatele prostředků SQL virtuálních počítačů**, což je virtuální počítač s Windows běží služby poskytovatele.
- **Zprostředkovatel prostředků samotné**, která zpracovává požadavky pro zřizování a zpřístupňuje databáze prostředků.
- **Servery, které jsou hostiteli systému SQL Server**, které poskytují kapacitu pro databáze, se nazývají hostování servery.

Musíte vytvořit jeden (nebo více) serverů SQL nebo poskytovat přístup k externí instance SQL.

## <a name="deploy-the-resource-provider"></a>Nasazení poskytovatele prostředků

1. Pokud jste tak již neučinili, zaregistrujte vaší development kit a stáhnout bitovou kopii jádro systému Windows Server 2016 Datacenter ke stažení přes správu Marketplace. Je nutné použít bitovou kopii jádro systému Windows Server 2016. Skript můžete také použít k vytvoření [bitové kopie systému Windows Server 2016](https://docs.microsoft.com/azure/azure-stack/azure-stack-add-default-image) -je nutné vybrat možnost jádra. Modul runtime rozhraní .NET 3.5 už nepotřebujete.

2. Přihlaste se k hostiteli, který přístup privilegované koncový bod virtuálního počítače.

    a. Instalace Azure zásobníku Development Kit (ASDK) Přihlaste se na fyzickém hostiteli.

    b. V systémech s více uzly hostitele musí být systém, kterým může přistupovat privilegované koncový bod. 
    
    >[!NOTE]
    > Systém, kde je spuštěn skript *musí* být systém Windows 10 nebo Windows Server 2016 na nejnovější verzi modulu runtime rozhraní .NET nainstalované. Jinak se instalace nepovede. Hostitel ASDK splňuje tato kritéria.


3. Zprostředkovatel prostředků SQL binární stažení a spuštění Self-Extractor extrahujte obsah do dočasného adresáře.

    >[!NOTE] 
    > Sestavení zprostředkovatele prostředků odpovídá sestavení Azure zásobníku. Je nutné stáhnout správné binární verze zásobník Azure, který běží.

    | Sestavení Azure zásobníku | Instalační program SQL RP |
    | --- | --- |
    | 1.0.180102.3 | **Počkejte prosím, další informace, aktuální sestavení, nenainstaluje ale bude nadále spuštěna v několika uzly po upgradu zásobník Azure.** |
    | 1.0.171122.1 | [SQL RP verze 1.1.12.0](https://aka.ms/azurestacksqlrp) |
    | 1.0.171028.1 | [SQL RP verze 1.1.8.0](https://aka.ms/azurestacksqlrp1710) |
    | 1.0.170928.3 | [SQL RP verze 1.1.3.0](https://aka.ms/azurestacksqlrp1709) |
   

4. Kořenový certifikát zásobník Azure se načtou z privilegovaných koncový bod. Pro ASDK se vytvoří certifikát podepsaný svým držitelem v rámci tohoto procesu. Pro více uzly je nutné zadat příslušný certifikát.

    Pokud potřebujete přidat vlastní certifikát, je nutné umístit do souboru PFX **DependencyFilesLocalPath** (viz níže) následujícím způsobem:

    - Certifikát zástupných znaků pro \*.dbadapter.\< oblast\>.\< externí plně kvalifikovaný název domény\> nebo certifikát jedné lokalitě s běžný název sqladapter.dbadapter.\< oblast\>.\< externí plně kvalifikovaný název domény\>
    - Tento certifikát musí být důvěryhodný, jako by být vydaný certifikační autority. To znamená řetěz certifikátů, musí existovat bez nutnosti zprostředkující certifikáty.
    - V DependencyFilesLocalPath existuje pouze jeden certifikát souboru.
    - Název souboru nesmí obsahovat žádné speciální znaky.


5. Otevřete **nové** konzolu prostředí PowerShell se zvýšenými oprávněními (správce) a změny adresáře, kam jste extrahovali soubory. Použijte nové okno se pokud chcete vyhnout potížím, které může způsobit nesprávné moduly Powershellu již načtena v systému.

6. [Nainstalovat Azure PowerShell verze 1.2.11](azure-stack-powershell-install.md).

7. Spusťte skript DeploySqlProvider.ps1 s níže uvedenými parametry.

Skript provede tyto kroky:

- Nahrajte certifikáty a další artefakty na účet úložiště v zásobníku vaší Azure.
- Publikování balíčků galerie, abyste mohli nasadit databází SQL v galerii.
- Publikování Galerie balíčku pro nasazení serverů pro hostování
- Nasazení virtuálního počítače pomocí bitové kopie systému Windows Server 2016, vytvořili v kroku 1 a nainstalujte poskytovatele prostředků.
- Zaregistrujte místní záznam DNS, který se mapuje na zprostředkovateli prostředků virtuálních počítačů.
- Registrace zprostředkovatele prostředků s místní Azure Resource Manager (uživatele a správce).

> [!NOTE]
> Pokud instalace používá víc než 90 minut, může dojít k selhání a na obrazovce a v souboru protokolu se zobrazí chybová zpráva o, ale nasazení se pokus o z selhávající krok. Systémy, které nesplňuje doporučené specifikace paměti a virtuální procesor nemusí být možné instalovat SQL RP.
>

Tady je příklad můžete spustit z prostředí PowerShell vyzve (však změnit na informace o účtu a hesla, podle potřeby):

```
# Install the AzureRM.Bootstrapper module, set the profile, and install AzureRM and AzureStack modules
Install-Module -Name AzureRm.BootStrapper -Force
Use-AzureRmProfile -Profile 2017-03-09-profile
Install-Module -Name AzureStack -RequiredVersion 1.2.11 -Force

# Use the NetBIOS name for the Azure Stack domain. On ASDK, the default is AzureStack and the default prefix is AzS
# For integrated systems, the domain and the prefix will be the same.
$domain = "AzureStack"
$prefix = "AzS"
$privilegedEndpoint = "$prefix-ERCS01"

# Point to the directory where the RP installation files were extracted
$tempDir = 'C:\TEMP\SQLRP'

# The service admin account (can be AAD or ADFS)
$serviceAdmin = "admin@mydomain.onmicrosoft.com"
$AdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force
$AdminCreds = New-Object System.Management.Automation.PSCredential ($serviceAdmin, $AdminPass)

# Set credentials for the new Resource Provider VM
$vmLocalAdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force
$vmLocalAdminCreds = New-Object System.Management.Automation.PSCredential ("sqlrpadmin", $vmLocalAdminPass)

# and the cloudadmin credential required for Privileged Endpoint access
$CloudAdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force
$CloudAdminCreds = New-Object System.Management.Automation.PSCredential ("$domain\cloudadmin", $CloudAdminPass)

# change the following as appropriate
$PfxPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force

# Change directory to the folder where you extracted the installation files
# and adjust the endpoints
. $tempDir\DeploySQLProvider.ps1 -AzCredential $AdminCreds `
  -VMLocalCredential $vmLocalAdminCreds `
  -CloudAdminCredential $cloudAdminCreds `
  -PrivilegedEndpoint $privilegedEndpoint `
  -DefaultSSLCertificatePassword $PfxPass `
  -DependencyFilesLocalPath $tempDir\cert
 ```

### <a name="deploysqlproviderps1-parameters"></a>Parametry DeploySqlProvider.ps1
Tyto parametry můžete zadat na příkazovém řádku. Pokud ho použít nechcete, nebo jakékoli parametr ověření nezdaří, zobrazí se výzva k poskytování požadované těm, které jsou.

| Název parametru | Popis | Komentář nebo výchozí hodnotu |
| --- | --- | --- |
| **CloudAdminCredential** | Přihlašovací údaje pro správce cloudu potřebné pro přístup k privilegované koncový bod. | _požadované_ |
| **AzCredential** | Zadejte pověření pro účet správce služby Azure zásobníku. Pomocí stejných přihlašovacích údajů jako používat pro nasazování zásobník Azure). | _požadované_ |
| **VMLocalCredential** | Zadejte pověření pro účet místního správce zprostředkovatele prostředků SQL virtuálních počítačů. | _požadované_ |
| **PrivilegedEndpoint** | Zadejte IP adresu nebo název DNS Privleged koncového bodu. |  _požadované_ |
| **DependencyFilesLocalPath** | Soubor PFX certifikátu musí být umístěny v tomto adresáři také. | _volitelné_ (_povinné_ pro více uzly) |
| **DefaultSSLCertificatePassword** | Heslo pro certifikát .pfx | _požadované_ |
| **MaxRetryCount** | Definujte kolikrát chcete každou operaci opakovat, pokud dojde k chybě.| 2 |
| **RetryDuration** | Definujte časový limit mezi opakovanými pokusy, v sekundách. | 120 |
| **Odinstalace** | Odebrat zprostředkovatele prostředků a všechny přidružené prostředky (viz poznámky níže) | Ne |
| **Režim DebugMode** | Brání automatické čištění při selhání | Ne |


## <a name="verify-the-deployment-using-the-azure-stack-portal"></a>Ověření nasazení pomocí portálu Azure zásobníku

> [!NOTE]
>  Po dokončení skriptu instalace, musíte aktualizovat na portálu najdete v okně správce.


1. Přihlaste se k portálu pro správu jako správce služeb.

2. Ověřte, zda nasazení proběhla úspěšně. Vyhledejte **skupiny prostředků** &gt;, klikněte na **systému.\< umístění\>.sqladapter** prostředků skupiny a ověřte, zda všechny čtyři nasazení proběhla úspěšně.

      ![Ověření nasazení SQL RP](./media/azure-stack-sql-rp-deploy/sqlrp-verify.png)


## <a name="update-the-sql-resource-provider-adapter-multi-node-only-builds-1710-and-later"></a>Aktualizace adaptéru pro zprostředkovatele prostředků SQL (více uzly jen sestavení 1710 a novější)
Při každé aktualizaci sestavení zásobník Azure, budou vydané nový adaptér zprostředkovatele prostředků SQL. Zatímco existující adaptér může pokračovat v práci, se doporučuje aktualizovat na nejnovější verzi co nejdříve po aktualizaci zásobník Azure. Proces aktualizace je velmi podobný procesu instalace popsané výše. Vytvoří se nový virtuální počítač s nejnovější kód RP a nastavení se budou migrovat do této nové instance, včetně databáze a hostování informace o serveru, jakož i potřeby záznam DNS.

Pomocí skriptu UpdateSQLProvider.ps1 s stejné argumenty jako výše. Také je nutné zadat certifikát zde.

> [!NOTE]
> Aktualizace je podporována pouze v systémech s více uzly.

```
# Install the AzureRM.Bootstrapper module, set the profile, and install AzureRM and AzureStack modules
Install-Module -Name AzureRm.BootStrapper -Force
Use-AzureRmProfile -Profile 2017-03-09-profile
Install-Module -Name AzureStack -RequiredVersion 1.2.11 -Force

# Use the NetBIOS name for the Azure Stack domain. On ASDK, the default is AzureStack and the default prefix is AzS
# For integrated systems, the domain and the prefix will be the same.
$domain = "AzureStack"
$prefix = "AzS"
$privilegedEndpoint = "$prefix-ERCS01"

# Point to the directory where the RP installation files were extracted
$tempDir = 'C:\TEMP\SQLRP'

# The service admin account (can be AAD or ADFS)
$serviceAdmin = "admin@mydomain.onmicrosoft.com"
$AdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force
$AdminCreds = New-Object System.Management.Automation.PSCredential ($serviceAdmin, $AdminPass)

# Set credentials for the new Resource Provider VM
$vmLocalAdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force
$vmLocalAdminCreds = New-Object System.Management.Automation.PSCredential ("sqlrpadmin", $vmLocalAdminPass)

# and the cloudadmin credential required for Privileged Endpoint access
$CloudAdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force
$CloudAdminCreds = New-Object System.Management.Automation.PSCredential ("$domain\cloudadmin", $CloudAdminPass)

# change the following as appropriate
$PfxPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force

# Change directory to the folder where you extracted the installation files
# and adjust the endpoints
. $tempDir\UpdateSQLProvider.ps1 -AzCredential $AdminCreds `
  -VMLocalCredential $vmLocalAdminCreds `
  -CloudAdminCredential $cloudAdminCreds `
  -PrivilegedEndpoint $privilegedEndpoint `
  -DefaultSSLCertificatePassword $PfxPass `
  -DependencyFilesLocalPath $tempDir\cert
 ```

### <a name="updatesqlproviderps1-parameters"></a>Parametry UpdateSQLProvider.ps1
Tyto parametry můžete zadat na příkazovém řádku. Pokud ho použít nechcete, nebo jakékoli parametr ověření nezdaří, zobrazí se výzva k poskytování požadované těm, které jsou.

| Název parametru | Popis | Komentář nebo výchozí hodnotu |
| --- | --- | --- |
| **CloudAdminCredential** | Přihlašovací údaje pro správce cloudu potřebné pro přístup k privilegované koncový bod. | _požadované_ |
| **AzCredential** | Zadejte pověření pro účet správce služby Azure zásobníku. Pomocí stejných přihlašovacích údajů jako používat pro nasazování zásobník Azure). | _požadované_ |
| **VMLocalCredential** | Zadejte pověření pro účet místního správce zprostředkovatele prostředků SQL virtuálních počítačů. | _požadované_ |
| **PrivilegedEndpoint** | Zadejte IP adresu nebo název DNS Privleged koncového bodu. |  _požadované_ |
| **DependencyFilesLocalPath** | Soubor PFX certifikátu musí být umístěny v tomto adresáři také. | _volitelné_ (_povinné_ pro více uzly) |
| **DefaultSSLCertificatePassword** | Heslo pro certifikát .pfx | _požadované_ |
| **MaxRetryCount** | Definujte kolikrát chcete každou operaci opakovat, pokud dojde k chybě.| 2 |
| **RetryDuration** | Definujte časový limit mezi opakovanými pokusy, v sekundách. | 120 |
| **Odinstalace** | Odebrat zprostředkovatele prostředků a všechny přidružené prostředky (viz poznámky níže) | Ne |
| **Režim DebugMode** | Brání automatické čištění při selhání | Ne |



## <a name="remove-the-sql-resource-provider-adapter"></a>Odeberte adaptér zprostředkovatele prostředků SQL

Chcete-li odebrat poskytovatele prostředků, je nutné nejprve odebrat všechny závislosti.

1. Zkontrolujte, zda že máte původní balíček nasazení, který jste stáhli pro tuto verzi adaptér zprostředkovatele prostředků SQL.

2. Od zprostředkovatele prostředků (to neodstraní data), je třeba odstranit všechny uživatelské databáze. To by měli provádět sami uživatelé.

3. Správce musí odstranit hostitelskými servery z adaptéru zprostředkovatele prostředků SQL

4. Správce musí odstranit všechny plány, které odkazují na adaptéru zprostředkovatele prostředků SQL.

5. Správce musí odstranit všechny identifikátory SKU a kvóty, které jsou přidružené k adaptéru zprostředkovatele prostředků SQL.

6. Spusťte znovu skript nasazení s-odinstalovat parametr, koncové body Azure Resource Manager, DirectoryTenantID a pověření pro účet správce služby.


## <a name="next-steps"></a>Další postup

[Přidat servery, který je hostitelem](azure-stack-sql-resource-provider-hosting-servers.md) a [vytváření databází](azure-stack-sql-resource-provider-databases.md).

Zkuste jiné [PaaS služby](azure-stack-tools-paas-services.md) jako [poskytovatele prostředků serveru MySQL](azure-stack-mysql-resource-provider-deploy.md) a [poskytovatele prostředků App Services](azure-stack-app-service-overview.md).
