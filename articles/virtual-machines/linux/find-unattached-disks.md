---
title: "Vyhledejte a odstraňte odpojit spravovanými a nespravovanými disky Azure | Microsoft Docs"
description: "Postup nalezení a odstranění odpojit spravovaných a nespravovaných (objekty BLOB VHD/stránky) disky systému Azure, pomocí rozhraní příkazového řádku Azure"
services: virtual-machines-linux
documentationcenter: 
author: ramankumarlive
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 01/10/2017
ms.author: ramankum
ms.openlocfilehash: 9ada768cd4128b9dd6949b5a96c557496c6bb11c
ms.sourcegitcommit: 817c3db817348ad088711494e97fc84c9b32f19d
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/20/2018
---
# <a name="find-and-delete-unattached-azure-managed-and-unmanaged-disks"></a>Vyhledejte a odstraňte odpojit spravovanými a nespravovanými disky systému Azure
Při odstranění virtuálního počítače v Azure, disky připojené k němu nebudou odstraněny ve výchozím nastavení. Zabrání ztrátě dat v důsledku virtuálních počítačů odstraněna omylem, ale nadále zbytečně platit pro disky odpojit. Pomocí tohoto článku můžete najít a odstranit všechny nepřipojené disky a uložit náklady. 


## <a name="find-and-delete-unattached-managed-disks"></a>Vyhledejte a odstraňte odpojit spravované disky 

Tento skript je ukázkou, jak najít odpojit disky spravované pomocí vlastnosti spravované.  To projde všechny spravované disky v předplatném a ověří, zda *spravované* vlastnost má hodnotu null se najít disky odpojit spravované. *Spravované* vlastnosti se ukládají ID prostředku virtuálního počítače, ke kterému je připojen na Disk spravovaný. 

Důrazně doporučujeme vám pro první spuštění skriptu nastavením *deleteUnattachedDisks* proměnné na hodnotu 0, chcete-li zobrazit všechny disky odpojit. Po zkontrolování odpojit disky, spusťte skript nastavením *deleteUnattachedDisks* na 1, chcete-li odstranit všechny disky odpojit.

 ```azurecli

# Set deleteUnattachedDisks=1 if you want to delete unattached Managed Disks
# Set deleteUnattachedDisks=0 if you want to see the Id of the unattached Managed Disks
deleteUnattachedDisks=0

unattachedDiskIds=$(az disk list --query '[?managedBy==`null`].[id]' -o tsv)
for id in ${unattachedDiskIds[@]}
do
    if (( $deleteUnattachedDisks == 1 ))
    then

        echo "Deleting unattached Managed Disk with Id: "$id
        az disk delete --ids $id --yes
        echo "Deleted unattached Managed Disk with Id: "$id

    else
        echo $id
    fi
done

```
## <a name="find-and-delete-unattached-unmanaged-disks"></a>Vyhledejte a odstraňte odpojit nespravované disky 

Nespravované disky jsou soubory virtuálního pevného disku uložené jako [stránek](/rest/api/storageservices/understanding-block-blobs--append-blobs--and-page-blobs#about-page-blobs) v [účty Azure Storage](../../storage/common/storage-create-storage-account.md). Tento skript je ukázkou, jak najít pomocí vlastnosti LeaseStatus odpojit nespravované disky (objekty BLOB stránky). Dojde k opakování všechny nespravované disky ve všech úložiště účtů v předplatném a ověří, zda *LeaseStatus* vlastnost se odemkne najít odpojit nespravované disky. *LeaseStatus* vlastnost nastavena na uzamčeném, pokud nespravované disk připojen k virtuálnímu počítači. 

Důrazně doporučujeme vám pro první spuštění skriptu nastavením *deleteUnattachedVHDs* proměnné na hodnotu 0, chcete-li zobrazit všechny disky odpojit. Po zkontrolování odpojit disky, spusťte skript nastavením *deleteUnattachedVHDs* na 1, chcete-li odstranit všechny disky odpojit.


 ```azurecli
   
# Set deleteUnattachedVHDs=1 if you want to delete unattached VHDs
# Set deleteUnattachedVHDs=0 if you want to see the details of the unattached VHDs
deleteUnattachedVHDs=1

storageAccountIds=$(az storage account list --query [].[id] -o tsv)

for id in ${storageAccountIds[@]}
do
    connectionString=$(az storage account show-connection-string --ids $id --query connectionString -o tsv)
    containers=$(az storage container list --connection-string $connectionString --query [].[name] -o tsv)

    for container in ${containers[@]}
    do 
        
        blobs=$(az storage blob list -c $container --connection-string $connectionString --query "[?properties.blobType=='PageBlob' && ends_with(name,'.vhd')].[name]" -o tsv)
        
        for blob in ${blobs[@]}
        do
            leaseStatus=$(az storage blob show -n $blob -c $container --connection-string $connectionString --query "properties.lease.status" -o tsv)
            
            if [ "$leaseStatus" == "unlocked" ]
            then 

                if (( $deleteUnattachedVHDs == 1 ))
                then 

                    echo "Deleting VHD: "$blob" in container: "$container" in storage account: "$id

                    az storage blob delete --delete-snapshots include  -n $blob -c $container --connection-string $connectionString

                    echo "Deleted VHD: "$blob" in container: "$container" in storage account: "$id
                else
                    echo "StorageAccountId: "$id" container: "$container" VHD: "$blob
                fi

            fi
        done
    done
done 

```

## <a name="next-steps"></a>Další postup

[Odstranit účet úložiště](../../storage/common/storage-create-storage-account.md)



