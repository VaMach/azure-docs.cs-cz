---
title: "Sady škálování virtuálního počítače Azure šifrovat disky | Microsoft Docs"
description: "Zjistěte, jak k šifrování připojených disků v sady škálování virtuálního počítače."
services: virtual-machine-scale-sets
documentationcenter: 
author: iainfoulds
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/26/2018
ms.author: iainfou
ms.openlocfilehash: dddcece9f7566961b256369330661e5dbd5d4665
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/21/2018
---
# <a name="encrypt-os-and-attached-data-disks-in-a-virtual-machine-scale-set"></a>Šifrování operačního systému a připojené datové disky v škálovací sadu virtuálních počítačů
Azure [sady škálování virtuálního počítače](/azure/virtual-machine-scale-sets/) podporuje Azure disk encryption (ADE).  Šifrování Azure disku se dá nastavit pro Windows a Linux sadách škálování virtuálních počítačů k ochraně a zabezpečit měřítka nastaví data v klidovém stavu pomocí technologie oborový standard šifrování. Další informace najdete v tématu virtuální počítače Azure Disk Encryption pro systém Windows a Linux.

> [!NOTE]
>  Azure disk encryption pro sady škálování virtuálního počítače je aktuálně ve verzi public preview, k dispozici ve všech oblastech Azure veřejné.

Šifrování Azure disku je podporován:
- pro škálování sady vytvořené s spravovaných disků a nejsou podporovány pro sady škálování nativní (nebo nespravovaný) disku.
- pro svazky operačního systému a dat v systému Windows sady škálování. Zakázat šifrování je podporované pro svazky operačního systému a dat pro sady škálování systému Windows.
- pro datové svazky v sadách škálování Linux. Šifrování disku operačního systému není podporován v aktuální verzi preview pro Linux sady škálování.

Operace obnovení z Image a upgrade sady virtuálních počítačů škálování nejsou podporovány v aktuální verzi preview. Azure disk šifrování pro náhled sady škálování virtuálního počítače se doporučuje jenom v testovacích prostředích. Ve verzi preview nepovolujte šifrování disku v produkčním prostředí, kde je třeba upgradovat image operačního systému v sadě škálování šifrované.

## <a name="prerequisites"></a>Požadavky
Instalace nejnovější verze [prostředí Azure Powershell](https://github.com/Azure/azure-powershell/releases), který obsahuje příkazy šifrování.

Azure disk šifrování pro náhled sady škálování virtuálního počítače vyžaduje, abyste samoobslužné registrace předplatného použijte následující příkazy Powershellu: 

```powershell
Login-AzureRmAccount
Register-AzureRmProviderFeature -ProviderNamespace Microsoft.Compute -FeatureName "UnifiedDiskEncryption"
```

Počkejte přibližně 10 minut, dokud nebude stav "Registrovaná" vrátí následující příkaz: 

```powershell
Get-AzureRmProviderFeature -ProviderNamespace "Microsoft.Compute" -FeatureName "UnifiedDiskEncryption"
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Compute
```

## <a name="create-an-azure-key-vault-enabled-for-disk-encryption"></a>Vytvoření Azure trezoru klíčů pro šifrování disku povoleno
Vytvoření nového trezoru klíčů ve stejném předplatném, oblasti jako měřítko a nastavte zásady přístupu, EnabledForDiskEncryption' a.

```powershell
$rgname="windatadiskencryptiontest"
$VaultName="encryptionvault321"

New-AzureRmKeyVault -VaultName $VaultName -ResourceGroupName $rgName -Location southcentralus -EnabledForDiskEncryption
``` 

Nebo povolit existující trezor klíčů ve stejném předplatném, oblasti jako měřítka nastavit šifrování disku.

```powershell
$VaultName="encryptionvault321"
Set-AzureRmKeyVaultAccessPolicy -VaultName $VaultName -EnabledForDiskEncryption
```

## <a name="enable-encryption"></a>Povolit šifrování
Následující příkazy šifrování disku dat ve spuštěné škálování nastavit pomocí trezoru klíčů ve stejné skupině prostředků. Můžete také použít šablony k šifrování disků neběží [škálovací sadu Windows](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-vmss-windows-jumpbox) nebo [Linux škálovací sadu](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-vmss-linux-jumpbox).

```powershell
$rgname="windatadiskencryptiontest"
$VmssName="nt1vm"
$DiskEncryptionKeyVaultUrl="https://encryptionvault321.vault.azure.net"
$KeyVaultResourceId="/subscriptions/0754ecc2-d80d-426a-902c-b83f4cfbdc95/resourceGroups/windatadiskencryptiontest/providers/Microsoft.KeyVault/vaults/encryptionvault321"

Set-AzureRmVmssDiskEncryptionExtension -ResourceGroupName $rgName -VMScaleSetName $VmssName `
    -DiskEncryptionKeyVaultUrl $DiskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $KeyVaultResourceId –VolumeType Data
```

## <a name="check-encryption-progress"></a>Kontrola průběhu šifrování
Použijte následující příkazy zobrazíte stav šifrování škálovací sady.

```powershell
$rgname="windatadiskencryptiontest"
$VmssName="nt1vm"
Get-AzureRmVmssDiskEncryption -ResourceGroupName $rgName -VMScaleSetName $VmssName

Get-AzureRmVmssVMDiskEncryption -ResourceGroupName $rgName -VMScaleSetName $VmssName -InstanceId "4"
```

## <a name="disable-encryption"></a>Zakázat šifrování
Zakážete šifrování na spuštěné škálování virtuálních počítačů, nastavit pomocí následujících příkazů. Můžete také použít šablony můžete zakázat šifrování neběží [škálovací sadu Windows](https://github.com/Azure/azure-quickstart-templates/tree/master/201-decrypt-vmss-windows) nebo [Linux škálovací sadu](https://github.com/Azure/azure-quickstart-templates/tree/master/201-decrypt-vmss-linux).

```powershell
$rgname="windatadiskencryptiontest"
$VmssName="nt1vm"
Disable-AzureRmVmssDiskEncryption -ResourceGroupName $rgName -VMScaleSetName $VmssName
```
