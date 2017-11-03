---
title: "Obnovit disk virtuálního počítače s Azure Backup | Microsoft Docs"
description: "Zjistěte, jak k obnovení na disk a vytvořte obnovit virtuální počítač v Azure pomocí zálohování a obnovení služby."
services: backup, virtual-machines
documentationcenter: virtual-machines
author: markgalioto
manager: carmonm
editor: 
tags: azure-resource-manager, virtual-machine-backup
ms.assetid: 
ms.service: backup, virtual-machines
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 09/28/2017
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: 9bc6da13786eb9eb6186ceadf0432b3a3ec2c941
ms.sourcegitcommit: e462e5cca2424ce36423f9eff3a0cf250ac146ad
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/01/2017
---
# <a name="restore-a-disk-and-create-a-recovered-vm-in-azure"></a>Obnovení na disk a vytvořte obnovený virtuální počítač v Azure
Zálohování Azure vytvoří body obnovení, které jsou uložené v geograficky redundantní obnovení trezorů. Při obnovení z bodu obnovení můžete obnovit celý virtuální počítač nebo jednotlivé soubory. Tento článek vysvětluje, jak obnovit dokončení virtuálního počítače. V tomto kurzu se naučíte:

> [!div class="checklist"]
> * Seznam a bodů obnovení vyberte
> * Obnovení disku z bodu obnovení
> * Vytvoření virtuálního počítače z obnovené disku

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Pokud si zvolíte instalaci a použití rozhraní příkazového řádku místně, tento kurz vyžaduje, že používáte Azure CLI verze 2.0.18 nebo novější. Verzi zjistíte spuštěním příkazu `az --version`. Pokud potřebujete instalaci nebo upgrade, přečtěte si téma [Instalace Azure CLI 2.0]( /cli/azure/install-azure-cli). 


## <a name="prerequisites"></a>Požadavky
Tento kurz vyžaduje Linux virtuální počítač, který je chráněný pomocí zálohování Azure. Simulaci náhodnému odstranění virtuálního počítače a procesu obnovení, vytvoření virtuálního počítače z disku v bodu obnovení. Pokud potřebujete Linux virtuální počítač, který je chráněný pomocí služby Azure Backup, přečtěte si [zálohování virtuálního počítače v Azure pomocí rozhraní příkazového řádku](quick-backup-vm-cli.md).


## <a name="backup-overview"></a>Přehled služby Backup
Pokud Azure zahájí zálohování, zálohování rozšíření ve virtuálním počítači pořídí snímek bodu v čase. Rozšíření zálohování je nainstalován ve virtuálním počítači, pokud se požaduje první zálohy. Zálohování Azure můžete také pořízení snímku podkladové úložiště, pokud virtuální počítač není spuštěn, když probíhá zálohování.

Ve výchozím nastavení trvá Azure Backup konzistentní zálohu systému souborů. Jakmile Azure Backup používá snímku, data se přenáší do trezoru služeb zotavení. Chcete-li maximalizovat efektivitu, Azure Backup identifikuje a přenáší pouze bloky dat, které se změnily od předchozí zálohy.

Po dokončení přenosu dat se odebere snímku a vytvoří bod obnovení.


## <a name="list-available-recovery-points"></a>Seznam dostupných bodů obnovení
Chcete-li obnovit disk, vyberte bod obnovení jako zdroj pro obnovení dat. Jako výchozí zásady vytvoří bod obnovení každý den a uchovává je 30 dní, můžete ponechat sadu body obnovení, která vám umožní vybrat konkrétní bod v čase pro obnovení. 

Chcete-li zobrazit seznam dostupných bodů obnovení, použijte [az zálohování recoverypoint seznamu](https://docs.microsoft.com/cli/azure/backup/recoverypoint?view=azure-cli-latest#az_backup_recoverypoint_list). Vytvoření bodu obnovení **název** slouží k obnovení disky. V tomto kurzu chceme nejnovější bod obnovení, která je k dispozici. `--query [0].name` Parametr vybere název nejnovější bod obnovení následujícím způsobem:

```azurecli-interactive
az backup recoverypoint list \
    --resource-group myResourceGroup \
    --vault-name myRecoveryServicesVault \
    --container-name myVM \
    --item-name myVM \
    --query [0].name \
    --output tsv
```


## <a name="restore-a-vm-disk"></a>Obnovení disku virtuálního počítače
Obnovení na disku z bodu obnovení, je nejprve vytvořit účet úložiště Azure. Tento účet úložiště se používá k uložení obnovené disku. V dalších krocích obnovené disk se používá k vytvoření virtuálního počítače.

1. Chcete-li vytvořit účet úložiště, použijte [vytvořit účet úložiště az](https://docs.microsoft.com/cli/azure/storage/account?view=azure-cli-latest#az_storage_account_create). Název účtu úložiště musí být psaný malými písmeny a musí být globálně jedinečný. Nahraďte *můj_účet_úložiště* s svůj vlastní jedinečný název:

    ```azurecli-interactive
    az storage account create \
        --resource-group myResourceGroup \
        --name mystorageaccount \
        --sku Standard_LRS
    ```

2. Obnovení na disku z bodu obnovení se [az zálohy obnovit obnovení disky](https://docs.microsoft.com/cli/azure/backup/restore?view=azure-cli-latest#az_backup_restore_restore_disks). Nahraďte *můj_účet_úložiště* s názvem účtu úložiště, který jste vytvořili v předchozím příkazu. Nahraďte *myRecoveryPointName* s názvem bodu obnovení jste získali ve výstupu z předchozí [az zálohování recoverypoint seznamu](https://docs.microsoft.com/cli/azure/backup/recoverypoint?view=azure-cli-latest#az_backup_recoverypoint_list) příkaz:

    ```azurecli-interactive
    az backup restore restore-disks \
        --resource-group myResourceGroup \
        --vault-name myRecoveryServicesVault \
        --container-name myVM \
        --item-name myVM \
        --storage-account mystorageaccount \
        --rp-name myRecoveryPointName
    ```


## <a name="monitor-the-restore-job"></a>Monitorování úlohy obnovení
Chcete-li sledovat stav úlohy obnovení, použijte [az úlohu zálohování seznamu](https://docs.microsoft.com/cli/azure/backup/job?view=azure-cli-latest#az_backup_job_list):

```azurecli-interactive 
az backup job list \
    --resource-group myResourceGroup \
    --vault-name myRecoveryServicesVault \
    --output table
```

Výstup bude vypadat v následujícím příkladu, který ukazuje úloha obnovení je *InProgress*:

```
Name      Operation        Status      Item Name    Start Time UTC       Duration
--------  ---------------  ----------  -----------  -------------------  --------------
7f2ad916  Restore          InProgress  myvm         2017-09-19T19:39:52  0:00:34.520850
a0a8e5e6  Backup           Completed   myvm         2017-09-19T03:09:21  0:15:26.155212
fe5d0414  ConfigureBackup  Completed   myvm         2017-09-19T03:03:57  0:00:31.191807
```

Když *stav* sestav úlohy obnovení *dokončeno*, obnovení na disku k účtu úložiště.


## <a name="convert-the-restored-disk-to-a-managed-disk"></a>Převést obnovené disk na Disk spravovaný
Úloha obnovení vytvoří nespravovaného disku. Chcete-li vytvořit virtuální počítač z disku, nejprve převeďte spravovaného disku.

1. Získat informace o připojení pro váš účet úložiště s [az úložiště účet zobrazit. připojovací řetězec](https://docs.microsoft.com/cli/azure/storage/account?view=azure-cli-latest#az_storage_account_show_connection_string). Nahraďte *můj_účet_úložiště* s názvem úložiště účet následujícím způsobem:
    
    ```azurecli-interactive
    export AZURE_STORAGE_CONNECTION_STRING=$( az storage account show-connection-string \
        --resource-group myResourceGroup \
        --output tsv \
        --name mystorageaccount )
    ```

2. Nespravované disk, je zabezpečena v účtu úložiště. Následující příkazy získat informace o vaší nespravované disku a vytvoření proměnné s názvem *uri* používané v dalším kroku při vytvoření disku spravované.

    ```azurecli-interactive
    container=$(az storage container list --query [0].name -o tsv)
    blob=$(az storage blob list --container-name $container --query [0].name -o tsv)
    uri=$(az storage blob url --container-name $container --name $blob -o tsv)
    ```

3. Teď vytvoříte Disk spravované z obnovené disku s [vytvoření disku az](https://docs.microsoft.com/cli/azure/disk?view=azure-cli-latest#az_disk_create). *Uri* proměnné z předchozího kroku slouží jako zdroj pro spravované Disk.

    ```azurecli-interactive
    az disk create \
        --resource-group myResourceGroup \
        --name myRestoredDisk \
        --source $uri
    ```

4. Nyní máte spravované Disk z obnovené disku, vyčistit nespravované disku a úložiště účet s [odstranit účet úložiště az](/cli/azure/storage/account?view=azure-cli-latest#az_storage_account_delete). Nahraďte *můj_účet_úložiště* s názvem úložiště účet následujícím způsobem:

    ```azurecli-interactive
    az storage account delete \
        --resource-group myResourceGroup \
        --name mystorageaccount
    ```


## <a name="create-a-vm-from-the-restored-disk"></a>Vytvoření virtuálního počítače z obnovené disku
Posledním krokem je vytvoření virtuálního počítače z disku spravované.

1. Vytvoření virtuálního počítače z disku spravované s [vytvořit virtuální počítač az](/cli/azure/vm?view=azure-cli-latest#az_vm_create) následujícím způsobem:

    ```azurecli-interactive
    az vm create \
        --resource-group myResourceGroup \
        --name myRestoredVM \
        --attach-os-disk myRestoredDisk \
        --os-type linux
    ```

2. Pokud chcete potvrdit, že virtuální počítač vytvořen z obnovené disku, seznam virtuálních počítačů ve vaší skupině prostředků s [seznamu virtuálních počítačů az](/cli/azure/vm?view=azure-cli-latest#az_vm_list) následujícím způsobem:

    ```azurecli-interactive
    az vm list --resource-group myResourceGroup --output table
    ```


## <a name="next-steps"></a>Další kroky
V tomto kurzu obnovit z bodu obnovení na disk a potom vytvořili virtuální počítač z disku. Naučili jste se tyto postupy:

> [!div class="checklist"]
> * Seznam a bodů obnovení vyberte
> * Obnovení disku z bodu obnovení
> * Vytvoření virtuálního počítače z obnovené disku

Přechodu na v dalším kurzu se dozvíte o obnovení jednotlivých souborů z bodu obnovení.

> [!div class="nextstepaction"]
> [Obnovit soubory do virtuálního počítače v Azure](tutorial-restore-files.md)

