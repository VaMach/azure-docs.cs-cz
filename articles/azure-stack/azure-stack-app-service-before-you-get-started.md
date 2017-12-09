---
title: "Před nasazením služby App Service v zásobníku Azure | Microsoft Docs"
description: "Kroky k dokončení před nasazením služby App Service v Azure zásobníku"
services: azure-stack
documentationcenter: 
author: apwestgarth
manager: stefsch
editor: 
ms.assetid: 
ms.service: azure-stack
ms.workload: app-service
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/17/2017
ms.author: anwestg
ms.openlocfilehash: d6962bf2ffbf731a4aa301e663c7c7d3428080d4
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/08/2017
---
# <a name="before-you-get-started-with-app-service-on-azure-stack"></a>Před zahájením práce s App Service v Azure zásobníku

Azure App Service v zásobníku Azure nabízí řadu předběžné kroků, které je třeba dokončit před nasazením:

- Stažení Azure App Service na skriptů pomocné rutiny Azure zásobníku
- Vysoká dostupnost
- Certifikáty požadované pro službu Azure App Service v Azure zásobníku
- Příprava souborového serveru
- Příprava systému SQL Server
- Vytvoření aplikace Azure Active Directory
- Vytvoření aplikace Active Directory Federation Services

## <a name="download-the-azure-app-service-on-azure-stack-installer-and-helper-scripts"></a>Stažení Azure App Service na instalační program Azure zásobníku a skripty pomocné rutiny

1. Stažení [služby App Service na Azure zásobníku nasazení pomocníka skripty](https://aka.ms/appsvconmashelpers).
2. Stažení [služby App Service na Instalační službě Azure zásobníku](https://aka.ms/appsvconmasinstaller).
3. Extrahujte soubory ze souboru zip skripty pomocné rutiny. Následující soubory a struktura složek se zobrazí:
  - Common.ps1
  - Vytvoření AADIdentityApp.ps1
  - Vytvoření ADFSIdentityApp.ps1
  - Vytvoření AppServiceCerts.ps1
  - Get-AzureStackRootCert.ps1
  - Odebrat AppService.ps1
  - Moduly
    - GraphAPI.psm1
    
## <a name="high-availability"></a>Vysoká dostupnost

Aplikační služba Azure v zásobníku Azure není aktuálně moct nabízí vysokou dostupnost, protože zásobník Azure pouze nasadí úlohy do jednoho jedné domény selhání.

Příprava služby Azure App Service v zásobníku Azure pro vysokou dostupnost, je nutné nasadit požadované souborový Server a SQL Server v konfiguraci s vysokou dostupností. Pokud Azure zásobníku podporuje více domén selhání, poskytujeme pokyny pro povolení služby Azure App Service v Azure zásobníku v konfiguraci s vysokou dostupností.


## <a name="certificates-required-for-azure-app-service-on-azure-stack"></a>Certifikáty požadované pro službu Azure App Service v Azure zásobníku

### <a name="certificates-required-for-the-azure-stack-development-kit"></a>Certifikáty požadované pro Azure zásobníku Development Kit

Tento skript první spolupracuje se službou certifikační autority zásobník Azure k vytvoření čtyři certifikátů, které jsou potřebné službou App Service.

| Název souboru | Použití |
| --- | --- |
| _.appservice.Local.azurestack.external.pfx | Certifikát SSL výchozí služby App Service |
| API.appservice.Local.azurestack.external.pfx | Certifikát SSL rozhraní API služby App Service |
| FTP.appservice.Local.azurestack.external.pfx | Certifikát SSL služby App Service vydavatele |
| Sso.appservice.Local.azurestack.external.pfx | Certifikátu Identity aplikace služby App Service |

Spuštění skriptu na hostiteli Azure zásobníku Development Kit a ujistěte se, že používáte prostředí PowerShell jako azurestack\CloudAdmin.

1. V relaci prostředí PowerShell spuštěna jako azurestack\AzureStackAdmin spusťte skript vytvořit AppServiceCerts.ps1 ze složky, které jste extrahovali skripty pomocné rutiny. Tento skript vytvoří čtyři certifikáty ve stejné složce jako vytvořit certifikáty skript, který potřebuje služby App Service.
2. Zadejte heslo k zabezpečení soubory PFX a poznamenejte si ho. Musíte zadat ji ve službě App Service na Azure zásobníku Instalační služby.

#### <a name="create-appservicecertsps1-parameters"></a>Vytvoření AppServiceCerts.ps1 parametry

| Parametr | Požadované a volitelné | Výchozí hodnota | Popis |
| --- | --- | --- | --- |
| PfxPassword | Požaduje se | Hodnotu Null | Heslo použité k ochraně privátní klíč certifikátu |
| domainName | Požaduje se | Local.azurestack.external | Azure příponu zásobníku oblast a domény |

### <a name="certificates-required-for-a-production-deployment-of-azure-app-service-on-azure-stack"></a>Certifikáty požadované pro produkční nasazení služby Azure App Service v Azure zásobníku

Provoz poskytovatele prostředků v produkčním prostředí, je nutné zadat následující čtyři certifikáty:

#### <a name="default-domain-certificate"></a>Výchozí certifikát domény

Výchozí certifikát domény je umístěn v roli Front-endu. Uživatel aplikace se používá pro zástupný znak nebo výchozí domény požadavky na Azure App Service. Certifikát je také použít k řízení operací se zdrojovým (KUDU).

Certifikát musí být ve formátu .pfx a musí být certifikát se zástupným znakem dva subjektu. To umožňuje výchozí doménu a scm koncový bod pro operace zdroj ovládacího prvku na které se vztahují jeden certifikát.

| Formát | Příklad |
| --- | --- |
| \*.appservice. \<oblast\>.\< DomainName\>.\< rozšíření\> | \*. appservice.redmond.azurestack.external |
| \*. scm.appservice. <region>. <DomainName>.<extension> | \*. appservice.scm.redmond.azurestack.external |

#### <a name="api-certificate"></a>Certifikát rozhraní API

Certifikát rozhraní API je umístěn v roli správy a k zabezpečení volání rozhraní api se používá zprostředkovatel prostředků. Certifikát pro publikování musí obsahovat subjektem, který odpovídá položky DNS rozhraní API:

| Formát | Příklad |
| --- | --- |
| API.appservice. \<oblast\>.\< DomainName\>.\< rozšíření\> | API.appservice.redmond.azurestack.external |

#### <a name="publishing-certificate"></a>Publikování certifikátů

Certifikát pro roli vydavatele zabezpečuje FTPS přenos pro vlastníci aplikace při uložení obsahu.  Certifikát pro publikování musí obsahovat subjektem, který odpovídá položce FTPS DNS.

| Formát | Příklad |
| --- | --- |
| FTP.appservice. \<oblast\>.\< DomainName\>.\< rozšíření\> | API.appservice.redmond.azurestack.external |

#### <a name="identity-certificate"></a>Certifikát identity

Certifikát Identity aplikace umožňuje:
- integrace adresáře, zásobník Azure a služby App Service pro podporu integrace s poskytovatelem prostředků výpočetní AAD nebo AD FS
- Jednotné přihlašování scénáře pro pokročilé Developer Tools v rámci služby Azure App Service v Azure zásobníku.
Certifikát pro identitu musí obsahovat subjektem, který odpovídá následujícím formátu:

| Formát | Příklad |
| --- | --- |
| sso.appservice. \<oblast\>.\< DomainName\>.\< rozšíření\> | sso.appservice.redmond.azurestack.external |

### <a name="extract-the-azure-stack-azure-resource-manager-root-certificate"></a>Extrahujte certifikát kořenové Azure zásobník Azure Resource Manager

V relaci prostředí PowerShell spuštěna jako azurestack\CloudAdmin spusťte skript Get-AzureStackRootCert.ps1 ze složky, které jste extrahovali skripty pomocné rutiny. Tento skript vytvoří čtyři certifikáty ve stejné složce jako vytvořit certifikáty skript, který potřebuje služby App Service.

| Parametr Get-AzureStackRootCert.ps1 | Požadované a volitelné | Výchozí hodnota | Popis |
| --- | --- | --- | --- |
| PrivelegedEndpoint | Požaduje se | AzS ERCS01 | Privilegované koncový bod. |
| CloudAdminCredential | Požaduje se | AzureStack\CloudAdmin | Azure pověření účtu domény správce cloudu zásobníku |


## <a name="prepare-the-file-server"></a>Příprava souborového serveru

Aplikační služba Azure vyžaduje použití souborového serveru. Pro nasazení v produkčním prostředí musí být souborový Server nakonfigurovaný jako vysoce dostupný a umožňuje zpracovávat chyby.

Pro použití s Azure zásobníku Development Kit pouze nasazení, můžete použít tento příklad šablony nasazení Správce prostředků Azure pro nasazení souborového serveru nakonfigurovaný jeden uzel: https://aka.ms/appsvconmasdkfstemplate. Jeden uzel souborového serveru bude v pracovní skupině.

### <a name="provision-groups-and-accounts-in-active-directory"></a>Zřídit skupin a účtů ve službě Active Directory


1. Vytvořte následující skupiny globálního zabezpečení služby Active Directory:
    - FileShareOwners
    - FileShareUsers
2. Vytvořte následující účty služby Active Directory jako účty služeb:
    - Vlastník sdílené složky
    - FileShareUser

    Zabezpečení doporučujeme uživatelům pro tyto účty (a pro všechny webové role) musí být odlišný od sebe navzájem a mít silné uživatelská jména a hesla.
    Nastavení hesla s následující podmínky:
     - Povolit **heslo je platné stále**.
     - Povolit **uživatel nemůže změnit heslo**.
     - Zakázat **musí uživatel změnit heslo při příštím přihlášení**.
3. Přidejte účty na členství ve skupinách následujícím způsobem:
    - Přidat **vlastník sdílené složky** k **FileShareOwners** skupiny.
    - Přidat **FileShareUser** k **FileShareUsers** skupiny.

### <a name="provision-groups-and-accounts-in-a-workgroup"></a>Zřídit skupin a účtů v pracovní skupině.

>[!NOTE]
> Spustit všechny následující příkazy, při konfiguraci souborového serveru v relaci příkazového řádku správce.  **Nepoužívejte prostředí PowerShell.**

Při použití výše uvedené šablony Azure Resource Manageru, uživatelé jsou již vytvořeny.

1. Spusťte následující příkazy můžete vytvořit účty vlastník sdílené složky a FileShareUser. Nahraďte <password> vlastními hodnotami.
``` DOS
net user FileShareOwner <password> /add /expires:never /passwordchg:no
net user FileShareUser <password> /add /expires:never /passwordchg:no
```
2. Nastavení hesla pro účty bez vypršení platnosti spuštěním následujících příkazů WMIC:
``` DOS
WMIC USERACCOUNT WHERE "Name='FileShareOwner'" SET PasswordExpires=FALSE
WMIC USERACCOUNT WHERE "Name='FileShareUser'" SET PasswordExpires=FALSE
```
3. Vytvořte místní skupiny FileShareUsers a FileShareOwners a do nich přidat účty v prvním kroku.
``` DOS
net localgroup FileShareUsers /add
net localgroup FileShareUsers FileShareUser /add
net localgroup FileShareOwners /add
net localgroup FileShareOwners FileShareOwner /add
```

### <a name="provision-the-content-share"></a>Zřídit sdílené složce obsahu

Sdílené složce obsahu obsahuje obsah webu klienta. Postup přidělení sdílené složky obsahu na jeden souborový server je stejný pro prostředí služby Active Directory a pracovní skupiny, ale jiné pro cluster s podporou převzetí služeb při selhání ve službě Active Directory.

#### <a name="provision-the-content-share-on-a-single-file-server-ad-or-workgroup"></a>Zřídit sdílené složky obsahu na jeden souborový server (AD nebo pracovní skupině)

Na jeden souborový server, na příkazovém řádku se zvýšenými oprávněními spusťte následující příkazy. Nahraďte hodnota pro 'C:\WebSites' odpovídající cesty ve vašem prostředí.

```DOS
set WEBSITES_SHARE=WebSites
set WEBSITES_FOLDER=C:\WebSites
md %WEBSITES_FOLDER%
net share %WEBSITES_SHARE% /delete
net share %WEBSITES_SHARE%=%WEBSITES_FOLDER% /grant:Everyone,full
```

### <a name="to-enable-winrm-add-the-fileshareowners-group-to-the-local-administrators-group"></a>Pokud chcete povolit WinRM, přidejte skupinu FileShareOwners do místní skupiny Administrators

Aby Vzdálená správa systému Windows fungovat správně musíte přidat FileShareOwners skupiny do místní skupiny Administrators.

#### <a name="active-directory"></a>Active Directory

Na souborovém serveru nebo na všech uzlech clusteru převzetí služeb při selhání souborového serveru, spusťte na příkazovém řádku se zvýšenými oprávněními následující příkazy. Nahraďte hodnotu pro <DOMAIN> s názvem domény, kterou chcete použít.

```DOS
set DOMAIN=<DOMAIN>
net localgroup Administrators %DOMAIN%\FileShareOwners /add
```

#### <a name="workgroup"></a>Pracovní skupiny

Spusťte následující příkaz na příkazovém řádku se zvýšenými oprávněními na souborovém serveru.

```DOS
net localgroup Administrators FileShareOwners /add
```

### <a name="configure-access-control-to-the-shares"></a>Konfigurace řízení přístupu ke sdíleným složkám

Na příkazovém řádku se zvýšenými oprávněními spusťte následující příkazy, na souborovém serveru nebo na uzlu clusteru převzetí služeb při selhání souborového serveru, který je aktuální vlastník prostředku clusteru. Nahraďte hodnoty kurzívou hodnoty, které jsou specifické pro vaše prostředí.

#### <a name="active-directory"></a>Active Directory
```DOS
set DOMAIN=<DOMAIN>
set WEBSITES_FOLDER=C:\WebSites
icacls %WEBSITES_FOLDER% /reset
icacls %WEBSITES_FOLDER% /grant Administrators:(OI)(CI)(F)
icacls %WEBSITES_FOLDER% /grant %DOMAIN%\FileShareOwners:(OI)(CI)(M)
icacls %WEBSITES_FOLDER% /inheritance:r
icacls %WEBSITES_FOLDER% /grant %DOMAIN%\FileShareUsers:(CI)(S,X,RA)
icacls %WEBSITES_FOLDER% /grant *S-1-1-0:(OI)(CI)(IO)(RA,REA,RD)
```

#### <a name="workgroup"></a>Pracovní skupiny
```DOS
set WEBSITES_FOLDER=C:\WebSites
icacls %WEBSITES_FOLDER% /reset
icacls %WEBSITES_FOLDER% /grant Administrators:(OI)(CI)(F)
icacls %WEBSITES_FOLDER% /grant FileShareOwners:(OI)(CI)(M)
icacls %WEBSITES_FOLDER% /inheritance:r
icacls %WEBSITES_FOLDER% /grant FileShareUsers:(CI)(S,X,RA)
icacls %WEBSITES_FOLDER% /grant *S-1-1-0:(OI)(CI)(IO)(RA,REA,RD)
```

## <a name="prepare-the-sql-server"></a>Příprava systému SQL Server

Pro Azure App Service na hostování zásobník Azure a měření databáze musíte připravit SQL serveru pro uchování databáze Azure App Service.

Pro použití s Development Kit zásobník Azure, můžete použít SQL Express 2014 SP2 nebo novější.

Pro produkční a účely vysokou dostupnost, doporučujeme použít s plnou verzí systému SQL 2014 SP2 nebo novější, povolit smíšený režim ověřování a nasadit v [vysoce dostupné konfiguraci](https://docs.microsoft.com/sql/sql-server/failover-clusters/high-availability-solutions-sql-server).

Azure App Service na serveru SQL Azure zásobníku musí být přístupné ze všech rolí služby App Service. SQL Server se dá nasadit v rámci předplatného výchozí zprostředkovatel v zásobníku Azure. Můžete taky použít stávající infrastruktury v rámci vaší organizace (Pokud je k dispozici připojení k Azure zásobníku). Pokud používáte image Azure Marketplace, nezapomeňte nakonfigurovat bránu firewall, odpovídajícím způsobem. 

Pro všechny role systému SQL Server můžete použít výchozí instance nebo pojmenované instance. Pokud používáte pojmenovanou instanci, ale být se, že ručně spusťte službu SQL Browser a otevřete port 1434.

## <a name="create-an-azure-active-directory-application"></a>Vytvoření aplikace Azure Active Directory

Konfigurace objektu zabezpečení služby Azure AD pro podporu následujících:
- Škálovací sady virtuálních počítačů integrace v vrstvách pracovního procesu.
- Jednotné přihlašování pro nástroje portálu a pokročilé vývojáře Azure Functions.

Tento postup platí pro Azure AD, které jsou zabezpečené jenom prostředí Azure zásobníku.

Správci musí nakonfigurovat jednotné přihlašování k:
- Povolte rozšířené vývojářské nástroje v rámci služby App Service (Kudu).
- Povolte používání prostředí portálu Azure Functions.

Postupujte následovně:

1. Otevřete prostředí PowerShell instance jako azurestack\AzureStackAdmin.
2. Přejděte do umístění stažené a rozbalené v skripty [požadovaných krok](https://docs.microsoft.com/azure/azure-stack/azure-stack-app-service-before-you-get-started#download-the-azure-app-service-on-azure-stack-installer-and-helper-scripts).
3. [Instalace prostředí PowerShell Azure zásobníku](azure-stack-powershell-install.md).
4. Spustit **vytvořit AADIdentityApp.ps1** skriptu. Když se zobrazí výzva k vaše ID klienta Azure AD, zadejte ID klienta Azure AD, kterou používáte pro vaše nasazení zásobník Azure, například myazurestack.onmicrosoft.com.
5. V **pověření** okno, zadejte účet správce služby Azure AD a heslo. Klikněte na **OK**.
6. Zadejte heslo certifikátu pro a cesta k souboru certifikátu [certifikát vytvořený dříve](https://docs.microsoft.com/en-gb/azure/azure-stack/azure-stack-app-service-before-you-get-started#certificates-required-for-azure-app-service-on-azure-stack). Certifikát vytvořený pro tento krok ve výchozím nastavení je **sso.appservice.local.azurestack.external.pfx**.
7. Tento skript vytvoří novou aplikaci v klientovi Azure AD. Poznamenejte si ID aplikace, která je vrácena ve výstupu prostředí PowerShell. Je třeba tyto informace během instalace.
8. Otevřete nové okno prohlížeče a přihlaste se k portálu Azure (portal.azure.com) jako **správce Azure Active Directory Service**.
9. Otevřete poskytovatel prostředků Azure AD.
10. Klikněte na tlačítko **registrace aplikace**.
11. Vyhledejte **ID aplikace** vrátí jako součást kroku 7. Aplikace služby App Service je uveden.
12. Klikněte na tlačítko **aplikace** v seznamu
13. Klikněte na tlačítko **požadovaná oprávnění** > **udělit oprávnění** > **Ano**.

| Vytvoření AADIdentityApp.ps1 parametr | Požadované a volitelné | Výchozí hodnota | Popis |
| --- | --- | --- | --- |
| DirectoryTenantName | Požaduje se | Hodnotu Null | Azure AD ID klienta. Zadejte identifikátor GUID nebo řetězec, například myazureaaddirectory.onmicrosoft.com |
| AdminArmEndpoint | Požaduje se | Hodnotu Null | Správce Azure Resource Manager koncový bod, například adminmanagement.local.azurestack.external |
| TenantARMEndpoint | Požaduje se | Hodnotu Null | Klienta koncový bod Azure Resource Manager, například: management.local.azurestack.external |
| AzureStackAdminCredential | Požaduje se | Hodnotu Null | Přihlašovací údaje Správce služby Azure AD |
| CertificateFilePath | Požaduje se | Hodnotu Null | Cesta k souboru certifikátu aplikace identity generované dříve. |
| CertificatePassword | Požaduje se | Hodnotu Null | Heslo použité k ochraně privátní klíč certifikátu. |

## <a name="create-an-active-directory-federation-services-application"></a>Vytvoření aplikace Active Directory Federation Services

Pro prostředí Azure zásobníku zabezpečeným službou AD FS musíte nakonfigurovat objektu služby AD FS pro podporu následujících:
- Škálovací sady virtuálních počítačů integrace v vrstvách pracovního procesu.
- Jednotné přihlašování pro nástroje portálu a pokročilé vývojáře Azure Functions.

Správci musí nakonfigurovat jednotné přihlašování k:
- Nakonfigurujte hlavní název služby pro integraci sady škálování virtuálního počítače v pracovní vrstvách.
- Povolte rozšířené vývojářské nástroje v rámci služby App Service (Kudu).
- Povolte používání prostředí portálu Azure Functions.

Postupujte následovně:

1. Otevřete prostředí PowerShell instance jako azurestack\azurestackadmin.
2. Přejděte do umístění stažené a rozbalené v skripty [požadovaných krok](https://docs.microsoft.com/en-gb/azure/azure-stack/azure-stack-app-service-before-you-get-started#download-the-azure-app-service-on-azure-stack-installer-and-helper-scripts).
3. [Instalace prostředí PowerShell Azure zásobníku](azure-stack-powershell-install.md).
4.  Spustit **vytvořit ADFSIdentityApp.ps1** skriptu.
5.  V **pověření** okno, zadejte účet správce cloudové služby AD FS a heslo. Klikněte na **OK**.
6.  Zadejte heslo certifikátu pro a cesta k souboru certifikátu [certifikát vytvořený dříve](https://docs.microsoft.com/en-gb/azure/azure-stack/azure-stack-app-service-before-you-get-started#certificates-required-for-azure-app-service-on-azure-stack). Certifikát vytvořený pro tento krok ve výchozím nastavení je sso.appservice.local.azurestack.external.pfx.

| Vytvoření ADFSIdentityApp.ps1 parametr | Požadované a volitelné | Výchozí hodnota | Popis |
| --- | --- | --- | --- |
| AdminArmEndpoint | Požaduje se | Hodnotu Null | Koncový bod správce Azure Resource Manager. Například adminmanagement.local.azurestack.external. |
| PrivilegedEndpoint | Požaduje se | Hodnotu Null | Privilegované koncový bod. Například AzS ERCS01. |
| CloudAdminCredential | Požaduje se | Hodnotu Null | Azure zásobníku cloudadmin domény pověření účtu. Například Azurestack\CloudAdmin. |
| CertificateFilePath | Požaduje se | Hodnotu Null | Cesta k souboru certifikátu PFX identity aplikace. |
| CertificatePassword | Požaduje se | Hodnotu Null | Heslo použité k ochraně privátní klíč certifikátu. |


## <a name="next-steps"></a>Další kroky

[Nainstalujte zprostředkovatele prostředků služby App Service](azure-stack-app-service-deploy.md).
