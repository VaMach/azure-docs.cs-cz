---
title: "Vytvoření clusteru služby Azure Service Fabric ze šablony | Microsoft Docs"
description: "Tento článek popisuje postup nastavení zabezpečení clusteru Service Fabric v Azure pomocí Azure Resource Manager, Azure Key Vault a Azure Active Directory (Azure AD) pro ověřování klientů."
services: service-fabric
documentationcenter: .net
author: chackdan
manager: timlt
editor: chackdan
ms.assetid: 15d0ab67-fc66-4108-8038-3584eeebabaa
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/22/2017
ms.author: chackdan
ms.openlocfilehash: 47152d05eb7e31e7fe1f35e33a10fe8e903e21e2
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="create-a-service-fabric-cluster-by-using-azure-resource-manager"></a>Vytvořit cluster Service Fabric pomocí Azure Resource Manager
> [!div class="op_single_selector"]
> * [Azure Resource Manager](service-fabric-cluster-creation-via-arm.md)
> * [Azure Portal](service-fabric-cluster-creation-via-portal.md)
>
>

Tento podrobný průvodce vás provede procesem nastavení zabezpečení clusteru Azure Service Fabric v Azure pomocí Azure Resource Manager. Jsme na vědomí, že článek je dlouhá. Nicméně pokud jste již důkladně obeznámení s obsahem, je nutné pečlivě postupujte podle následujících kroků.

Průvodce zahrnuje následující postupy:

* Nastavení klíče trezoru služby Azure k nahrání certifikátů pro zabezpečení clusteru a aplikace
* Vytvoření clusteru s podporou zabezpečené v Azure pomocí Azure Resource Manager
* Ověřování uživatelů pomocí služby Azure Active Directory (Azure AD) pro správu clusteru

Cluster s podporou zabezpečení je cluster, který brání neoprávněným přístupem k operace správy. To zahrnuje nasazení, upgrade a odstranění aplikací, služeb a data, která obsahují. Nezabezpečená clusteru je cluster každý, kdo může připojit k kdykoli a provádět operace správy. Přestože je možné vytvořit nezabezpečená clusteru, důrazně doporučujeme vytvořit cluster s podporou zabezpečení od samého počátku. Protože nezabezpečená clusteru nelze zabezpečit později, je nutné vytvořit nový cluster.

Koncept vytváření zabezpečené clusterů je stejný, ať už jsou Linux nebo Windows clusterů. Další informace a pomocné rutiny skripty pro vytvoření zabezpečeného clusterů systému Linux, najdete v části [vytváření zabezpečené clusterů v systému Linux](#secure-linux-clusters).

## <a name="sign-in-to-your-azure-account"></a>Přihlášení k účtu Azure
Tato příručka používá [prostředí Azure PowerShell][azure-powershell]. Když spustíte novou relaci prostředí PowerShell, přihlaste se k účtu Azure a vybrat své předplatné před spuštěním příkazů Azure.

Přihlaste se k účtu Azure:

```powershell
Login-AzureRmAccount
```

Vyberte předplatné:

```powershell
Get-AzureRmSubscription
Set-AzureRmContext -SubscriptionId <guid>
```

## <a name="set-up-a-key-vault"></a>Nastavení trezoru klíčů
Tato část obsahuje informace o vytvoření trezoru klíčů pro cluster Service Fabric v Azure a aplikace Service Fabric. Dokončení Průvodce Azure Key Vault, najdete v části [Key Vault Příručka Začínáme][key-vault-get-started].

Service Fabric používá certifikáty X.509 k zabezpečení clusteru a poskytují funkce zabezpečení aplikací. Používáte ke správě certifikátů pro clusterů Service Fabric v Azure Key Vault. Pokud cluster je nasazené v Azure, poskytovatel prostředků Azure, která je odpovědná za vytváření clusterů Service Fabric vyžaduje certifikáty od Key Vault a nainstaluje je v clusteru virtuálních počítačů.

Následující diagram znázorňuje vztah mezi Azure Key Vault, cluster Service Fabric a poskytovatel prostředků Azure, která používá certifikáty uložené v trezoru klíčů při vytváření clusteru:

![Diagram instalace certifikátu][cluster-security-cert-installation]

### <a name="create-a-resource-group"></a>Vytvoření skupiny prostředků
Prvním krokem je vytvoření skupiny prostředků speciálně pro váš trezor klíčů. Doporučujeme převést trezoru klíčů do vlastní skupiny prostředků. Tato akce umožňuje odstranit výpočetního prostředí a úložiště skupiny prostředků, včetně skupinu prostředků, která obsahuje daný cluster Service Fabric bez ztráty klíče a tajné klíče. Skupinu prostředků, která obsahuje váš trezor klíčů _musí být ve stejné oblasti_ jako cluster, který se používá.

Pokud plánujete nasadit clustery v několika oblastech, doporučujeme tento je název skupiny prostředků a klíč trezoru způsobem, který určuje, které oblasti patří do.  

```powershell

    New-AzureRmResourceGroup -Name westus-mykeyvault -Location 'West US'
```
Výstup by měl vypadat takto:

```powershell

    WARNING: The output object type of this cmdlet is going to be modified in a future release.

    ResourceGroupName : westus-mykeyvault
    Location          : westus
    ProvisioningState : Succeeded
    Tags              :
    ResourceId        : /subscriptions/<guid>/resourceGroups/westus-mykeyvault

```
<a id="new-key-vault"></a>

### <a name="create-a-key-vault-in-the-new-resource-group"></a>Vytvoření trezoru klíčů do nové skupiny prostředků
Trezor klíčů _musí být povolen pro nasazení_ umožňující poskytovatele výpočetních prostředků z něj získat certifikáty a nainstalujte ji na instancí virtuálního počítače:

```powershell

    New-AzureRmKeyVault -VaultName 'mywestusvault' -ResourceGroupName 'westus-mykeyvault' -Location 'West US' -EnabledForDeployment

```

Výstup by měl vypadat takto:

```powershell

    Vault Name                       : mywestusvault
    Resource Group Name              : westus-mykeyvault
    Location                         : West US
    Resource ID                      : /subscriptions/<guid>/resourceGroups/westus-mykeyvault/providers/Microsoft.KeyVault/vaults/mywestusvault
    Vault URI                        : https://mywestusvault.vault.azure.net
    Tenant ID                        : <guid>
    SKU                              : Standard
    Enabled For Deployment?          : False
    Enabled For Template Deployment? : False
    Enabled For Disk Encryption?     : False
    Access Policies                  :
                                       Tenant ID                :    <guid>
                                       Object ID                :    <guid>
                                       Application ID           :
                                       Display Name             :    
                                       Permissions to Keys      :    get, create, delete, list, update, import, backup, restore
                                       Permissions to Secrets   :    all


    Tags                             :
```
<a id="existing-key-vault"></a>

## <a name="use-an-existing-key-vault"></a>Použít existující trezor klíčů

Chcete-li použít existující trezor klíčů, můžete _ji musíte povolit pro nasazení_ umožňující poskytovatele výpočetních prostředků z něj získat certifikáty a nainstalujte ji na uzlech clusteru:

```powershell

Set-AzureRmKeyVaultAccessPolicy -VaultName 'ContosoKeyVault' -EnabledForDeployment

```

<a id="add-certificate-to-key-vault"></a>

## <a name="add-certificates-to-your-key-vault"></a>Přidejte certifikáty do trezoru klíčů

Ve službě Service Fabric se k ověřování a šifrování pro zabezpečení různých aspektů clusteru a jeho aplikací využívají certifikáty. Další informace o tom, jak certifikáty se používají v Service Fabric najdete v tématu [scénáře zabezpečení clusteru Service Fabric][service-fabric-cluster-security].

### <a name="cluster-and-server-certificate-required"></a>Cluster a server certifikátu (povinné)
Tento certifikát je vyžadován k zabezpečení clusteru a zabránit neoprávněnému přístupu k němu. Poskytuje zabezpečení clusteru dvěma způsoby:

* Ověřování clusteru: ověřuje komunikaci mezi uzly pro cluster federaci. Pouze uzly, které můžete prokázání své identity s tímto certifikátem může připojit ke clusteru.
* Ověření serveru: ověřuje koncové body správy clusteru klient pro správu, tak, aby klient správy zná je rozhovoru s skutečné clusteru. Tento certifikát také poskytuje protokolem SSL pro rozhraní API pro správu protokolu HTTPS a pro Service Fabric Explorer přes protokol HTTPS.

Poskytovat tyto účely, certifikát musí splňovat následující požadavky:

* Certifikát musí obsahovat privátní klíč.
* Certifikát se musí vytvořit pro výměnu klíčů, což je exportovat do souboru Personal Information Exchange (.pfx).
* Název subjektu certifikátu musí odpovídat domény, který používáte pro přístup ke clusteru Service Fabric. Toto porovnání se vyžaduje k zajištění protokolem SSL pro koncové body správy protokolu HTTPS a Service Fabric Explorer clusteru. Nelze získat certifikát SSL od certifikační autority (CA) pro. cloudapp.azure.com domény. Je nutné získat vlastní název domény pro váš cluster. Pokud budete požadovat certifikát od certifikační Autority, název subjektu certifikátu musí odpovídat názvu vlastní domény, který používáte pro váš cluster.

### <a name="application-certificates-optional"></a>Certifikáty aplikace (volitelné)
Libovolný počet dalších certifikátů lze nainstalovat na clusteru pro aplikace z bezpečnostních důvodů. Před vytvořením clusteru, vezměte v úvahu scénáře zabezpečení aplikací, které vyžadují certifikát nainstalovaný na uzlech, například:

* Šifrování a dešifrování hodnot konfigurace aplikace.
* Šifrování dat mezi uzly během replikace.

### <a name="formatting-certificates-for-azure-resource-provider-use"></a>Certifikáty formátování pro použití poskytovatele prostředků Azure.
Můžete přidat a používat soubory privátních klíčů (.pfx) přímo prostřednictvím trezoru klíčů. Poskytovatel prostředků Azure výpočetní však vyžaduje klíče ukládaly ve speciální formátu JavaScript Object Notation (JSON). Formát obsahuje soubor .pfx jako řetězec s kódováním 64 základní a heslo soukromého klíče. Abychom vyhověli tyto požadavky, klíče musí být umístěn do řetězce formátu JSON a pak uloženy jako "tajemství" v trezoru klíčů.

Pro usnadnění tohoto procesu [modul prostředí PowerShell je dostupná na Githubu][service-fabric-rp-helpers]. Pokud chcete používat modul, postupujte takto:

1. Stáhněte si celý obsah úložiště do místního adresáře.
2. Přejděte do místního adresáře.
2. Naimportujte modul ServiceFabricRPHelpers v okně prostředí PowerShell:

```powershell

 Import-Module "C:\..\ServiceFabricRPHelpers\ServiceFabricRPHelpers.psm1"

```

`Invoke-AddCertToKeyVault` Příkaz v tento modul prostředí PowerShell automaticky formáty privátní klíč certifikátu do řetězce formátu JSON a odešle ji do trezoru klíčů. Pomocí příkazu Přidat certifikát clusteru a všechny další aplikaci certifikáty do trezoru klíčů. Tento krok opakujte pro všechny další certifikáty, které chcete nainstalovat v clusteru.

#### <a name="uploading-an-existing-certificate"></a>Odesílání existující certifikát

```powershell

 Invoke-AddCertToKeyVault -SubscriptionId <guid> -ResourceGroupName westus-mykeyvault -Location "West US" -VaultName mywestusvault -CertificateName mycert -Password "<password>" -UseExistingCertificate -ExistingPfxFilePath "C:\path\to\mycertkey.pfx"

```

Pokud dojde k chybě, jako je například znázorněno zde, obvykle to znamená, že máte konflikt adresy URL prostředku. Konflikt vyřešit, změňte název trezoru klíčů.

```
Set-AzureKeyVaultSecret : The remote name could not be resolved: 'westuskv.vault.azure.net'
At C:\Users\chackdan\Documents\GitHub\Service-Fabric\Scripts\ServiceFabricRPHelpers\ServiceFabricRPHelpers.psm1:440 char:11
+ $secret = Set-AzureKeyVaultSecret -VaultName $VaultName -Name $Certif ...
+           ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : CloseError: (:) [Set-AzureKeyVaultSecret], WebException
    + FullyQualifiedErrorId : Microsoft.Azure.Commands.KeyVault.SetAzureKeyVaultSecret

```

Po vyřešení konfliktu výstup by měl vypadat přibližně takto:

```

    Switching context to SubscriptionId <guid>
    Ensuring ResourceGroup westus-mykeyvault in West US
    WARNING: The output object type of this cmdlet is going to be modified in a future release.
    Using existing value mywestusvault in West US
    Reading pfx file from C:\path\to\key.pfx
    Writing secret to mywestusvault in vault mywestusvault


Name  : CertificateThumbprint
Value : E21DBC64B183B5BF355C34C46E03409FEEAEF58D

Name  : SourceVault
Value : /subscriptions/<guid>/resourceGroups/westus-mykeyvault/providers/Microsoft.KeyVault/vaults/mywestusvault

Name  : CertificateURL
Value : https://mywestusvault.vault.azure.net:443/secrets/mycert/4d087088df974e869f1c0978cb100e47

```

>[!NOTE]
>Musíte tři předchozí řetězců, CertificateThumbprint, SourceVault a CertificateURL, chcete-li nastavení zabezpečení clusteru Service Fabric a získat všechny certifikáty aplikace, které používáte pro zabezpečení aplikací. Pokud neuložíte řetězce, může být obtížné je znovu načíst pomocí dotazu trezoru klíčů později.

<a id="add-self-signed-certificate-to-key-vault"></a>

#### <a name="creating-a-self-signed-certificate-and-uploading-it-to-the-key-vault"></a>Vytvořit certifikát podepsaný svým držitelem a pak ho nahrát do trezoru klíčů

Pokud jste již odeslali certifikáty do trezoru klíčů, tento krok přeskočte. Tento krok je pro generování nový certifikát podepsaný svým držitelem a pak ho nahrát do trezoru klíčů. Po změně parametrů v následující skript a potom ho spusťte, vám měla zobrazit výzva k zadání hesla certifikátu.  

```powershell

$ResourceGroup = "chackowestuskv"
$VName = "chackokv2"
$SubID = "6c653126-e4ba-42cd-a1dd-f7bf96ae7a47"
$locationRegion = "westus"
$newCertName = "chackotestcertificate1"
$dnsName = "www.mycluster.westus.mydomain.com" #The certificate's subject name must match the domain used to access the Service Fabric cluster.
$localCertPath = "C:\MyCertificates" # location where you want the .PFX to be stored

 Invoke-AddCertToKeyVault -SubscriptionId $SubID -ResourceGroupName $ResourceGroup -Location $locationRegion -VaultName $VName -CertificateName $newCertName -CreateSelfSignedCertificate -DnsName $dnsName -OutputPath $localCertPath

```

Pokud dojde k chybě, jako je například znázorněno zde, obvykle to znamená, že máte konflikt adresy URL prostředku. Konflikt vyřešit, změňte název trezoru klíčů, název RG a tak dále.

```
Set-AzureKeyVaultSecret : The remote name could not be resolved: 'westuskv.vault.azure.net'
At C:\Users\chackdan\Documents\GitHub\Service-Fabric\Scripts\ServiceFabricRPHelpers\ServiceFabricRPHelpers.psm1:440 char:11
+ $secret = Set-AzureKeyVaultSecret -VaultName $VaultName -Name $Certif ...
+           ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : CloseError: (:) [Set-AzureKeyVaultSecret], WebException
    + FullyQualifiedErrorId : Microsoft.Azure.Commands.KeyVault.SetAzureKeyVaultSecret

```

Po vyřešení konfliktu výstup by měl vypadat přibližně takto:

```
PS C:\Users\chackdan\Documents\GitHub\Service-Fabric\Scripts\ServiceFabricRPHelpers> Invoke-AddCertToKeyVault -SubscriptionId $SubID -ResourceGroupName $ResouceGroup -Location $locationRegion -VaultName $VName -CertificateName $newCertName -Password $certPassword -CreateSelfSignedCertificate -DnsName $dnsName -OutputPath $localCertPath
Switching context to SubscriptionId 6c343126-e4ba-52cd-a1dd-f8bf96ae7a47
Ensuring ResourceGroup chackowestuskv in westus
WARNING: The output object type of this cmdlet will be modified in a future release.
Creating new vault westuskv1 in westus
Creating new self signed certificate at C:\MyCertificates\chackonewcertificate1.pfx
Reading pfx file from C:\MyCertificates\chackonewcertificate1.pfx
Writing secret to chackonewcertificate1 in vault westuskv1


Name  : CertificateThumbprint
Value : 96BB3CC234F9D43C25D4B547sd8DE7B569F413EE

Name  : SourceVault
Value : /subscriptions/6c653126-e4ba-52cd-a1dd-f8bf96ae7a47/resourceGroups/chackowestuskv/providers/Microsoft.KeyVault/vaults/westuskv1

Name  : CertificateURL
Value : https://westuskv1.vault.azure.net:443/secrets/chackonewcertificate1/ee247291e45d405b8c8bbf81782d12bd

```

>[!NOTE]
>Musíte tři předchozí řetězců, CertificateThumbprint, SourceVault a CertificateURL, chcete-li nastavení zabezpečení clusteru Service Fabric a získat všechny certifikáty aplikace, které používáte pro zabezpečení aplikací. Pokud neuložíte řetězce, může být obtížné je znovu načíst pomocí dotazu trezoru klíčů později.

 V tomto okamžiku byste měli mít zavedené následující prvky:

* Skupina prostředků trezoru klíčů.
* Trezor klíčů a jeho adresa URL (volané SourceVault v předchozí výstup prostředí PowerShell).
* Ověřovací certifikát serverů clusteru a jeho adresa URL v trezoru klíčů.
* Certifikáty aplikací a jejich adresy URL v trezoru klíčů.


<a id="add-AAD-for-client"></a>

## <a name="set-up-azure-active-directory-for-client-authentication"></a>Nastavení Azure Active Directory pro ověřování klientů

Azure AD umožňuje spravovat přístup uživatelů k aplikacím organizace (označované jako klienty). Aplikace se dál dělí na ty s webové přihlášení uživatelského rozhraní a ty prostředí nativního klienta. V tomto článku předpokládáme, že jste již vytvořili klienta. Pokud máte není, spusťte načtením [postup získání klienta Azure Active Directory][active-directory-howto-tenant].

Cluster Service Fabric nabízí několik vstupní body k jeho funkce správy, včetně webové [Service Fabric Explorer] [ service-fabric-visualizing-your-cluster] a [Visual Studio] [ service-fabric-manage-application-in-visual-studio]. V důsledku toho můžete vytvořit dvě aplikace Azure AD k řízení přístupu do clusteru, jednu webovou aplikaci a jeden nativní aplikace.

Pro zjednodušení některé kroky v konfiguraci Azure AD se cluster Service Fabric, jsme vytvořili sadu skriptů prostředí Windows PowerShell.

> [!NOTE]
> Následující kroky musí dokončit před vytvořením clusteru. Skripty očekává koncové body a názvy clusterů, hodnoty by měla být plánované a není hodnoty, že jste již vytvořili.

1. [Stáhněte si skripty] [ sf-aad-ps-script-download] do vašeho počítače.
2. Klikněte pravým tlačítkem na soubor zip, vyberte **vlastnosti**, vyberte **Odblokovat** zaškrtněte políčko a potom klikněte na **použít**.
3. Rozbalte soubor zip.
4. Spustit `SetupApplications.ps1`a zadejte TenantId, název clusteru a WebApplicationReplyUrl jako parametry. Například:

    ```powershell
    .\SetupApplications.ps1 -TenantId '690ec069-8200-4068-9d01-5aaf188e557a' -ClusterName 'mycluster' -WebApplicationReplyUrl 'https://mycluster.westus.cloudapp.azure.com:19080/Explorer/index.html'
    ```

    Vaše ID Tenanta můžete najít spuštěním příkazu Powershellu `Get-AzureSubscription`. Provádění tento příkaz zobrazí TenantId pro každé předplatné.

    Název clusteru se používá jako předpona aplikace Azure AD, které jsou vytvořené pomocí skriptu. Nemusí se přesně shodovat s názvem skutečné clusteru. Je určena pouze k usnadňují mapování na cluster Service Fabric, který se používá s artefakty Azure AD.

    WebApplicationReplyUrl je výchozí koncový bod, který vrátí Azure AD pro uživatele po jejich dokončení přihlašování. Nastavte tento koncový bod jako koncový bod Service Fabric Explorer pro váš cluster, který ve výchozím nastavení je:

    https://&lt;cluster_domain&gt;: 19080/Explorer

    Zobrazí se výzva k přihlášení k účtu, který má oprávnění správce pro tenanta Azure AD. Po přihlášení vytvoří skript web a nativní aplikace představují cluster Service Fabric. Pokud se podíváte na klienta aplikace v [portál Azure classic][azure-classic-portal], měli byste vidět dvě nové položky:

   * *Název clusteru*\_clusteru
   * *Název clusteru*\_klienta

   Skript vypíše JSON vyžadují šablony Azure Resource Manageru při vytvoření clusteru v další části, takže je vhodné ponechat otevřené okno prostředí PowerShell.

```json
"azureActiveDirectory": {
  "tenantId":"<guid>",
  "clusterApplication":"<guid>",
  "clientApplication":"<guid>"
},
```

## <a name="create-a-service-fabric-cluster-resource-manager-template"></a>Vytvoření šablony správce prostředků clusteru Service Fabric
V této části výstupy z předchozích příkazů prostředí PowerShell se používají v šabloně Resource Manager clusteru Service Fabric.

Jsou k dispozici v šablonách Resource Manageru ukázkový [šablony Azure úvodní Galerie na Githubu][azure-quickstart-templates]. Tyto šablony slouží jako výchozí bod pro šablonu clusteru.

### <a name="create-the-resource-manager-template"></a>Vytvoření šablony Resource Manageru
Tato příručka používá [5 uzlu clusteru zabezpečené] [ service-fabric-secure-cluster-5-node-1-nodetype] příklad šablony a parametry šablony. Stáhněte si `azuredeploy.json` a `azuredeploy.parameters.json` do počítače a otevřete oba soubory ve svém oblíbeném textovém editoru.

### <a name="add-certificates"></a>Přidat certifikáty
Přidejte certifikáty do šablony správce prostředků clusteru pod položkou trezoru klíčů, který obsahuje klíče certifikátu. Doporučujeme umístit klíč trezoru hodnoty v souboru parametrů šablony Resource Manageru. Tím zajistí, že budou Resource Manager soubor šablony opakovaně použitelné volné hodnot konkrétní do nasazení.

#### <a name="add-all-certificates-to-the-virtual-machine-scale-set-osprofile"></a>Přidejte všechny certifikáty do osProfile sady škálování virtuálního počítače
Každý certifikát, který je nainstalován v clusteru musí být nakonfigurován v části osProfile prostředku sada škálování (Microsoft.Compute/virtualMachineScaleSets). Tato akce Nastaví zprostředkovatele prostředků k instalaci certifikátu na virtuálních počítačích. Tato instalace zahrnuje certifikát clusteru a všechny certifikáty zabezpečení aplikací, které budete používat pro vaše aplikace:

```json
{
  "apiVersion": "2016-03-30",
  "type": "Microsoft.Compute/virtualMachineScaleSets",
  ...
  "properties": {
    ...
    "osProfile": {
      ...
      "secrets": [
        {
          "sourceVault": {
            "id": "[parameters('sourceVaultValue')]"
          },
          "vaultCertificates": [
            {
              "certificateStore": "[parameters('clusterCertificateStorevalue')]",
              "certificateUrl": "[parameters('clusterCertificateUrlValue')]"
            },
            {
              "certificateStore": "[parameters('applicationCertificateStorevalue')",
              "certificateUrl": "[parameters('applicationCertificateUrlValue')]"
            },
            ...
          ]
        }
      ]
    }
  }
}
```

#### <a name="configure-the-service-fabric-cluster-certificate"></a>Konfigurace certifikátu clusteru Service Fabric
Certifikát pro ověření clusteru musí být konfigurované v obou Service Fabric prostředek clusteru (Microsoft.ServiceFabric/clusters) a nastaví rozšíření Service Fabric pro škálování virtuálních počítačů v prostředku sady škálování virtuálního počítače. Toto uspořádání umožňuje poskytovateli prostředků Service Fabric ho nakonfigurovat pro použití pro ověření clusteru a ověření serveru pro správu koncové body.

##### <a name="virtual-machine-scale-set-resource"></a>Škálovací sady virtuálních počítačů prostředků:
```json
{
  "apiVersion": "2016-03-30",
  "type": "Microsoft.Compute/virtualMachineScaleSets",
  ...
  "properties": {
    ...
    "virtualMachineProfile": {
      "extensionProfile": {
        "extensions": [
          {
            "name": "[concat('ServiceFabricNodeVmExt','_vmNodeType0Name')]",
            "properties": {
              ...
              "settings": {
                ...
                "certificate": {
                  "thumbprint": "[parameters('clusterCertificateThumbprint')]",
                  "x509StoreName": "[parameters('clusterCertificateStoreValue')]"
                },
                ...
              }
            }
          }
        ]
      }
    }
  }
}
```

##### <a name="service-fabric-resource"></a>Prostředek infrastruktury služby:
```json
{
  "apiVersion": "2016-03-01",
  "type": "Microsoft.ServiceFabric/clusters",
  "name": "[parameters('clusterName')]",
  "location": "[parameters('clusterLocation')]",
  "dependsOn": [
    "[concat('Microsoft.Storage/storageAccounts/', variables('supportLogStorageAccountName'))]"
  ],
  "properties": {
    "certificate": {
      "thumbprint": "[parameters('clusterCertificateThumbprint')]",
      "x509StoreName": "[parameters('clusterCertificateStoreValue')]"
    },
    ...
  }
}
```

### <a name="insert-azure-ad-configuration"></a>Vložit konfigurace služby Azure AD
Konfigurace Azure AD, kterou jste vytvořili dříve lze vložit přímo do vaší šablony Resource Manageru. Doporučujeme však nejprve extrahovat hodnoty do souboru parametrů pro zachovat opakovaně použitelného šablony Resource Manageru a volné hodnot, které jsou specifické pro nasazení.

```json
{
  "apiVersion": "2016-03-01",
  "type": "Microsoft.ServiceFabric/clusters",
  "name": "[parameters('clusterName')]",
  ...
  "properties": {
    "certificate": {
      "thumbprint": "[parameters('clusterCertificateThumbprint')]",
      "x509StoreName": "[parameters('clusterCertificateStorevalue')]"
    },
    ...
    "azureActiveDirectory": {
      "tenantId": "[parameters('aadTenantId')]",
      "clusterApplication": "[parameters('aadClusterApplicationId')]",
      "clientApplication": "[parameters('aadClientApplicationId')]"
    },
    ...
  }
}
```

### < "Konfigurace arm" ></a>parametry šablony konfigurace správce prostředků
<!--- Loc Comment: It seems that <a "configure-arm" > must be replaced with <a name="configure-arm"></a> since the link seems not to be redirecting correctly --->
Nakonec použijte hodnoty výstup z trezoru klíčů a Azure AD PowerShell příkazy k naplnění souboru parametrů:

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        ...
        "clusterCertificateStoreValue": {
            "value": "My"
        },
        "clusterCertificateThumbprint": {
            "value": "<thumbprint>"
        },
        "clusterCertificateUrlValue": {
            "value": "https://myvault.vault.azure.net:443/secrets/myclustercert/4d087088df974e869f1c0978cb100e47"
        },
        "applicationCertificateStorevalue": {
            "value": "My"
        },
        "applicationCertificateUrlValue": {
            "value": "https://myvault.vault.azure.net:443/secrets/myapplicationcert/2e035058ae274f869c4d0348ca100f08"
        },
        "sourceVaultvalue": {
            "value": "/subscriptions/<guid>/resourceGroups/mycluster-keyvault/providers/Microsoft.KeyVault/vaults/myvault"
        },
        "aadTenantId": {
            "value": "<guid>"
        },
        "aadClusterApplicationId": {
            "value": "<guid>"
        },
        "aadClientApplicationId": {
            "value": "<guid>"
        },
        ...
    }
}
```
V tomto okamžiku byste měli mít zavedené následující prvky:

* Skupina prostředků trezoru klíčů
  * Trezor klíčů
  * Ověřovací certifikát serverů clusteru
  * Certifikát pro šifrování dat
* Klientovi služby Azure Active Directory
  * Aplikace Azure AD pro správu webových a Service Fabric Exploreru
  * Aplikace pro správu nativního klienta Azure AD.
  * Uživatelé a jejich přiřazené role
* Šablony Resource Manageru clusteru Service Fabric
  * Certifikáty nakonfigurovaný pomocí trezoru klíčů
  * Azure Active Directory, které jsou nakonfigurované

Následující diagram znázorňuje, kde trezoru klíčů a konfigurace Azure AD zapadnou do vaší šablony Resource Manageru.

![Mapa závislostí Resource Manager][cluster-security-arm-dependency-map]

## <a name="create-the-cluster"></a>Vytvoření clusteru
Nyní jste připraveni k vytvoření clusteru pomocí [nasazení šablony Azure resource][resource-group-template-deploy].

#### <a name="test-it"></a>otestovat
Použijte následující příkaz prostředí PowerShell k testování šablony správce prostředků se souborem parametry:

```powershell
Test-AzureRmResourceGroupDeployment -ResourceGroupName "myresourcegroup" -TemplateFile .\azuredeploy.json -TemplateParameterFile .\azuredeploy.parameters.json
```

#### <a name="deploy-it"></a>nasazení
V případě úspěšného test šablony Resource Manageru, použijte následující příkaz prostředí PowerShell k nasazení vaší šablony správce prostředků se souborem parametry:

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName "myresourcegroup" -TemplateFile .\azuredeploy.json -TemplateParameterFile .\azuredeploy.parameters.json
```

<a name="assign-roles"></a>

## <a name="assign-users-to-roles"></a>Přiřadit uživatele k rolím
Po vytvoření aplikace, které chcete představují clusteru přiřadit uživatelům role nepodporuje Service Fabric: jen pro čtení a správce. Můžete přiřadit role pomocí [portál Azure classic][azure-classic-portal].

1. Na portálu Azure přejděte ke klientovi a potom vyberte **aplikace**.
2. Vyberte webové aplikaci, která má název, jako je `myTestCluster_Cluster`.
3. Klikněte **uživatelé** kartě.
4. Vybrat uživatele a přiřadit a klikněte **přiřadit** tlačítko v dolní části obrazovky.

    ![Přiřazení uživatelů pro tlačítko role][assign-users-to-roles-button]
5. Vyberte roli přiřadit uživatele.

    ![Dialogové okno "Přiřazení uživatelů"][assign-users-to-roles-dialog]

> [!NOTE]
> Další informace o rolích v Service Fabric najdete v tématu [řízení přístupu na základě rolí pro Service Fabric klienty](service-fabric-cluster-security-roles.md).
>
>

 <a name="secure-linux-clusters"></a>
 <!--- Loc Comment: It seems that letter S in cluster was missing, which caused the wrong redirection of the link --->

## <a name="create-secure-clusters-on-linux"></a>Vytvoření zabezpečeného clusterů v systému Linux
Pro usnadnění procesu uvádíme [pomocné rutiny skriptu](http://github.com/ChackDan/Service-Fabric/tree/master/Scripts/CertUpload4Linux). Než použijete tento skript pomocné rutiny, ujistěte se, že už máte rozhraní příkazového řádku Azure (CLI) nainstalovaná a je v cestě. Ujistěte se, že skript má oprávnění ke spuštění spuštěním `chmod +x cert_helper.py` po stáhnout. Prvním krokem je pro přihlášení k účtu Azure pomocí rozhraní příkazového řádku s `azure login` příkaz. Po přihlášení k účtu Azure, použijte skript Pomocník s certifikátem podepsaným certifikační Autority, jak ukazuje následující příkaz:

```sh
./cert_helper.py [-h] CERT_TYPE [-ifile INPUT_CERT_FILE] [-sub SUBSCRIPTION_ID] [-rgname RESOURCE_GROUP_NAME] [-kv KEY_VAULT_NAME] [-sname CERTIFICATE_NAME] [-l LOCATION] [-p PASSWORD]
```

Parametr - ifile může trvat soubor .pfx nebo soubor .pem jako vstup pro typ certifikátu (pfx nebo pem nebo ss, pokud je certifikát podepsaný svým držitelem).
Parametru -h vytiskne text nápovědy.


Tento příkaz vrátí následující tři řetězce jako výstup:

* SourceVaultID, což je ID pro nové KeyVault ResourceGroup se vám vytvoří
* CertificateUrl pro přístup k certifikátu
* Miniatura certifikátu, který se používá pro ověřování

Následující příklad ukazuje, jak používat příkaz:

```sh
./cert_helper.py pfx -sub "fffffff-ffff-ffff-ffff-ffffffffffff"  -rgname "mykvrg" -kv "mykevname" -ifile "/home/test/cert.pfx" -sname "mycert" -l "East US" -p "pfxtest"
```
Provádění předchozí příkaz nabízí tři řetězce následujícím způsobem:

```sh
SourceVault: /subscriptions/fffffff-ffff-ffff-ffff-ffffffffffff/resourceGroups/mykvrg/providers/Microsoft.KeyVault/vaults/mykvname
CertificateUrl: https://myvault.vault.azure.net/secrets/mycert/00000000000000000000000000000000
CertificateThumbprint: 0xfffffffffffffffffffffffffffffffffffffffff
```

Název subjektu certifikátu musí odpovídat domény, který používáte pro přístup ke clusteru Service Fabric. Toto porovnání je potřeba zadat protokolem SSL pro koncové body správy protokolu HTTPS a Service Fabric Explorer clusteru. Nelze získat od certifikační Autority pro certifikát SSL `.cloudapp.azure.com` domény. Je nutné získat vlastní název domény pro váš cluster. Pokud budete požadovat certifikát od certifikační Autority, název subjektu certifikátu musí odpovídat názvu vlastní domény, který používáte pro váš cluster.

Tyto názvy předmětu se položky je třeba vytvořit zabezpečení clusteru Service Fabric (bez Azure AD), jak je popsáno v [parametrů šablony Resource Manageru nakonfigurovat](#configure-arm). Můžete připojit k zabezpečení clusteru podle pokynů pro [ověřování klientský přístup do clusteru s podporou](service-fabric-connect-to-secure-cluster.md). Linux clusterů nepodporují ověřování Azure AD. Můžete přiřadit role správce a klienta, jak je popsáno v [přiřadit role pro uživatele](#assign-roles) části. Když zadáte role správce a klienta pro Linux cluster, je nutné zadat kryptografické otisky certifikátů pro ověřování. Protože je prováděna žádné ověření řetězu nebo odvolání nezadáte název předmětu.

Pokud chcete použít certifikát podepsaný svým držitelem pro testování, můžete jej vygenerovat stejný skriptu. Certifikát můžete pak nahrajte do trezoru klíčů tím, že poskytuje příznak `ss` místo certifikátu cestu a název certifikátu. Například najdete následující příkaz pro vytváření a odesílání certifikát podepsaný svým držitelem:

```sh
./cert_helper.py ss -rgname "mykvrg" -sub "fffffff-ffff-ffff-ffff-ffffffffffff" -kv "mykevname"   -sname "mycert" -l "East US" -p "selftest" -subj "mytest.eastus.cloudapp.net"
```
Tento příkaz vrátí stejné tři řetězce: SourceVault, CertificateUrl a CertificateThumbprint. Potom můžete řetězce k vytvoření clusteru s podporou zabezpečení Linux a umístění, kde je umístěn certifikát podepsaný svým držitelem. Je nutné certifikát podepsaný svým držitelem pro připojení ke clusteru. Můžete připojit k zabezpečení clusteru podle pokynů pro [ověřování klientský přístup do clusteru s podporou](service-fabric-connect-to-secure-cluster.md).

Název subjektu certifikátu musí odpovídat domény, který používáte pro přístup ke clusteru Service Fabric. Toto porovnání je potřeba zadat protokolem SSL pro koncové body správy protokolu HTTPS a Service Fabric Explorer clusteru. Nelze získat od certifikační Autority pro certifikát SSL `.cloudapp.azure.com` domény. Je nutné získat vlastní název domény pro váš cluster. Pokud budete požadovat certifikát od certifikační Autority, název subjektu certifikátu musí odpovídat názvu vlastní domény, který používáte pro váš cluster.

Parametry ze skriptu pomocná na portálu Azure můžete vyplnit, jak je popsáno v [na portálu Azure vytvořit cluster](service-fabric-cluster-creation-via-portal.md#create-cluster-in-the-azure-portal) části.

## <a name="next-steps"></a>Další kroky
V tuto chvíli máte zabezpečené cluster s poskytnete správu ověřování Azure Active Directory. Dále [připojit ke clusteru](service-fabric-connect-to-secure-cluster.md) a zjistěte, jak [spravovat tajné klíče aplikace](service-fabric-application-secret-management.md).

## <a name="troubleshoot-setting-up-azure-active-directory-for-client-authentication"></a>Řešení potíží s nastavení Azure Active Directory pro ověřování klientů
Pokud narazíte na problém při při instalaci Azure AD pro ověřování klientů, přečtěte si možná řešení v této části.

### <a name="service-fabric-explorer-prompts-you-to-select-a-certificate"></a>Service Fabric Explorer zobrazí výzva k výběru certifikátu
#### <a name="problem"></a>Problém
Po přihlášení úspěšně do služby Azure AD v Service Fabric Exploreru, prohlížeč vrací na domovskou stránku, ale zobrazí se výzva k výběru certifikátu.

![Vyberte certifikát SFX dialogové okno][sfx-select-certificate-dialog]

#### <a name="reason"></a>Důvod
Uživatel není přiřazena role v clusteru aplikace Azure AD. Ověření Azure AD se proto nezdaří na cluster Service Fabric. Service Fabric Explorer spadne zpět na ověřování pomocí certifikátu.

#### <a name="solution"></a>Řešení
Postupujte podle pokynů pro nastavení služby Azure AD a přiřadit role uživatele. Také doporučujeme zapnout "Přiřazení uživatelských požadovaná pro přístup k aplikaci" jako `SetupApplications.ps1` nepodporuje.

### <a name="connection-with-powershell-fails-with-an-error-the-specified-credentials-are-invalid"></a>Připojení pomocí prostředí PowerShell selže s chybou: "zadané přihlašovací údaje jsou neplatné."
#### <a name="problem"></a>Problém
Použijete-li se připojit ke clusteru pomocí "Azureactivedirectory selhala" režim zabezpečení, po přihlášení úspěšně do služby Azure AD PowerShell, připojení se nezdaří s chybou: "zadané přihlašovací údaje jsou neplatné."

#### <a name="solution"></a>Řešení
Toto řešení je stejný jako předchozí.

### <a name="service-fabric-explorer-returns-a-failure-when-you-sign-in-aadsts50011"></a>Service Fabric Explorer vrátí chybu při přihlášení: "AADSTS50011"
#### <a name="problem"></a>Problém
Při pokusu o přihlášení k Azure AD v Service Fabric Exploreru stránce vrátí chybu: "AADSTS50011: adresu &lt;url&gt; neodpovídá adresy odpovědět, nakonfigurované pro aplikaci: &lt;guid&gt;."

![Adresa pro odpověď SFX neodpovídá.][sfx-reply-address-not-match]

#### <a name="reason"></a>Důvod
Aplikace clusteru (web), která představuje Service Fabric Explorer pokus o ověření služby Azure AD a jako součást požadavku poskytuje návratové adresy URL přesměrování. Adresa URL není uvedena v aplikaci Azure AD, ale **adresa URL odpovědi** seznamu.

#### <a name="solution"></a>Řešení
Na **konfigurace** karta aplikace clusteru (web), přidejte adresu URL z Service Fabric Explorer na **adresa URL odpovědi** seznamu nebo nahradit jedné z položek v seznamu. Po dokončení, uložte změnu.

![Adresa url odpovědi pro webové aplikace][web-application-reply-url]

### <a name="connect-the-cluster-by-using-azure-ad-authentication-via-powershell"></a>Připojení ke clusteru pomocí ověřování Azure AD pomocí prostředí PowerShell
Pokud chcete připojit cluster Service Fabric, použijte v následujícím příkladu příkaz prostředí PowerShell:

```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint <endpoint> -KeepAliveIntervalInSec 10 -AzureActiveDirectory -ServerCertThumbprint <thumbprint>
```

Další informace o rutině Connect-ServiceFabricCluster najdete v tématu [Connect-ServiceFabricCluster](https://msdn.microsoft.com/library/mt125938.aspx).

### <a name="can-i-reuse-the-same-azure-ad-tenant-in-multiple-clusters"></a>Můžete opakovaně použít stejnou klienta Azure AD ve více clusterů?
Ano. Ale nezapomeňte přidat adresu URL z Service Fabric Explorer aplikace clusteru (web). Service Fabric Exploreru, jinak nebude fungovat.

### <a name="why-do-i-still-need-a-server-certificate-while-azure-ad-is-enabled"></a>Proč stále potřebuji certifikát serveru je zapnuto Azure AD?
FabricClient a FabricGateway provést vzájemné ověření. Během ověřování Azure AD integrace Azure AD poskytuje identitu klienta k serveru a certifikát serveru se používá k ověření identity serveru. Další informace o certifikátech Service Fabric najdete v tématu [certifikáty X.509 a Service Fabric][x509-certificates-and-service-fabric].

<!-- Links -->
[azure-powershell]:https://azure.microsoft.com/documentation/articles/powershell-install-configure/
[key-vault-get-started]:../key-vault/key-vault-get-started.md
[aad-graph-api-docs]:https://msdn.microsoft.com/library/azure/ad/graph/api/api-catalog
[azure-classic-portal]: https://manage.windowsazure.com
[service-fabric-rp-helpers]: https://github.com/ChackDan/Service-Fabric/tree/master/Scripts/ServiceFabricRPHelpers
[service-fabric-cluster-security]: service-fabric-cluster-security.md
[active-directory-howto-tenant]: ../active-directory/active-directory-howto-tenant.md
[service-fabric-visualizing-your-cluster]: service-fabric-visualizing-your-cluster.md
[service-fabric-manage-application-in-visual-studio]: service-fabric-manage-application-in-visual-studio.md
[sf-aad-ps-script-download]:http://servicefabricsdkstorage.blob.core.windows.net/publicrelease/MicrosoftAzureServiceFabric-AADHelpers.zip
[azure-quickstart-templates]: https://github.com/Azure/azure-quickstart-templates
[service-fabric-secure-cluster-5-node-1-nodetype]: https://github.com/Azure/azure-quickstart-templates/blob/master/service-fabric-secure-cluster-5-node-1-nodetype/
[resource-group-template-deploy]: https://azure.microsoft.com/documentation/articles/resource-group-template-deploy/
[x509-certificates-and-service-fabric]: service-fabric-cluster-security.md#x509-certificates-and-service-fabric

<!-- Images -->
[cluster-security-arm-dependency-map]: ./media/service-fabric-cluster-creation-via-arm/cluster-security-arm-dependency-map.png
[cluster-security-cert-installation]: ./media/service-fabric-cluster-creation-via-arm/cluster-security-cert-installation.png
[assign-users-to-roles-button]: ./media/service-fabric-cluster-creation-via-arm/assign-users-to-roles-button.png
[assign-users-to-roles-dialog]: ./media/service-fabric-cluster-creation-via-arm/assign-users-to-roles.png
[sfx-select-certificate-dialog]: ./media/service-fabric-cluster-creation-via-arm/sfx-select-certificate-dialog.png
[sfx-reply-address-not-match]: ./media/service-fabric-cluster-creation-via-arm/sfx-reply-address-not-match.png
[web-application-reply-url]: ./media/service-fabric-cluster-creation-via-arm/web-application-reply-url.png

