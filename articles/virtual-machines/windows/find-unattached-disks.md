---
title: "Vyhledejte a odstraňte odpojit spravovanými a nespravovanými disky Azure | Microsoft Docs"
description: "Postup nalezení a odstranění odpojit spravovaných a nespravovaných (objekty BLOB VHD/stránky) disky systému Azure, pomocí prostředí Azure PowerShell."
services: virtual-machines-windows
documentationcenter: 
author: ramankumarlive
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 01/10/2017
ms.author: ramankum
ms.openlocfilehash: a846d3578d40b19762f185381c92bdf8e225b185
ms.sourcegitcommit: 817c3db817348ad088711494e97fc84c9b32f19d
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/20/2018
---
# <a name="find-and-delete-unattached-azure-managed-and-unmanaged-disks"></a>Vyhledejte a odstraňte odpojit spravovanými a nespravovanými disky systému Azure
Při odstranění virtuálního počítače v Azure, disky připojené k němu nebudou odstraněny ve výchozím nastavení. Zabrání ztrátě dat v důsledku virtuálních počítačů odstraněna omylem, ale nadále zbytečně platit pro disky odpojit. Pomocí tohoto článku můžete najít a odstranit všechny nepřipojené disky a uložit náklady. 


## <a name="find-and-delete-unattached-managed-disks"></a>Vyhledejte a odstraňte odpojit spravované disky 

Tento skript je ukázkou, jak najít odpojit [spravované disky](managed-disks-overview.md) pomocí *spravované* vlastnost. Dojde k opakování všechny spravované disky v předplatného a kontroly *spravované* vlastnost má hodnotu null se najít disky odpojit spravované. *Spravované* vlastnosti se ukládají ID prostředku virtuálního počítače, ke kterému je připojen na Disk spravovaný.

Důrazně doporučujeme vám pro první spuštění skriptu nastavením *deleteUnattachedDisks* proměnné na hodnotu 0, chcete-li zobrazit všechny disky odpojit. Po zkontrolování odpojit disky, spusťte skript nastavením *deleteUnattachedDisks* na 1, chcete-li odstranit všechny disky odpojit.

```azurepowershell-interactive

# Set deleteUnattachedDisks=1 if you want to delete unattached Managed Disks
# Set deleteUnattachedDisks=0 if you want to see the Id of the unattached Managed Disks
$deleteUnattachedDisks=0

$managedDisks = Get-AzureRmDisk

foreach ($md in $managedDisks) {
    
    # ManagedBy property stores the Id of the VM to which Managed Disk is attached to
    # If ManagedBy property is $null then it means that the Managed Disk is not attached to a VM
    if($md.ManagedBy -eq $null){

        if($deleteUnattachedDisks -eq 1){
            
            Write-Host "Deleting unattached Managed Disk with Id: $($md.Id)"

            $md | Remove-AzureRmDisk -Force

            Write-Host "Deleted unattached Managed Disk with Id: $($md.Id) "

        }else{

            $md.Id

        }
           
    }
     
 } 
```
## <a name="find-and-delete-unattached-unmanaged-disks"></a>Vyhledejte a odstraňte odpojit nespravované disky 

Nespravované disky jsou soubory virtuálního pevného disku uložené jako [objekty BLOB stránky] (/ rest/api/storageservices/understanding-block-blobs--append-blobs--and-page-blobs#about-page-blobs) v [účty Azure Storage](../../storage/common/storage-create-storage-account.md). Tento skript je ukázkou, jak najít odpojit nespravované disky (objekty BLOB stránky) pomocí *LeaseStatus* vlastnost. Dojde k opakování všechny nespravované disky ve všech úložiště účtů v předplatném a ověří, zda *LeaseStatus* vlastnost se odemkne najít odpojit nespravované disky. *LeaseStatus* vlastnost nastavena na uzamčeném, pokud nespravované disk připojen k virtuálnímu počítači.

Důrazně doporučujeme vám pro první spuštění skriptu nastavením *deleteUnattachedVHDs* proměnné na hodnotu 0, chcete-li zobrazit všechny disky odpojit. Po zkontrolování odpojit disky, spusťte skript nastavením *deleteUnattachedVHDs* na 1, chcete-li odstranit všechny disky odpojit.


```azurepowershell-interactive
   
# Set deleteUnattachedVHDs=1 if you want to delete unattached VHDs
# Set deleteUnattachedVHDs=0 if you want to see the Uri of the unattached VHDs
$deleteUnattachedVHDs=1

$storageAccounts = Get-AzureRmStorageAccount

foreach($storageAccount in $storageAccounts){

    $storageKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $storageAccount.ResourceGroupName -Name $storageAccount.StorageAccountName)[0].Value

    $context = New-AzureStorageContext -StorageAccountName $storageAccount.StorageAccountName -StorageAccountKey $storageKey

    $containers = Get-AzureStorageContainer -Context $context

    foreach($container in $containers){

        $blobs = Get-AzureStorageBlob -Container $container.Name -Context $context

        #Fetch all the Page blobs with extension .vhd as only Page blobs can be attached as disk to Azure VMs
        $blobs | Where-Object {$_.BlobType -eq 'PageBlob' -and $_.Name.EndsWith('.vhd')} | ForEach-Object { 
        
            #If a Page blob is not attached as disk then LeaseStatus will be unlocked
            if($_.ICloudBlob.Properties.LeaseStatus -eq 'Unlocked'){
              
                  if($deleteUnattachedVHDs -eq 1){

                        Write-Host "Deleting unattached VHD with Uri: $($_.ICloudBlob.Uri.AbsoluteUri)"

                        $_ | Remove-AzureStorageBlob -Force

                        Write-Host "Deleted unattached VHD with Uri: $($_.ICloudBlob.Uri.AbsoluteUri)"
                  }
                  else{

                        $_.ICloudBlob.Uri.AbsoluteUri

                  }

            }
        
        }

    }

}

```

## <a name="next-steps"></a>Další postup

[Odstranit účet úložiště](../../storage/common/storage-create-storage-account.md)




