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
ms.date: 10/10/2017
ms.author: JeffGo
ms.openlocfilehash: 329970d8717053ab7126fb8fb6a4a119ccbff6b7
ms.sourcegitcommit: 6acb46cfc07f8fade42aff1e3f1c578aa9150c73
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/18/2017
---
# <a name="use-sql-databases-on-microsoft-azure-stack"></a>Databáze SQL pro použití v zásobníku Microsoft Azure

*Platí pro: Azure zásobníku integrované systémy a Azure zásobníku Development Kit*

Používat adaptér zprostředkovatele prostředků systému SQL Server jako službu vystavit databází SQL [zásobník Azure](azure-stack-poc.md). Po instalaci poskytovatele prostředků a připojte ho k jedné nebo více instancí systému SQL Server, můžete vytvořit vás a uživatele:
- databáze pro nativní cloudové aplikace
- weby, které jsou založené na SQL
- úlohy, které jsou založené na SQL nemusí pokaždé, když zřízení virtuálního počítače (VM) který je hostitelem systému SQL Server.

Zprostředkovatel prostředků nepodporuje všechny možnosti správy databáze z [Azure SQL Database](https://azure.microsoft.com/services/sql-database/). Například fondů elastické databáze a schopnost automaticky vytočit výkonu databáze nahoru či dolů nejsou k dispozici. Však nepodporuje Podpora zprostředkovatele prostředků podobné vytvářet, číst, aktualizovat a odstranit operace. Rozhraní API není kompatibilní s databázi SQL.

## <a name="sql-server-resource-provider-adapter-architecture"></a>Architektura adaptér zprostředkovatele prostředků SQL serveru
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

3. [Stáhněte si soubor binární soubory zprostředkovatele prostředků SQL](https://aka.ms/azurestacksqlrp) a provedení Self-Extractor extrahujte obsah do dočasného adresáře.

4. Kořenový certifikát zásobník Azure se načtou z privilegovaných koncový bod. Pro ASDK se vytvoří certifikát podepsaný svým držitelem v rámci tohoto procesu. Pro více uzly je nutné zadat příslušný certifikát.

    Pokud potřebujete přidat vlastní certifikát, je třeba následující certifikát:

    Certifikát se zástupným znakem pro \*.dbadapter.\< oblast\>.\< externí plně kvalifikovaný název domény\>. Tento certifikát musí být důvěryhodný, jako by být vydaný certifikační autority. To znamená řetěz certifikátů, musí existovat bez nutnosti zprostředkující certifikáty. Certifikát jedné lokality lze použít s explicitní název virtuálního počítače [sqladapter] použít během instalace.


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
> Pokud instalace používá víc než 90 minut, může dojít k selhání a na obrazovce a v souboru protokolu se zobrazí chybová zpráva o, ale nasazení se pokus o z selhávající krok. Systémy, které nesplňuje doporučené specifikace paměti a základní nemusí být možné instalovat SQL RP.
>

Tady je příklad můžete spustit z prostředí PowerShell vyzve (však změnit na informace o účtu a hesla, podle potřeby):

```
# Install the AzureRM.Bootstrapper module, set the profile, and install AzureRM and AzureStack modules
Install-Module -Name AzureRm.BootStrapper -Force
Use-AzureRmProfile -Profile 2017-03-09-profile
Install-Module -Name AzureStack -RequiredVersion 1.2.11 -Force

# Use the NetBIOS name for the Azure Stack domain. On ASDK, the default is AzureStack
$domain = "AzureStack"
# Point to the directory where the RP installation files were extracted
$tempDir = 'C:\TEMP\SQLRP'

# The service admin account (can be AAD or ADFS)
$serviceAdmin = "admin@mydomain.onmicrosoft.com"
$AdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force
$AdminCreds = New-Object System.Management.Automation.PSCredential ($serviceAdmin, $AdminPass)

# Set the credentials for the Resource Provider VM
$vmLocalAdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force
$vmLocalAdminCreds = New-Object System.Management.Automation.PSCredential ("sqlrpadmin", $vmLocalAdminPass)

# and the cloudadmin credential required for Privileged Endpoint access
$CloudAdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force
$CloudAdminCreds = New-Object System.Management.Automation.PSCredential ("$domain\cloudadmin", $CloudAdminPass)

# change the following as appropriate
$PfxPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force

# Change directory to the folder where you extracted the installation files
# and adjust the endpoints
.$tempDir\DeploySQLProvider.ps1 -AzCredential $AdminCreds -VMLocalCredential $vmLocalAdminCreds -CloudAdminCredential $cloudAdminCreds -PrivilegedEndpoint '10.10.10.10' -DefaultSSLCertificatePassword $PfxPass -DependencyFilesLocalPath $tempDir\cert
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





## <a name="removing-the-sql-adapter-resource-provider"></a>Odebrání poskytovatele prostředků adaptér SQL

Chcete-li odebrat poskytovatele prostředků, je nutné nejprve odebrat všechny závislosti.

1. Zkontrolujte, zda že máte původní balíček nasazení, který jste stáhli pro tuto verzi zprostředkovatele prostředků.

2. Od zprostředkovatele prostředků (to neodstraní data), je třeba odstranit všechny uživatelské databáze. To by měli provádět sami uživatelé.

3. Správce musí odstranit hostitelskými servery z adaptéru SQL

4. Správce musí odstranit všechny plány, které odkazují na adaptér SQL.

5. Správce musí odstranit všechny identifikátory SKU a kvóty, které jsou přidružené k adaptéru SQL.

6. Spusťte znovu skript nasazení s-odinstalovat parametr, koncové body Azure Resource Manager, DirectoryTenantID a pověření pro účet správce služby.


## <a name="next-steps"></a>Další kroky


Zkuste jiné [PaaS služby](azure-stack-tools-paas-services.md) jako [poskytovatele prostředků serveru MySQL](azure-stack-mysql-resource-provider-deploy.md) a [poskytovatele prostředků App Services](azure-stack-app-service-overview.md).
