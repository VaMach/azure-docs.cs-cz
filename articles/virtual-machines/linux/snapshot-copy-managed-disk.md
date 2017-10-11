---
title: "Zkopírujte diskem spravované Azure pro Back up | Microsoft Docs"
description: "Naučte se vytvořit kopii Azure spravované disku pro back up nebo řešení potíží s disku."
documentationcenter: 
author: squillace
manager: timlt
editor: 
tags: azure-resource-manager
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.topic: article
ms.date: 2/6/2017
ms.author: rasquill
ms.openlocfilehash: c91367ef11c9d531bebac7c069d2df586607ec29
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-copy-of-a-vhd-stored-as-an-azure-managed-disk-by-using-managed-snapshots"></a>Vytvoření kopie virtuálního pevného disku uložené jako spravované disku Azure s využitím spravované snímky
Pořízení snímku spravované disku pro zálohu nebo vytvoření disku spravované ze snímku a jeho připojení k testovací virtuální počítač k řešení. Spravované snímek je úplné v daném okamžiku kopie disku spravovaných virtuálních počítačů. Ji vytvoří kopii svůj disk VHD jen pro čtení a ve výchozím nastavení, ukládá jako standardní spravované Disk. 

Informace o cenách najdete v tématu [Azure Storage – ceny](https://azure.microsoft.com/pricing/details/managed-disks/). <!--Add link to topic or blog post that explains managed disks. -->

K pořízení snímku na Disk spravovaný pomocí portálu Azure nebo Azure CLI 2.0.

## <a name="use-azure-cli-20-to-take-a-snapshot"></a>Použití Azure CLI 2.0 k pořízení snímku

> [!NOTE] 
> Následující příklad vyžaduje Azure CLI 2.0 nainstalovaná a přihlášení k účtu Azure.

Následující kroky ukazují, jak získat a pořízení snímku spravované disk operačního systému pomocí `az snapshot create` s `--source-disk` parametr. Následující příklad předpokládá, že je virtuální počítač s názvem `myVM` vytvořené pomocí spravovaného disku operačního systému ve `myResourceGroup` skupinu prostředků.

```azure-cli
# take the disk id with which to create a snapshot
osDiskId=$(az vm show -g myResourceGroup -n myVM --query "storageProfile.osDisk.managedDisk.id" -o tsv)
az snapshot create -g myResourceGroup --source "$osDiskId" --name osDisk-backup
```

Výstup by měl vypadat podobně jako:

```json
{
  "accountType": "Standard_LRS",
  "creationData": {
    "createOption": "Copy",
    "imageReference": null,
    "sourceResourceId": null,
    "sourceUri": "/subscriptions/<guid>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/disks/osdisk_6NexYgkFQU",
    "storageAccountId": null
  },
  "diskSizeGb": 30,
  "encryptionSettings": null,
  "id": "/subscriptions/<guid>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/snapshots/osDisk-backup",
  "location": "westus",
  "name": "osDisk-backup",
  "osType": "Linux",
  "ownerId": null,
  "provisioningState": "Succeeded",
  "resourceGroup": "myResourceGroup",
  "tags": null,
  "timeCreated": "2017-02-06T21:27:10.172980+00:00",
  "type": "Microsoft.Compute/snapshots"
}
```

## <a name="use-azure-portal-to-take-a-snapshot"></a>Pořízení snímku pomocí portálu Azure 

1. Přihlaste se k webu [Azure Portal](https://portal.azure.com).
2. Spuštění v levé horní, klikněte na tlačítko **nový** a vyhledejte **snímku**.
3. V okně snímku, klikněte na tlačítko **vytvořit**.
4. Zadejte **název** pro snímku.
5. Vyberte existující [skupinu prostředků](../../azure-resource-manager/resource-group-overview.md#resource-groups) nebo zadejte název nové skupiny prostředků. 
6. Vyberte umístění datového centra Azure.  
7. Pro **zdrojový disk**, vyberte spravované Disk snímek.
8. Vyberte **typ účtu** používat k uložení snímku. Doporučujeme, abyste **Standard_LRS** Pokud to uložená na vysokou provádění disku potřebujete.
9. Klikněte na možnost **Vytvořit**.

Pokud máte v plánu používat k vytvoření disku spravované a připojte ji virtuální počítač, který musí být vysoké provádění snímku, použijte parametr `--sku Premium_LRS` s `az snapshot create` příkaz. Tím se vytvoří snímek tak, aby je uložena jako spravovaný Disk úrovně Premium. Prémiové disky spravované provést lépe, protože jsou disků SSD (Solid-State Drive), ale náklady více než standardní disky (HDD).


