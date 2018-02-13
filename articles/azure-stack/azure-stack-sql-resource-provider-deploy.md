---
title: "Používání databáze SQL v Azure zásobníku | Microsoft Docs"
description: "Zjistěte, jak můžete nasadit databází SQL jako služba na Azure zásobníku a rychlé kroky k nasazení adaptér zprostředkovatele prostředků systému SQL Server."
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
ms.date: 01/09/2018
ms.author: JeffGo
ms.openlocfilehash: bf52ed4986b4e0930b57721c0e38bbf748045a36
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/01/2018
---
# <a name="use-sql-databases-on-microsoft-azure-stack"></a>Databáze SQL pro použití v zásobníku Microsoft Azure

*Platí pro: Azure zásobníku integrované systémy a Azure zásobníku Development Kit*

Používat adaptér zprostředkovatele prostředků systému SQL Server jako službu vystavit databází SQL [zásobník Azure](azure-stack-poc.md). Po instalaci poskytovatele prostředků a připojte ho k jedné nebo více instancí systému SQL Server, můžete vytvořit vás a uživatele:
- Databáze pro nativní cloudové aplikace.
- Weby, které jsou založené na SQL.
- Úlohy, které jsou založené na SQL.
Nemáte pokaždé, když zřízení virtuálního počítače (VM) který je hostitelem systému SQL Server.

Zprostředkovatel prostředků nepodporuje všechny možnosti správy databáze z [Azure SQL Database](https://azure.microsoft.com/services/sql-database/). Například fondů elastické databáze a schopnost vytočit výkonu databáze nahoru a dolů automaticky nejsou k dispozici. Však nepodporuje Podpora zprostředkovatele prostředků podobné vytvářet, číst, aktualizovat a odstranit operace. Rozhraní API není kompatibilní s databází SQL.

## <a name="sql-resource-provider-adapter-architecture"></a>Architektura adaptér zprostředkovatele prostředků SQL
Zprostředkovatel prostředků se skládá ze tří součástí:

- **Adaptér pro poskytovatele prostředků SQL virtuálních počítačů**, což je virtuální počítač Windows, který spouští služby poskytovatele.
- **Zprostředkovatel prostředků samotné**, která zpracovává požadavky pro zřizování a zpřístupňuje databáze prostředků.
- **Servery, které jsou hostiteli systému SQL Server**, které poskytují kapacitu pro databáze názvem hostitelskými servery.

Musíte vytvořit jednu (nebo více) intances systému SQL Server nebo poskytovat přístup k externí instance systému SQL Server.

## <a name="deploy-the-resource-provider"></a>Nasazení poskytovatele prostředků

1. Pokud jste tak již neučinili, zaregistrujte vaší development kit a stáhnout bitovou kopii jádro systému Windows Server 2016 Datacenter ke stažení přes správu Marketplace. Je nutné použít bitovou kopii jádro systému Windows Server 2016. Skript můžete také použít k vytvoření [bitové kopie systému Windows Server 2016](https://docs.microsoft.com/azure/azure-stack/azure-stack-add-default-image). (Nezapomeňte vybrat možnost jádra). Modul runtime rozhraní .NET 3.5 už nepotřebujete.

2. Přihlaste se k hostiteli, který přístup privilegované koncový bod virtuálního počítače.

    - Na Azure zásobníku Development Kit instalace Přihlaste se k fyzického hostitele.

    - V systémech s více uzly hostitele musí být systém, kterým může přistupovat privilegované koncový bod.
    
    >[!NOTE]
    > Systém, kde je spuštěn skript *musí* být systém Windows 10 nebo Windows Server 2016 na nejnovější verzi modulu runtime rozhraní .NET nainstalované. Instalace selže. Azure SDK zásobníku hostitele splňuje tato kritéria.


3. Stáhněte poskytovatele prostředků SQL binární. Pak spusťte Self-Extractor extrahujte obsah do dočasného adresáře.

    >[!NOTE] 
    > Sestavení zprostředkovatele prostředků odpovídá sestavení Azure zásobníku. Ujistěte se, že jste stáhnout správné binární verze zásobník Azure, který běží.

    | Azure sestavení zásobníku | Instalační program zprostředkovatele prostředků SQL |
    | --- | --- |
    |1.0.180102.3, 1.0.180103.2 nebo 1.0.180106.1 (více uzly) | [SQL RP verze 1.1.14.0](https://aka.ms/azurestacksqlrp1712) |
    | 1.0.171122.1 | [SQL RP verze 1.1.12.0](https://aka.ms/azurestacksqlrp1711) |
    | 1.0.171028.1 | [SQL RP verze 1.1.8.0](https://aka.ms/azurestacksqlrp1710) |
  

4. Kořenový certifikát zásobník Azure se načtou z privilegovaných koncový bod. Pro zásobník sadu Azure SDK se vytvoří certifikát podepsaný svým držitelem v rámci tohoto procesu. Pro více uzly je nutné zadat příslušný certifikát.

   Chcete-li přidat vlastní certifikát, umístěte soubor .pfx v **DependencyFilesLocalPath** následujícím způsobem:

    - Certifikát zástupných znaků pro \*.dbadapter.\< oblast\>.\< externí plně kvalifikovaný název domény\> nebo certifikát jedné lokalitě s běžný název sqladapter.dbadapter.\< oblast\>.\< externí plně kvalifikovaný název domény\>.

    - Tento certifikát musí být důvěryhodný. To znamená řetěz certifikátů, musí existovat bez nutnosti zprostředkující certifikáty.

    - V DependencyFilesLocalPath existuje pouze jeden certifikát souboru.

    - Název souboru nesmí obsahovat žádné speciální znaky.


5. Otevřete **nové** konzolu prostředí PowerShell se zvýšenými oprávněními (správce) a změny adresáře, kam jste extrahovali soubory. Použijte nové okno se pokud chcete vyhnout potížím, které by mohly nastat z nesprávné modulů prostředí PowerShell, které jsou již načtena v systému.

6. [Nainstalovat Azure PowerShell verze 1.2.11](azure-stack-powershell-install.md).

7. Spusťte skript DeploySqlProvider.ps1, který provádí tyto kroky:

    - Ukládání certifikáty a další artefakty na účet úložiště v Azure zásobníku.
    - Publikuje Galerie balíčky, abyste mohli nasadit databází SQL v galerii.
    - Publikuje balíček Galerie pro nasazení hostitelskými servery.
    - Virtuální počítač se nasadí pomocí bitové kopie systému Windows Server 2016, který byl vytvořen v kroku 1 a poté nainstaluje poskytovatele prostředků.
    - Zaregistruje místní záznam DNS, který se mapuje na zprostředkovateli prostředků virtuálních počítačů.
    - Zaregistruje poskytovatel prostředků s místní Azure Resource Manager (uživatele a správce).

> [!NOTE]
> Pokud instalace používá víc než 90 minut, může selhat. Pokud se nezdaří, na obrazovce a v souboru protokolu se zobrazí zpráva o neúspěšném zpracování, ale nasazení se pokus o z selhávající krok. Systémy, které nesplňuje doporučené specifikace paměti a virtuální procesor nemusí být možné instalovat poskytovatele provedena SQL.
>

Tady je příklad, který můžete spustit z příkazového řádku prostředí PowerShell. (Nezapomeňte změnit informace o účtu a hesla podle potřeby.)

```
# Install the AzureRM.Bootstrapper module, set the profile, and install the AzureRM and AzureStack modules.
Install-Module -Name AzureRm.BootStrapper -Force
Use-AzureRmProfile -Profile 2017-03-09-profile
Install-Module -Name AzureStack -RequiredVersion 1.2.11 -Force

# Use the NetBIOS name for the Azure Stack domain. On the Azure Stack SDK, the default is AzureStack and the default prefix is AzS.
# For integrated systems, the domain and the prefix are the same.
$domain = "AzureStack"
$prefix = "AzS"
$privilegedEndpoint = "$prefix-ERCS01"

# Point to the directory where the resource provider installation files were extracted.
$tempDir = 'C:\TEMP\SQLRP'

# The service admin account (can be Azure Active Directory or Active Directory Federation Services).
$serviceAdmin = "admin@mydomain.onmicrosoft.com"
$AdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force
$AdminCreds = New-Object System.Management.Automation.PSCredential ($serviceAdmin, $AdminPass)

# Set credentials for the new Resource Provider VM.
$vmLocalAdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force
$vmLocalAdminCreds = New-Object System.Management.Automation.PSCredential ("sqlrpadmin", $vmLocalAdminPass)

# And the cloudadmin credential that's required for privileged endpoint access.
$CloudAdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force
$CloudAdminCreds = New-Object System.Management.Automation.PSCredential ("$domain\cloudadmin", $CloudAdminPass)

# Change the following as appropriate.
$PfxPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force

# Change directory to the folder where you extracted the installation files.
# Then adjust the endpoints.
. $tempDir\DeploySQLProvider.ps1 -AzCredential $AdminCreds `
  -VMLocalCredential $vmLocalAdminCreds `
  -CloudAdminCredential $cloudAdminCreds `
  -PrivilegedEndpoint $privilegedEndpoint `
  -DefaultSSLCertificatePassword $PfxPass `
  -DependencyFilesLocalPath $tempDir\cert
 ```

### <a name="deploysqlproviderps1-parameters"></a>DeploySqlProvider.ps1 parameters
Tyto parametry můžete zadat na příkazovém řádku. Pokud ho použít nechcete, nebo pokud žádné parametr ověření selže, zobrazí se výzva k zadání požadované parametry.

| Název parametru | Popis | Komentář nebo výchozí hodnotu |
| --- | --- | --- |
| **CloudAdminCredential** | Přihlašovací údaje pro správce cloudu potřebné pro přístup k privilegované koncový bod. | _Požadované_ |
| **AzCredential** | Přihlašovací údaje pro účet správce služby Azure zásobníku. Pomocí stejných přihlašovacích údajů jako používat pro nasazování Azure zásobníku. | _Požadované_ |
| **VMLocalCredential** | Přihlašovací údaje pro účet místního správce zprostředkovatele prostředků SQL virtuálních počítačů. | _Požadované_ |
| **PrivilegedEndpoint** | IP adresa nebo název DNS privilegované koncového bodu. |  _Požadované_ |
| **DependencyFilesLocalPath** | Soubor .pfx certifikátu musí být umístěny v tomto adresáři také. | _Volitelné_ (_povinné_ pro více uzly) |
| **DefaultSSLCertificatePassword** | Heslo pro certifikát .pfx. | _Požadované_ |
| **MaxRetryCount** | Počet přístupů, které chcete opakovat každé operace, pokud dojde k selhání.| 2 |
| **RetryDuration** | Interval časového limitu mezi opakovanými pokusy, v sekundách. | 120 |
| **Odinstalace** | Odebere poskytovatele prostředků a všechny přidružené prostředky (viz následující poznámky k). | Ne |
| **Režim DebugMode** | Brání automatické čištění při selhání. | Ne |


## <a name="verify-the-deployment-using-the-azure-stack-portal"></a>Ověření nasazení pomocí portálu Azure zásobníku

> [!NOTE]
>  Po dokončení skriptu instalace musíte aktualizovat na portálu najdete v okně správce.


1. Přihlaste se k portálu pro správu jako správce služeb.

2. Ověřte, zda nasazení proběhla úspěšně. Přejděte na **skupiny prostředků**. Vyberte **systému.\< umístění\>.sqladapter** skupinu prostředků. Ověřte, zda všechny čtyři nasazení proběhla úspěšně.

      ![Ověření nasazení poskytovatele prostředků SQL](./media/azure-stack-sql-rp-deploy/sqlrp-verify.png)


## <a name="update-the-sql-resource-provider-adapter-multi-node-only-builds-1710-and-later"></a>Aktualizace adaptéru pro zprostředkovatele prostředků SQL (více uzly jen sestavení 1710 a novější)
Při každé aktualizaci sestavení Azure zásobníku nový adaptér zprostředkovatele prostředků SQL vydání. Existující adaptér může pokračovat v práci. Doporučujeme však aktualizaci na nejnovější verzi co nejdříve po aktualizaci zásobník Azure. 

Proces aktualizace je podobný procesu instalace, který je popsán výše. Vytvoříte nový virtuální počítač s nejnovější kód zprostředkovatele prostředků. Kromě toho můžete migrovat nastavení do této nové instance, včetně databáze a hostování informace o serveru. Můžete také migrovat nezbytné záznam DNS.

Pomocí skriptu UpdateSQLProvider.ps1 s stejné argumenty, které jsme popsané výše. Také je nutné zadat certifikát zde.

> [!NOTE]
> Tento proces aktualizace je podporována pouze v systémech s více uzly.

```
# Install the AzureRM.Bootstrapper module, set the profile, and install the AzureRM and AzureStack modules.
Install-Module -Name AzureRm.BootStrapper -Force
Use-AzureRmProfile -Profile 2017-03-09-profile
Install-Module -Name AzureStack -RequiredVersion 1.2.11 -Force

# Use the NetBIOS name for the Azure Stack domain. On the Azure Stack SDK, the default is AzureStack and the default prefix is AzS.
# For integrated systems, the domain and the prefix are the same.
$domain = "AzureStack"
$prefix = "AzS"
$privilegedEndpoint = "$prefix-ERCS01"

# Point to the directory where the resource provider installation files were extracted.
$tempDir = 'C:\TEMP\SQLRP'

# The service admin account (can be Azure AD or AD FS).
$serviceAdmin = "admin@mydomain.onmicrosoft.com"
$AdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force
$AdminCreds = New-Object System.Management.Automation.PSCredential ($serviceAdmin, $AdminPass)

# Set credentials for the new Resource Provider VM.
$vmLocalAdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force
$vmLocalAdminCreds = New-Object System.Management.Automation.PSCredential ("sqlrpadmin", $vmLocalAdminPass)

# And the cloudadmin credential required for privileged endpoint access.
$CloudAdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force
$CloudAdminCreds = New-Object System.Management.Automation.PSCredential ("$domain\cloudadmin", $CloudAdminPass)

# Change the following as appropriate.
$PfxPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force

# Change directory to the folder where you extracted the installation files.
# Then adjust the endpoints.
. $tempDir\UpdateSQLProvider.ps1 -AzCredential $AdminCreds `
  -VMLocalCredential $vmLocalAdminCreds `
  -CloudAdminCredential $cloudAdminCreds `
  -PrivilegedEndpoint $privilegedEndpoint `
  -DefaultSSLCertificatePassword $PfxPass `
  -DependencyFilesLocalPath $tempDir\cert
 ```

### <a name="updatesqlproviderps1-parameters"></a>UpdateSQLProvider.ps1 parameters
Tyto parametry můžete zadat na příkazovém řádku. Pokud ho použít nechcete, nebo pokud žádné parametr ověření selže, zobrazí se výzva k zadání požadované parametry.

| Název parametru | Popis | Komentář nebo výchozí hodnotu |
| --- | --- | --- |
| **CloudAdminCredential** | Přihlašovací údaje pro správce cloudu potřebné pro přístup k privilegované koncový bod. | _Požadované_ |
| **AzCredential** | Přihlašovací údaje pro účet správce služby Azure zásobníku. Pomocí stejných přihlašovacích údajů, které jste použili pro nasazení Azure zásobníku. | _Požadované_ |
| **VMLocalCredential** | Přihlašovací údaje pro účet místního správce zprostředkovatele prostředků SQL virtuálních počítačů. | _Požadované_ |
| **PrivilegedEndpoint** | IP adresa nebo název DNS privilegované koncového bodu. |  _Požadované_ |
| **DependencyFilesLocalPath** | Soubor .pfx certifikátu musí být umístěny v tomto adresáři také. | _Volitelné_ (_povinné_ pro více uzly) |
| **DefaultSSLCertificatePassword** | Heslo pro certifikát .pfx. | _požadované_ |
| **MaxRetryCount** | Počet přístupů, které chcete opakovat každé operace, pokud dojde k selhání.| 2 |
| **RetryDuration** |Interval časového limitu mezi opakovanými pokusy, v sekundách. | 120 |
| **Odinstalace** | Odebere poskytovatele prostředků a všechny přidružené prostředky (viz následující poznámky k). | Ne |
| **Režim DebugMode** | Brání automatické čištění při selhání. | Ne |



## <a name="remove-the-sql-resource-provider-adapter"></a>Odeberte adaptér zprostředkovatele prostředků SQL

Odebrat zprostředkovatele prostředků, je nutné nejprve odebrat všechny závislosti.

1. Ujistěte se, že máte původní balíček nasazení, který jste stáhli pro tuto verzi adaptér zprostředkovatele prostředků SQL.

2. Od zprostředkovatele prostředků je třeba odstranit všechny uživatelské databáze. (Odstranění uživatelských databází neodstraní data.) Tato úloha by měla provést sami uživatelé.

3. Správce musí odstranit hostitelskými servery z adaptéru zprostředkovatele prostředků SQL.

4. Správce musí odstranit všechny plány, které odkazují na adaptéru zprostředkovatele prostředků SQL.

5. Správce musí odstranit všechny identifikátory SKU a kvóty, které jsou spojeny s adaptérem zprostředkovatele prostředků SQL.

6. Spusťte znovu skript nasazení s následující prvky:
    - -Odinstalovat parametr
    - Koncové body Azure Resource Manager
    - DirectoryTenantID
    - Přihlašovací údaje k účtu správce služby


## <a name="next-steps"></a>Další postup

[Přidání hostitelské servery](azure-stack-sql-resource-provider-hosting-servers.md) a [vytváření databází](azure-stack-sql-resource-provider-databases.md).

Zkuste jiné [PaaS služby](azure-stack-tools-paas-services.md) jako [poskytovatele prostředků serveru MySQL](azure-stack-mysql-resource-provider-deploy.md) a [poskytovatele prostředků App Services](azure-stack-app-service-overview.md).
