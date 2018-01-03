---
title: "Používání databází MySQL jako PaaS v zásobníku Azure | Microsoft Docs"
description: "Zjistěte, jak můžete nasadit MySQL zprostředkovatel prostředků a poskytovat databází MySQL jako službu v Azure zásobníku"
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
ms.openlocfilehash: 71abceb1afe315a09ea88b593f9806e9e8b31f16
ms.sourcegitcommit: 68aec76e471d677fd9a6333dc60ed098d1072cfc
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/18/2017
---
# <a name="use-mysql-databases-on-microsoft-azure-stack"></a>Používání databází MySQL v zásobníku Microsoft Azure

*Platí pro: Azure zásobníku integrované systémy a Azure zásobníku Development Kit*

Můžete nasadit poskytovatele prostředků MySQL v Azure zásobníku. Poté, co nasadíte poskytovatele prostředků, můžete vytvořit MySQL servery a databáze pomocí šablony nasazení Azure Resource Manager a zadejte databází MySQL jako služba. Databáze MySQL, které jsou společné pro webové servery, podporují mnoho platformy webu. Jako příklad poté, co nasadíte poskytovatele prostředků, můžete vytvořit weby WordPress z platformy Azure Web Apps jako služba (PaaS) rozšíření pro Azure zásobníku.

Chcete-li nasadit zprostředkovatele databáze MySQL na systém, který nemá přístup k Internetu, můžete zkopírovat soubor [mysql konektor net 6.9.9.msi](https://dev.mysql.com/get/Download/sConnector-Net/mysql-connector-net-6.9.9.msi) do místní sdílené složky. Potom zadejte název této sdílené složky Po zobrazení výzvy. Je také nutné nainstalovat moduly Azure a Azure PowerShell zásobníku.


## <a name="mysql-server-resource-provider-adapter-architecture"></a>Architektura adaptér zprostředkovatele prostředků serveru MySQL

Zprostředkovatel prostředků se skládá ze tří součástí:

- **MySQL prostředků zprostředkovatele adaptér virtuálního počítače**, což je virtuální počítač s Windows běží služby poskytovatele.
- **Zprostředkovatel prostředků samotné**, která zpracovává požadavky pro zřizování a zpřístupňuje databáze prostředků.
- **Servery, které hostují MySQL Server**, které poskytují kapacitu pro databáze, se nazývají hostování servery.

Tato verze už vytvoří instanci databáze MySQL. Musíte je vytvořit a poskytují přístup k externí instance systému SQL. Přejděte [galerii pro rychlý start Azure zásobníku](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/mysql-standalone-server-windows) pro šablonu příklad, který můžete:
- Vytvoření serveru MySQL pro vás
- stáhnout a nasadit MySQL Server z Marketplace.

> [!NOTE]
> Hostitelské servery, které jsou nainstalované v zásobníku Azure několika uzly musí být vytvořeny z předplatného klienta. Nemohou být vytvářeny ze předplatného výchozí zprostředkovatel. Jinými slovy musí se vytvořit z portálu klienta nebo z relace prostředí PowerShell s odpovídající přihlášení. Všechny hostitelské servery jsou fakturovatelné virtuální počítače a musí mít příslušnou licenci. Správce služby můžete být vlastníkem daného předplatného.

### <a name="required-privileges"></a>Požadovaná oprávnění
Systémový účet musí mít následující oprávnění:

1.  Databáze: Vytvořit, vyřadit
2.  Přihlášení: Vytvořit, nastavit, vyřadit, udělit, odvolat

## <a name="deploy-the-resource-provider"></a>Nasazení poskytovatele prostředků

1. Pokud jste tak již neučinili, zaregistrujte vaší development kit a stáhnout bitovou kopii jádro systému Windows Server 2016 Datacenter ke stažení přes správu Marketplace. Je nutné použít bitovou kopii jádro systému Windows Server 2016. Skript můžete také použít k vytvoření [bitové kopie systému Windows Server 2016](https://docs.microsoft.com/azure/azure-stack/azure-stack-add-default-image) -je nutné vybrat možnost jádra. Modul runtime rozhraní .NET 3.5 už nepotřebujete.


2. Přihlaste se k hostiteli, který přístup privilegované koncový bod virtuálního počítače.

    a. Instalace Azure zásobníku Development Kit (ASDK) Přihlaste se na fyzickém hostiteli.

    b. V systémech s více uzly hostitele musí být systém, kterým může přistupovat privilegované koncový bod.
    
    >[!NOTE]
    > Systém, kde je spuštěn skript *musí* být systém Windows 10 nebo Windows Server 2016 na nejnovější verzi modulu runtime rozhraní .NET nainstalované. Jinak se instalace nepovede. Hostitel ASDK splňuje tato kritéria.
    

3. Zprostředkovatel prostředků MySQL binární stažení a spuštění Self-Extractor extrahujte obsah do dočasného adresáře.

    >[!NOTE] 
    > Sestavení zprostředkovatele prostředků odpovídá sestavení Azure zásobníku. Je nutné stáhnout správné binární verze zásobník Azure, který běží.

    | Sestavení Azure zásobníku | Instalační program MySQL RP |
    | --- | --- |
    | 1.0.171122.1 | [MySQL RP verze 1.1.12.0](https://aka.ms/azurestackmysqlrp) |
    | 1.0.171028.1 | [MySQL RP verze 1.1.8.0](https://aka.ms/azurestackmysqlrp1710) |
    | 1.0.170928.3 | [MySQL RP verze 1.1.3.0](https://aka.ms/azurestackmysqlrp1709) |

4.  Kořenový certifikát zásobník Azure se načtou z privilegovaných koncový bod. Pro ASDK se vytvoří certifikát podepsaný svým držitelem v rámci tohoto procesu. Pro více uzly je nutné zadat příslušný certifikát.

    Pokud potřebujete přidat vlastní certifikát, je nutné umístit do souboru PFX **DependencyFilesLocalPath** (viz níže) následujícím způsobem:

    - Certifikát zástupných znaků pro \*.dbadapter.\< oblast\>.\< externí plně kvalifikovaný název domény\> nebo certifikát jedné lokalitě s běžný název mysqladapter.dbadapter.\< oblast\>.\< externí plně kvalifikovaný název domény\>
    - Tento certifikát musí být důvěryhodný, jako by být vydaný certifikační autority. To znamená řetěz certifikátů, musí existovat bez nutnosti zprostředkující certifikáty.
    - V DependencyFilesLocalPath existuje pouze jeden certifikát souboru.
    - Název souboru nesmí obsahovat žádné speciální znaky.



5. Otevřete **nové** konzolu prostředí PowerShell se zvýšenými oprávněními (správce) a změny adresáře, kam jste extrahovali soubory. Použijte nové okno se pokud chcete vyhnout potížím, které může způsobit nesprávné moduly Powershellu již načtena v systému.

6. [Nainstalovat Azure PowerShell verze 1.2.11](azure-stack-powershell-install.md).

7. Spusťte skript DeploySqlProvider.ps1.

Skript provede tyto kroky:

* Stáhněte si MySQL konektor binárního souboru (to se dá zajistit offline).
* Nahrajte certifikáty a další artefakty na účet úložiště v zásobníku vaší Azure.
* Publikování balíčků galerie, abyste mohli nasadit databází SQL v galerii.
* Publikování Galerie balíčku pro nasazení serverů pro hostování
* Nasazení virtuálního počítače pomocí bitové kopie systému Windows Server 2016, vytvořili v kroku 1 a nainstalujte poskytovatele prostředků.
* Zaregistrujte místní záznam DNS, který se mapuje na zprostředkovateli prostředků virtuálních počítačů.
* Registrace zprostředkovatele prostředků s místní Azure Resource Manager (klient a správce).


Můžete:
- Zadejte alespoň požadované parametry na příkazovém řádku
- nebo pokud spustíte bez zadaných parametrů, je po zobrazení výzvy zadejte.

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
$tempDir = 'C:\TEMP\MYSQLRP'

# The service admin account (can be AAD or ADFS)
$serviceAdmin = "admin@mydomain.onmicrosoft.com"
$AdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force
$AdminCreds = New-Object System.Management.Automation.PSCredential ($serviceAdmin, $AdminPass)

# Set the credentials for the new Resource Provider VM
$vmLocalAdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force
$vmLocalAdminCreds = New-Object System.Management.Automation.PSCredential ("mysqlrpadmin", $vmLocalAdminPass)

# and the cloudadmin credential required for Privleged Endpoint access
$CloudAdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force
$CloudAdminCreds = New-Object System.Management.Automation.PSCredential ("$domain\cloudadmin", $CloudAdminPass)

# change the following as appropriate
$PfxPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force

# Run the installation script from the folder where you extracted the installation files
# Find the ERCS01 IP address first and make sure the certificate
# file is in the specified directory
. $tempDir\DeployMySQLProvider.ps1 -AzCredential $AdminCreds `
  -VMLocalCredential $vmLocalAdminCreds `
  -CloudAdminCredential $cloudAdminCreds `
  -PrivilegedEndpoint $privilegedEndpoint `
  -DefaultSSLCertificatePassword $PfxPass `
  -DependencyFilesLocalPath $tempDir\cert `
  -AcceptLicense

 ```


### <a name="deploysqlproviderps1-parameters"></a>Parametry DeploySqlProvider.ps1
Tyto parametry můžete zadat na příkazovém řádku. Pokud ho použít nechcete, nebo jakékoli parametr ověření nezdaří, zobrazí se výzva k poskytování požadované těm, které jsou.

| Název parametru | Popis | Komentář nebo výchozí hodnotu |
| --- | --- | --- |
| **CloudAdminCredential** | Přihlašovací údaje pro správce cloudu potřebné pro přístup k Privleged koncový bod. | _požadované_ |
| **AzCredential** | Zadejte pověření pro účet správce služby Azure zásobníku. Pomocí stejných přihlašovacích údajů jako používat pro nasazování zásobník Azure). | _požadované_ |
| **VMLocalCredential** | Zadejte pověření pro účet místního správce MySQL zprostředkovatele prostředků virtuálních počítačů. | _požadované_ |
| **PrivilegedEndpoint** | Zadejte IP adresu nebo název DNS privilegované koncového bodu. |  _požadované_ |
| **DependencyFilesLocalPath** | Cesta k místní sdílené složky obsahující [mysql konektor net 6.9.9.msi](https://dev.mysql.com/get/Downloads/Connector-Net/mysql-connector-net-6.9.9.msi). Pokud ji zadáte, musí být v tomto adresáři také umístěny soubor certifikátu. | _volitelné_ (_povinné_ pro více uzly) |
| **DefaultSSLCertificatePassword** | Heslo pro certifikát .pfx | _požadované_ |
| **MaxRetryCount** | Definujte kolikrát chcete každou operaci opakovat, pokud dojde k chybě.| 2 |
| **RetryDuration** | Definujte časový limit mezi opakovanými pokusy, v sekundách. | 120 |
| **Odinstalace** | Odebrat zprostředkovatele prostředků a všechny přidružené prostředky (viz poznámky níže) | Ne |
| **Režim DebugMode** | Brání automatické čištění při selhání | Ne |
| **AcceptLicense** | Přeskočí řádku tak, aby přijímal GPL licence (http://www.gnu.org/licenses/old-licenses/gpl-2.0.html) | |


V závislosti na výkonu a stahování rychlosti systému instalace může trvat jako málo jako 20 minut, nebo jako dlouho jako několik hodin. Pokud v okně MySQLAdapter není k dispozici, aktualizujte portál pro správu.

> [!NOTE]
> Pokud instalace používá víc než 90 minut, může dojít k selhání a zobrazí se chybová zpráva o na obrazovce a v souboru protokolu. Z selhávající krok tím se pokus o nasazení. Systémy, které nesplňuje doporučené specifikace paměti a základní nemusí být možné instalovat MySQL RP.



## <a name="verify-the-deployment-using-the-azure-stack-portal"></a>Ověření nasazení pomocí portálu Azure zásobníku

> [!NOTE]
>  Po dokončení skriptu instalace, musíte aktualizovat na portálu najdete v okně správce.


1. Přihlaste se k portálu pro správu jako správce služeb.

2. Ověřte, zda nasazení proběhla úspěšně. Vyhledejte **skupiny prostředků** &gt;, klikněte na **systému.\< umístění\>.mysqladapter** prostředků skupiny a ověřte, zda všechny čtyři nasazení proběhla úspěšně.

      ![Ověření nasazení MySQL RP](./media/azure-stack-mysql-rp-deploy/mysqlrp-verify.png)

## <a name="provide-capacity-by-connecting-to-a-mysql-hosting-server"></a>Zadejte kapacitu připojením k hostitelskému serveru MySQL

1. Přihlaste se k portálu Azure zásobníku jako správce služby

2. Přejděte do **prostředky pro správu** &gt; **MySQL hostitelské servery** &gt; **+ přidat**.

    **MySQL hostování servery** je okno, kde můžete připojit zprostředkovatele prostředků serveru MySQL skutečné instancí MySQL serveru, které slouží jako zprostředkovatel prostředků back-end.

    ![Hostitelské servery](./media/azure-stack-mysql-rp-deploy/mysql-add-hosting-server-2.png)

3. Vyplňte formulář Podrobnosti připojení vaší instance serveru MySQL. Zadejte název plně kvalifikované domény (FQDN) nebo platná IPv4 adresa a není krátký název virtuálního počítače. Tato instalace již poskytuje výchozí instance databáze MySQL. Zadaná velikost pomáhá spravovat kapacity databáze poskytovatele prostředků. Mělo by být blíží fyzické kapacitě databázového serveru.

    > [!NOTE]
    > Tak dlouho, dokud instance databáze MySQL byla přístupná pomocí klienta a správce Azure Resource Manager, mohou být umístěny pod kontrolou zprostředkovatele prostředků. Instance databáze MySQL __musí__ přidělit výhradně RP.

4. Při přidávání serverů, musíte je přiřadit k nové nebo existující SKU umožňující rozdílů mezi nabídek služeb.
  Například můžete mít instanci enterprise poskytuje:
    - kapacita databáze
    - Automatické zálohování
    - Rezervovat vysoce výkonné servery pro jednotlivá oddělení
 

Název SKU by měla odpovídat vlastnosti tak, aby klienti můžete umístit své databáze správně. Všechny hostitelské servery v SKU musí mít stejné funkce.

![Vytvoření databáze MySQL SKU](./media/azure-stack-mysql-rp-deploy/mysql-new-sku.png)


>[!NOTE]
> SKU může trvat až jednu hodinu, mají být zobrazeny v portálu. Databázi nelze vytvořit, dokud nebude vytvořen verze SKU.


## <a name="to-test-your-deployment-create-your-first-mysql-database"></a>Chcete nasazení otestovat, vytvořte svoji první databázi MySQL


1. Přihlaste se k portálu Azure zásobníku jako správce služby.

2. Klikněte **+ nový** tlačítko &gt; **Data + úložiště** &gt; **databáze MySQL**.

3. Vyplňte formulář s podrobnostmi o databázi.

    ![Vytvoření databáze MySQL testu](./media/azure-stack-mysql-rp-deploy/mysql-create-db.png)

4. Vyberte SKU.

    ![Vyberte SKU](./media/azure-stack-mysql-rp-deploy/mysql-select-a-sku.png)

5. Vytvořte nastavení přihlášení. Nastavení přihlášení lze opětovně použít nebo vytvořit nový. Toto nastavení obsahuje uživatelské jméno a heslo pro databázi.

    ![Vytvořte nové přihlašovací údaje databáze](./media/azure-stack-mysql-rp-deploy/create-new-login.png)

    Řetězec připojení zahrnuje název skutečné databázového serveru. Zkopírujte jej z portálu.

    ![Získání připojovacího řetězce pro databázi MySQL](./media/azure-stack-mysql-rp-deploy/mysql-db-created.png)

> [!NOTE]
> Délka uživatelská jména nesmí překročit 32 znaků s MySQL 5.7 nebo starších edic 16 znaků.


## <a name="add-capacity"></a>Přidat kapacity

Přidejte kapacity přidáním dalších serverů MySQL na portálu Azure zásobníku. Další servery, které lze přidat do nové nebo existující SKU. Ujistěte se, že vlastnosti serveru jsou stejné.


## <a name="make-mysql-databases-available-to-tenants"></a>Databáze MySQL zpřístupnit klientům
Vytvořte plány a nabízí databází MySQL zpřístupnit pro klienty. Přidání Microsoft.MySqlAdapter služby, přidejte kvóty atd.

![Vytvoření plánu a nabídky zahrnout databáze](./media/azure-stack-mysql-rp-deploy/mysql-new-plan.png)

## <a name="update-the-administrative-password"></a>Aktualizace hesla pro správu
Heslo můžete upravit tak, že první změníte na instanci serveru MySQL. Přejděte do **prostředky pro správu** &gt; **MySQL hostování servery** &gt; a klikněte na hostitelském serveru. V panelu nastavení klikněte na heslo.

![Aktualizovat heslo správce](./media/azure-stack-mysql-rp-deploy/mysql-update-password.png)

## <a name="update-the-mysql-resource-provider-adapter-multi-node-only-builds-1710-and-later"></a>Aktualizace adaptéru pro zprostředkovatele prostředků MySQL (více uzly jen sestavení 1710 a novější)
Při každé aktualizaci sestavení zásobník Azure, budou vydané nový adaptér zprostředkovatele prostředků MySQL. Zatímco existující adaptér může pokračovat v práci, se doporučuje aktualizovat na nejnovější verzi co nejdříve po aktualizaci zásobník Azure. Proces aktualizace je velmi podobný procesu instalace popsané výše. Vytvoří se nový virtuální počítač s nejnovější kód RP a nastavení se budou migrovat do této nové instance, včetně databáze a hostování informace o serveru, jakož i potřeby záznam DNS.

Pomocí skriptu UpdateMySQLProvider.ps1 s stejné argumenty jako výše. Také je nutné zadat certifikát zde.

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
. $tempDir\UpdateMySQLProvider.ps1 -AzCredential $AdminCreds `
  -VMLocalCredential $vmLocalAdminCreds `
  -CloudAdminCredential $cloudAdminCreds `
  -PrivilegedEndpoint $privilegedEndpoint `
  -DefaultSSLCertificatePassword $PfxPass `
  -DependencyFilesLocalPath $tempDir\cert `
  -AcceptLicense
 ```

### <a name="updatemysqlproviderps1-parameters"></a>Parametry UpdateMySQLProvider.ps1
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
| **AcceptLicense** | Přeskočí řádku tak, aby přijímal GPL licence (http://www.gnu.org/licenses/old-licenses/gpl-2.0.html) | |

## <a name="remove-the-mysql-resource-provider-adapter"></a>Odeberte adaptér zprostředkovatele prostředků MySQL

Odebrat zprostředkovatele prostředků, je nutné nejprve odebrat všechny závislosti.

1. Zkontrolujte, zda že máte původní balíček nasazení, který jste stáhli pro tuto verzi zprostředkovatele prostředků.

2. Od zprostředkovatele prostředků (to neodstraní data), je třeba odstranit všechny databáze klienta. To by měli provádět klientů sami.

3. Klienti musí registraci z oboru názvů.

4. Správce musí odstranit hostitelskými servery z adaptéru MySQL

5. Správce musí odstranit všechny plány, které odkazují na adaptér MySQL.

6. Správce musí odstranit žádné kvóty přidružené k adaptéru MySQL.

7. Spusťte znovu skript nasazení s-odinstalovat parametr, koncové body Azure Resource Manager, DirectoryTenantID a pověření pro účet správce služby.




## <a name="next-steps"></a>Další kroky

Zkuste jiné [PaaS služby](azure-stack-tools-paas-services.md) jako [poskytovatele prostředků SQL serveru](azure-stack-sql-resource-provider-deploy.md) a [poskytovatele prostředků App Services](azure-stack-app-service-overview.md).
