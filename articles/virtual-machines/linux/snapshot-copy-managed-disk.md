---
title: "Vytvořte snímek virtuálního pevného disku v Azure | Microsoft Docs"
description: "Postup vytvoření kopie virtuálního pevného disku v Azure jako zpět nahoru nebo řešení potíží."
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-resource-manager
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.topic: article
ms.date: 10/09/2017
ms.author: cynthn
ms.openlocfilehash: 152c5a1103d32af27f689086cfcc9cc1a7acc5d3
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/21/2018
---
# <a name="create-a-snapshot"></a>Vytvoření snímku 

Pořízení snímku operačním systémem nebo datovými disk, který vystavuje virtuálního pevného disku pro zálohování a řešení souvisejících problémů virtuálních počítačů. Snímek je úplná, jen pro čtení kopie virtuálního pevného disku. 

## <a name="use-azure-cli-20-to-take-a-snapshot"></a>Použití Azure CLI 2.0 k pořízení snímku

Následující příklad vyžaduje Azure CLI 2.0 nainstalovaná a přihlášení k účtu Azure. Verzi zjistíte spuštěním příkazu `az --version`. Pokud potřebujete instalaci nebo upgrade, přečtěte si téma [Instalace Azure CLI 2.0]( /cli/azure/install-azure-cli). 

Následující kroky ukazují, jak provést snímek pomocí `az snapshot create` s `--source-disk` parametr. Následující příklad předpokládá, že je virtuální počítač s názvem `myVM` vytvořené pomocí spravovaného disku operačního systému ve `myResourceGroup` skupinu prostředků.

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
2. Spuštění v levé horní, klikněte na tlačítko **vytvořit prostředek** a vyhledejte **snímku**.
3. V okně snímku, klikněte na tlačítko **vytvořit**.
4. Zadejte **název** pro snímku.
5. Vyberte existující [skupinu prostředků](../../azure-resource-manager/resource-group-overview.md#resource-groups) nebo zadejte název nové skupiny prostředků. 
6. Vyberte umístění datového centra Azure.  
7. Pro **zdrojový disk**, vyberte spravované Disk snímek.
8. Vyberte **typ účtu** používat k uložení snímku. Doporučujeme, abyste **Standard_LRS** Pokud to uložená na vysokou provádění disku potřebujete.
9. Klikněte na možnost **Vytvořit**.

Pokud máte v plánu používat k vytvoření disku spravované a připojte ji virtuální počítač, který musí být vysoké provádění snímku, použijte parametr `--sku Premium_LRS` s `az snapshot create` příkaz. Tím se vytvoří snímek tak, aby je uložena jako spravovaný Disk úrovně Premium. Prémiové disky spravované provést lépe, protože jsou disků SSD (Solid-State Drive), ale náklady více než standardní disky (HDD).


## <a name="next-steps"></a>Další postup

 Vytvoření virtuálního počítače ze snímku vytvořením se spravovaným diskem ze snímku a potom nový spravované disk jako disk operačního systému se připojuje. Další informace najdete v tématu [vytvoření virtuálního počítače ze snímku](./../scripts/virtual-machines-linux-cli-sample-create-vm-from-snapshot.md?toc=%2fcli%2fmodule%2ftoc.json) skriptu.

