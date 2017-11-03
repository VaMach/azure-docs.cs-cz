---
title: "Připojení k Azure zásobníku pomocí rozhraní příkazového řádku | Microsoft Docs"
description: "Naučte se používat rozhraní příkazového řádku (CLI) a platformy pro správu a nasazení prostředků v Azure zásobníku"
services: azure-stack
documentationcenter: 
author: SnehaGunda
manager: byronr
editor: 
ms.assetid: f576079c-5384-4c23-b5a4-9ae165d1e3c3
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/18/2017
ms.author: sngun
ms.openlocfilehash: 5ef64e727615d17ae550efbc7ea427936d7d4c3b
ms.sourcegitcommit: b979d446ccbe0224109f71b3948d6235eb04a967
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/25/2017
---
# <a name="install-and-configure-cli-for-use-with-azure-stack"></a>Instalace a konfigurace rozhraní příkazového řádku pro použití s Azure zásobníku

V tomto článku jsme vás provede procesem pomocí rozhraní příkazového řádku Azure (CLI) ke správě prostředků Azure zásobníku Development Kit ze systému Linux a Mac klientských platforem. 

## <a name="export-the-azure-stack-ca-root-certificate"></a>Exportujte certifikát kořenové certifikační Autority Azure zásobníku

Pokud používáte rozhraní příkazového řádku z virtuálního počítače, který běží v prostředí Azure zásobníku Development Kit, zásobník Azure kořenový certifikát je již nainstalována ve virtuálním počítači, je možné ho načíst přímo. Pokud používáte rozhraní příkazového řádku z pracovní stanice mimo sadě pro vývoj, musíte exportovat certifikát kořenové certifikační Autority zásobník Azure z sadě pro vývoj a přidejte ho do úložiště certifikátů Python stanici vývoje (externí platformy Linux nebo Mac.). 

Pokud chcete exportovat certifikát od kořenové zásobník Azure ve formátu PEM, přihlaste development Kit a spusťte následující skript:

```powershell
   $label = "AzureStackSelfSignedRootCert"
   Write-Host "Getting certificate from the current user trusted store with subject CN=$label"
   $root = Get-ChildItem Cert:\CurrentUser\Root | Where-Object Subject -eq "CN=$label" | select -First 1
   if (-not $root)
   {
       Log-Error "Certificate with subject CN=$label not found"
       return
   }

   Write-Host "Exporting certificate"
   Export-Certificate -Type CERT -FilePath root.cer -Cert $root

   Write-Host "Converting certificate to PEM format"
   certutil -encode root.cer root.pem
```

## <a name="install-cli"></a>Instalace rozhraní příkazového řádku

V dalším Přihlaste se k vaší pracovní stanici a nainstalovat rozhraní příkazového řádku. Azure zásobníku vyžaduje 2.0 verzi rozhraní příkazového řádku Azure. Můžete nainstalovat pomocí kroků popsaných v [nainstalovat Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli) článku. Pokud chcete ověřit, pokud byla instalace úspěšná, otevřete okno příkazového řádku nebo terminál a spusťte následující příkaz:

```azurecli
az --version
```

Měli byste vidět verzi rozhraní příkazového řádku Azure a další závislé knihovny, které jsou nainstalovány ve vašem počítači.

## <a name="trust-the-azure-stack-ca-root-certificate"></a>Důvěřovat certifikátu kořenové certifikační Autority Azure zásobníku

Chcete-li důvěryhodný kořenový certifikát certifikační Autority zásobník Azure, připojte ji existující certifikát Python. Pokud používáte rozhraní příkazového řádku z počítače systému Linux, který je vytvořen v prostředí Azure zásobníku, spusťte následující příkaz bash:

```bash
sudo cat /var/lib/waagent/Certificates.pem >> ~/lib/azure-cli/lib/python2.7/site-packages/certifi/cacert.pem
```

Pokud používáte rozhraní příkazového řádku z počítače mimo prostředí výběrové potvrzování Azure, musíte nejprve nastavit [připojení VPN do Azure zásobníku](azure-stack-connect-azure-stack.md). Nyní zkopírujte certifikát PEM, který jste dříve exportovali na stanici vývoje a spusťte následující příkazy, v závislosti na vaší pracovní stanici operačního systému.

### <a name="linux"></a>Linux

```bash
sudo cat PATH_TO_PEM_FILE >> ~/lib/azure-cli/lib/python2.7/site-packages/certifi/cacert.pem
```

### <a name="macos"></a>macOS

```bash
sudo cat PATH_TO_PEM_FILE >> ~/lib/azure-cli/lib/python2.7/site-packages/certifi/cacert.pem
```

### <a name="windows"></a>Windows

```powershell
$pemFile = "<Fully qualified path to the PEM certificate Ex: C:\Users\user1\Downloads\root.pem>"

$root = New-Object System.Security.Cryptography.X509Certificates.X509Certificate2
$root.Import($pemFile)

Write-Host "Extracting needed information from the cert file"
$md5Hash    = (Get-FileHash -Path $pemFile -Algorithm MD5).Hash.ToLower()
$sha1Hash   = (Get-FileHash -Path $pemFile -Algorithm SHA1).Hash.ToLower()
$sha256Hash = (Get-FileHash -Path $pemFile -Algorithm SHA256).Hash.ToLower()

$issuerEntry  = [string]::Format("# Issuer: {0}", $root.Issuer)
$subjectEntry = [string]::Format("# Subject: {0}", $root.Subject)
$labelEntry   = [string]::Format("# Label: {0}", $root.Subject.Split('=')[-1])
$serialEntry  = [string]::Format("# Serial: {0}", $root.GetSerialNumberString().ToLower())
$md5Entry     = [string]::Format("# MD5 Fingerprint: {0}", $md5Hash)
$sha1Entry    = [string]::Format("# SHA1 Finterprint: {0}", $sha1Hash)
$sha256Entry  = [string]::Format("# SHA256 Fingerprint: {0}", $sha256Hash)
$certText = (Get-Content -Path root.pem -Raw).ToString().Replace("`r`n","`n")

$rootCertEntry = "`n" + $issuerEntry + "`n" + $subjectEntry + "`n" + $labelEntry + "`n" + `
$serialEntry + "`n" + $md5Entry + "`n" + $sha1Entry + "`n" + $sha256Entry + "`n" + $certText

Write-Host "Adding the certificate content to Python Cert store"
Add-Content "${env:ProgramFiles(x86)}\Microsoft SDKs\Azure\CLI2\Lib\site-packages\certifi\cacert.pem" $rootCertEntry

Write-Host "Python Cert store was updated for allowing the azure stack CA root certificate"
```

## <a name="set-up-the-virtual-machine-aliases-endpoint"></a>Nastavit koncový bod aliasy virtuálního počítače

Než uživatelé mohou vytvářet virtuální počítače pomocí rozhraní příkazového řádku, správce cloudu by měl nastavit veřejně přístupném koncovém bodu, který obsahuje aliasy bitové kopie virtuálního počítače a zaregistrujte tento koncový bod s cloudem. `endpoint-vm-image-alias-doc` Parametr `az cloud register` příkaz se používá pro tento účel. Můžou správci cloudové musíte stáhnout bitovou kopii do zásobníku Azure marketplace, předtím, než se přidat do bitové kopie aliasy koncového bodu.
   
Například Azure používá následující identifikátor URI: https://raw.githubusercontent.com/Azure/azure-rest-api-specs/master/arm-compute/quickstart-templates/aliases.json. Správce cloudu by měl nastavit koncový bod podobné pro zásobník Azure s obrázky, které jsou k dispozici v zásobníku Azure marketplace.

## <a name="connect-to-azure-stack"></a>Připojení ke službě Azure Stack

Pro připojení k Azure zásobníku použijte následující kroky:

1. Zaregistrovat prostředí Azure zásobníku spuštěním `az cloud register` příkaz.
   
   a. K registraci *cloudu správu* prostředí, použijte:

      ```azurecli
      az cloud register \ 
        -n AzureStackAdmin \ 
        --endpoint-resource-manager "https://adminmanagement.local.azurestack.external" \ 
        --suffix-storage-endpoint "local.azurestack.external" \ 
        --suffix-keyvault-dns ".adminvault.local.azurestack.external" \ 
        --endpoint-active-directory-graph-resource-id "https://graph.windows.net/" \
        --endpoint-vm-image-alias-doc <URI of the document which contains virtual machine image aliases>
      ```

   b. K registraci *uživatele* prostředí, použijte:

      ```azurecli
      az cloud register \ 
        -n AzureStackUser \ 
        --endpoint-resource-manager "https://management.local.azurestack.external" \ 
        --suffix-storage-endpoint "local.azurestack.external" \ 
        --suffix-keyvault-dns ".vault.local.azurestack.external" \ 
        --endpoint-active-directory-graph-resource-id "https://graph.windows.net/" \
        --endpoint-vm-image-alias-doc <URI of the document which contains virtual machine image aliases>
      ```

2. Nastavení aktivního prostředí pomocí následujících příkazů.

   a. Pro *cloudu správu* prostředí, použijte:

      ```azurecli
      az cloud set \
        -n AzureStackAdmin
      ```

   b. Pro *uživatele* prostředí, použijte:

      ```azurecli
      az cloud set \
        -n AzureStackUser
      ```

3. Aktualizujte konfiguraci prostředí použít profil pro konkrétní verzi rozhraní API zásobník Azure. Pokud chcete aktualizovat konfiguraci, spusťte následující příkaz:

   ```azurecli
   az cloud update \
     --profile 2017-03-09-profile
   ```

4. Přihlaste se k prostředí Azure zásobníku pomocí `az login` příkaz. Můžete se přihlásit do prostředí Azure zásobníku buď jako uživatel, nebo jako [instanční objekt](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-application-objects). 

   * Přihlaste se jako *uživatele*: můžete zadat uživatelské jméno a heslo přímo v rámci `az login` příkaz nebo ověřit pomocí prohlížeče. Je nutné provést k tomu, pokud má váš účet povolenou službou Multi-Factor authentication.

      ```azurecli
      az login \
        -u <Active directory global administrator or user account. For example: username@<aadtenant>.onmicrosoft.com> \
        --tenant <Azure Active Directory Tenant name. For example: myazurestack.onmicrosoft.com>
      ```

      > [!NOTE]
      > Pokud váš uživatelský účet má povolenou službou Multi-Factor authentication, můžete použít `az login command` bez zadání `-u` parametr. Spuštěním příkazu vám dává adresu URL a kód, který je nutné použít k ověření.
   
   * Přihlaste se jako *instanční objekt*: než se přihlásit, [vytvořit objekt služby prostřednictvím portálu Azure](azure-stack-create-service-principals.md) nebo rozhraní příkazového řádku a přiřaďte mu roli. Nyní přihlásit pomocí následujícího příkazu:

      ```azurecli
      az login \
        --tenant <Azure Active Directory Tenant name. For example: myazurestack.onmicrosoft.com> \
        --service-principal \
        -u <Application Id of the Service Principal> \
        -p <Key generated for the Service Principal>
      ```

## <a name="test-the-connectivity"></a>Testovací připojení

Teď, když jste všechno My instalační program, umožňuje vytvářet prostředky v rámci zásobníku Azure pomocí rozhraní příkazového řádku. Můžete například vytvořit skupinu prostředků pro aplikace a přidat virtuální počítač. Chcete-li vytvořit skupinu prostředků s názvem "MyResourceGroup" použijte následující příkaz:

```azurecli
az group create \
  -n MyResourceGroup -l local
```

Pokud skupina prostředků je úspěšně vytvořil, předchozí příkaz výstupy následující vlastnosti nově vytvořeného prostředku:

![Vytvoření skupiny prostředků výstup](media/azure-stack-connect-cli/image1.png)

## <a name="next-steps"></a>Další kroky

[Nasazení šablon pomocí rozhraní příkazového řádku Azure](azure-stack-deploy-template-command-line.md)

[Správa uživatelských oprávnění](azure-stack-manage-permissions.md)

