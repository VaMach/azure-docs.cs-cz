---
ms.assetid: 
title: "Klíče účtu úložiště Azure Key Vault"
description: "Klíče účtu úložiště poskytují seemless integrace mezi Azure Key Vault a klíče založené na přístup k účtu úložiště Azure."
ms.topic: article
services: key-vault
ms.service: key-vault
author: lleonard-msft
ms.author: alleonar
manager: mbaldwin
ms.date: 10/12/2017
ms.openlocfilehash: 6ebac5fc90e259b19e0a4103a732754384232a44
ms.sourcegitcommit: 2a70752d0987585d480f374c3e2dba0cd5097880
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/19/2018
---
# <a name="azure-key-vault-storage-account-keys"></a>Klíče účtu úložiště Azure Key Vault

Před klíčů k účtu Azure Key Vault úložiště vývojáři museli spravovat vlastní klíče účtu úložiště Azure (ASA) a jejich otočení ručně nebo pomocí externího automatizace. Nyní, klíče účtu úložiště klíč trezoru jsou implementované jako [tajné klíče Key Vault](https://docs.microsoft.com/rest/api/keyvault/about-keys--secrets-and-certificates#BKMK_WorkingWithSecrets) pro ověřování pomocí účtu úložiště Azure. 

Funkci klíče účtu úložiště Azure (ASA) spravuje tajný otočení za vás. Odebere taky potřeba přímém kontaktu s klíčem ASA prostřednictvím nabídky sdíleného přístupu podpisy (SAS) jako metodu. 

Další obecné informace o účtech úložiště Azure najdete v tématu [účty Azure storage](https://docs.microsoft.com/azure/storage/storage-create-storage-account).

## <a name="supporting-interfaces"></a>Podpora rozhraní

Úplný seznam a odkazy na našem skriptovací a programovací rozhraní naleznete [Příručka vývojáře Key Vault](key-vault-developers-guide.md#coding-with-key-vault).


## <a name="what-key-vault-manages"></a>Co spravuje Key Vault

Při použití spravovaných klíče účtu úložiště Key Vault provede několik funkcí správy interní vaším jménem.

- Azure Key Vault spravuje klíče z Azure Storage účtu (ASA).
    - Azure Key Vault interně, můžete seznam klíčů (sync) s účtem úložiště Azure.  
    - Azure Key Vault regeneruje (otočí) klíče pravidelně. 
    - Hodnoty klíče se nikdy vrátí jako odpověď na volajícího. 
    - Azure Key Vault spravuje klíče účtů úložiště a klasické účty úložiště. 
- Azure Key Vault umožňuje vás, vlastníka trezoru nebo objekt, k vytvoření definice SAS (účet nebo SAS služby).
    - SAS, vytvořené pomocí definice SAS, je vrácena jako tajný klíč prostřednictvím cesta k identifikátoru URI REST. Další informace najdete v tématu [operace účet úložiště Azure Key Vault](https://docs.microsoft.com/rest/api/keyvault/storage-account-key-operations).

## <a name="naming-guidance"></a>Pokyny pro pojmenování

- Názvy účtů úložiště musí mít od 3 do 24 znaků a můžou obsahovat jenom číslice a malá písmena.  
- Název definice SAS musí být 1-102 znaků obsahující pouze 0 – 9,-z, A-Z.

## <a name="developer-experience"></a>Možnosti pro vývojáře

### <a name="before-azure-key-vault-storage-keys"></a>Před klíče úložiště Azure Key Vault 

Vývojáři použít k proveďte následující postupy se klíč účtu úložiště a získat tak přístup k úložišti Azure. 
 
```powershell
//create an Azure Storage Account using a connection string containing an account name and a storage key 

var storageAccount = CloudStorageAccount.Parse(CloudConfigurationManager.GetSetting("StorageConnectionString"));
var blobClient = storageAccount.CreateCloudBlobClient();
 ```
 
### <a name="after-azure-key-vault-storage-keys"></a>Po klíče úložiště Azure Key Vault 

```powershell
//Make sure to set storage permissions appropriately on your key vault
Set-AzureRmKeyVaultAccessPolicy -VaultName 'yourVault' -ObjectId yourObjectId -PermissionsToStorage all

//Get secret URI 

Set-AzureKeyVaultManagedStorageSasDefinition -Service Blob -ResourceType Container,Service -VaultName yourKV  

-AccountName msak01 -Name blobsas1 -Protocol HttpsOnly -ValidityPeriod ([System.Timespan]::FromDays(1)) -Permission Read,List

//Get a SAS token from Key Vault

var secret = await kv.GetSecretAsync("SecretUri");

// Create new storage credentials using the SAS token. 

var accountSasCredential = new StorageCredentials(secret.Value); 

// Use the storage credentials and the Blob storage endpoint to create a new Blob service client. 

var accountWithSas = new CloudStorageAccount(accountSasCredential, new Uri ("https://myaccount.blob.core.windows.net/"), null, null, null); 

var blobClientWithSas = accountWithSas.CreateCloudBlobClient(); 
 
// If your SAS token is about to expire, Get sasToken again from Key Vault and update it.

accountSasCredential.UpdateSASToken(sasToken);
```

 ### <a name="developer-guidance"></a>Průvodce pro vývojáře

- Povolte jenom spravovat klíče ASA Key Vault. Nepokoušejte se spravovat sami, bude narušovat procesy Key Vault. 
- Nepovolit ASA klíče, který bude spravován více než jeden objekt Key Vault. 
- Pokud budete muset ručně znovu vygenerovat klíče ASA, doporučujeme vám obnovit prostřednictvím Key Vault. 

## <a name="getting-started"></a>Začínáme

### <a name="setup-for-role-based-access-control-rbac-permissions"></a>Instalační program pro oprávnění k přístupu na základě rolí k řízení (RBAC)

Identita aplikace Azure Key Vault potřebuje oprávnění k *seznamu* a *znovu vygenerovat* klíče pro účet úložiště. Nastavte tato oprávnění pomocí následujících kroků:

- Získejte ObjectId identity Azure Key Vault: 

    `Get-AzureRmADServicePrincipal -ServicePrincipalName cfa8b339-82a2-471a-a3c9-0fc0be7a4093`

- Přiřazení role operátora klíče úložiště Azure Key Vault Identity: 

    `New-AzureRmRoleAssignment -ObjectId <objectId of AzureKeyVault from previous command> -RoleDefinitionName 'Storage Account Key Operator Service Role' -Scope '<azure resource id of storage account>'`

    >[!NOTE]
    > Pro typ účtu classic, nastavte parametr role na *"Classic úložiště účet klíč služby Role operátora."*

## <a name="working-example"></a>Příklad práce

Následující příklad ukazuje, že vytvoření trezoru klíč spravovaný účet úložiště Azure a přidružené definice sdíleného přístupového podpisu (SAS).

### <a name="assumptions"></a>Předpoklady

Následující příkazy jsou givens v tomto příkladu práci.

- Prostředku úložiště se nachází v: */subscriptions/subscriptionId/resourceGroups/yourresgroup1/providers/Microsoft.Storage/storageAccounts/yourtest1*

- Název trezoru klíčů je: *yourtest1*

### <a name="get-a-service-principal"></a>Získejte objekt služby

```powershell
$yourKeyVaultServicePrincipalId = (Get-AzureRmADServicePrincipal -ServicePrincipalName cfa8b339-82a2-471a-a3c9-0fc0be7a4093).Id
```

Výstup tohoto předchozí příkaz bude obsahovat vaše ServicePrincipal, který budete nazýváme *yourKeyVaultServicePrincipalId*. 

### <a name="set-permissions"></a>Nastavit oprávnění

Zajistěte, aby byla vaše oprávnění úložišť nastavená na *všechny*. Můžete získat youruserPrincipalId a nastavit oprávnění pro úložiště pomocí následujících příkazů.

```powershell
$youruserPrincipalId = (Get-AzureRmADUser -SearchString "your user principal name").Id
```
Nyní vyhledejte název a získat související ObjectId, které budete používat v nastavení oprávnění pro úložiště.

```powershell
Set-AzureRmKeyVaultAccessPolicy -VaultName 'yourtest1' -ObjectId $youruserPrincipalId -PermissionsToStorage all
```

### <a name="allow-access"></a>Povolit přístup

Budete muset poskytnout přístup k službě Key Vault k účtům úložiště, než bude možné vytvořit účet úložiště spravovaný a definice SAS.

```powershell
New-AzureRmRoleAssignment -ObjectId $yourKeyVaultServicePrincipalId -RoleDefinitionName 'Storage Account Key Operator Service Role' -Scope '/subscriptions/subscriptionId/resourceGroups/yourresgroup1/providers/Microsoft.Storage/storageAccounts/yourtest1'
```

### <a name="create-storage-account"></a>Vytvoření účtu úložiště

Nyní vytvořte účet úložiště spravované a dvě definice SAS. SAS účtu poskytuje přístup ke službě blob s jinými oprávněními.

```powershell
Add-AzureKeyVaultManagedStorageAccount -VaultName yourtest1 -Name msak01 -AccountResourceId /subscriptions/subscriptionId/resourceGroups/yourresgroup1/providers/Microsoft.Storage/storageAccounts/yourtest1 -ActiveKeyName key2 -DisableAutoRegenerateKey
```

### <a name="regeneration"></a>Opětovné generování

Nastavení období opětovné generování pomocí následujících příkazů.

```powershell
$regenPeriod = [System.Timespan]::FromDays(3)
Add-AzureKeyVaultManagedStorageAccount -VaultName yourtest1 -Name msak01 -AccountResourceId /subscriptions/subscriptionId/resourceGroups/yourresgroup1/providers/Microsoft.Storage/storageAccounts/yourtest1 -ActiveKeyName key2 -RegenerationPeriod $regenPeriod
```

### <a name="set-sas-definitions"></a>Definice sady SAS

Nastavte definice SAS v Key Vault pro váš účet úložiště spravovaný.

```powershell
Set-AzureKeyVaultManagedStorageSasDefinition -Service Blob -ResourceType Container,Service -VaultName yourtest1  -AccountName msak01 -Name blobsas1 -Protocol HttpsOnly -ValidityPeriod ([System.Timespan]::FromDays(1)) -Permission Read,List
Set-AzureKeyVaultManagedStorageSasDefinition -Service Blob -ResourceType Container,Service,Object -VaultName yourtest1  -AccountName msak01 -Name blobsas2 -Protocol HttpsOnly -ValidityPeriod ([System.Timespan]::FromDays(1)) -Permission Read,List,Write
```

### <a name="get-token"></a>Získání tokenu

Získat odpovídající tokeny SAS a provádět volání do úložiště.

```powershell
$sasToken1 = (Get-AzureKeyVaultSecret -VaultName yourtest1 -SecretName msak01-blobsas1).SecretValueText
$sasToken2 = (Get-AzureKeyVaultSecret -VaultName yourtest1 -SecretName msak01-blobsas2).SecretValueText
```

### <a name="create-storage"></a>Vytvoření úložiště

Všimněte si, že se pokoušíte získat přístup s *$sastoken1* selže, ale, že jsme mají mít přístup s *$sastoken2*. 

```powershell
$context1 = New-AzureStorageContext -SasToken $sasToken1 -StorageAccountName yourtest1
$context2 = New-AzureStorageContext -SasToken $sasToken2 -StorageAccountName yourtest1
Set-AzureStorageBlobContent -Container containertest1 -File "abc.txt"  -Context $context1
Set-AzureStorageBlobContent -Container cont1-file "file.txt"  -Context $context2
```

### <a name="example-summary"></a>Příklad souhrnu

Budete mít přístup k obsahu s tokenem SAS, který má oprávnění k zápisu objektu blob storage.

### <a name="relevant-powershell-cmdlets"></a>Příslušné rutiny prostředí Powershell

- [Get-AzureKeyVaultManagedStorageAccount ](https://docs.microsoft.com/powershell/module/azurerm.keyvault/get-azurekeyvaultmanagedstorageaccount?view=azurermps-4.3.1)
- [Add-AzureKeyVaultManagedStorageAccount](https://docs.microsoft.com/powershell/module/AzureRM.KeyVault/Add-AzureKeyVaultManagedStorageAccount?view=azurermps-4.3.1)
- [Get-AzureKeyVaultManagedStorageSasDefinition](https://docs.microsoft.com/powershell/module/AzureRM.KeyVault/Get-AzureKeyVaultManagedStorageSasDefinition?view=azurermps-4.3.1)
- [Update-AzureKeyVaultManagedStorageAccountKey](https://docs.microsoft.com/powershell/module/AzureRM.KeyVault/Update-AzureKeyVaultManagedStorageAccountKey?view=azurermps-4.3.1)
- [Remove-AzureKeyVaultManagedStorageAccount](https://docs.microsoft.com/powershell/module/azurerm.keyvault/remove-azurekeyvaultmanagedstorageaccount?view=azurermps-4.3.1)
- [Remove-AzureKeyVaultManagedStorageSasDefinition](https://docs.microsoft.com/powershell/module/AzureRM.KeyVault/Remove-AzureKeyVaultManagedStorageSasDefinition?view=azurermps-4.3.1)
- [Set-AzureKeyVaultManagedStorageSasDefinition](https://docs.microsoft.com/powershell/module/AzureRM.KeyVault/Set-AzureKeyVaultManagedStorageSasDefinition?view=azurermps-4.3.1)

## <a name="storage-account-onboarding"></a>Registrace účtu úložiště 

Příklad: jako vlastník objektu Key Vault, které přidáte objekt účtu úložiště Azure Key Vault pro zaváděním účet úložiště.

Během registrace, Key Vault musí ověřit, že identitu účtu registrace má oprávnění k *seznamu* a *znovu vygenerovat* klíče úložiště. Chcete-li ověřit, tato oprávnění, Key Vault získá OBO (na Behalf Of) token pomocí ověřovací služby, cílové skupiny nastavená na Azure Resource Manager a díky *seznamu* klíče volání služby Azure Storage. Pokud *seznamu* volání selže, Key Vault vytvoření objektu selže s kódem stavu HTTP *zakázáno*. S úložištěm entity váš trezor klíčů v mezipaměti klíče uvedené tímto způsobem. 

Key Vault musí ověřte, zda má identita *znovu vygenerovat* oprávnění předtím, než ji může převzít vlastnictví opětovné generování klíče. Chcete-li ověřit, že identita, prostřednictvím OBO token, stejně jako první strany identita Key Vault má tato oprávnění:

- Key Vault seznam oprávnění RBAC na prostředků účtu úložiště.
- Key Vault ověří odpovědi pomocí regulárního výrazu shody akce a jiné akce. 

Najít příklady podpůrné v [Key Vault - spravované ukázky klíče účtu úložiště](https://github.com/Azure/azure-sdk-for-net/blob/psSdkJson6/src/SDKs/KeyVault/dataPlane/Microsoft.Azure.KeyVault.Samples/samples/HelloKeyVault/Program.cs#L167).

Není-li identitu *znovu vygenerovat* oprávnění nebo pokud Key Vault první strany identity nemá *seznamu* nebo *znovu vygenerovat* oprávnění a potom registrace požadavek selže, vrátí kód odpovídající chyby a zprávy. 

OBO token bude fungovat pouze při použití vlastních, aplikace nativního klienta PowerShell nebo rozhraní příkazového řádku.

## <a name="other-applications"></a>Ostatní aplikace

- Tokeny SAS, vytvářeny pomocí klíčů k účtu úložiště Key Vault, zadejte i více řízený přístup k účtu úložiště Azure. Další informace najdete v tématu [pomocí sdílené přístupové podpisy](https://docs.microsoft.com/azure/storage/storage-dotnet-shared-access-signature-part-1).

## <a name="see-also"></a>Další informace najdete v tématech

- [Informace o klíčích, tajných kódech a certifikátech](https://docs.microsoft.com/rest/api/keyvault/)
- [Blog týmu trezoru klíčů](https://blogs.technet.microsoft.com/kv/)
