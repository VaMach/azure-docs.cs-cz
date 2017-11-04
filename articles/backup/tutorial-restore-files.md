---
title: "Obnovit soubory do virtuálního počítače s Azure Backup | Microsoft Docs"
description: "Zjistěte, jak provádět obnovení na úrovni souborů na virtuální počítač Azure s zálohování a obnovení služby."
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
ms.date: 09/29/2017
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: e1bbae56b70c50fcf691db47efd9dc587686e7da
ms.sourcegitcommit: e462e5cca2424ce36423f9eff3a0cf250ac146ad
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/01/2017
---
# <a name="restore-files-to-a-virtual-machine-in-azure"></a>Obnovit soubory do virtuálního počítače v Azure
Zálohování Azure vytvoří body obnovení, které jsou uložené v geograficky redundantní obnovení trezorů. Při obnovení z bodu obnovení můžete obnovit celý virtuální počítač nebo jednotlivé soubory. Tento článek podrobně popisuje postup obnovení jednotlivých souborů. V tomto kurzu se naučíte:

> [!div class="checklist"]
> * Seznam a bodů obnovení vyberte
> * Připojení k virtuálnímu počítači bod obnovení
> * Obnovit soubory z bodu obnovení

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Pokud si zvolíte instalaci a použití rozhraní příkazového řádku místně, tento kurz vyžaduje, že používáte Azure CLI verze 2.0.18 nebo novější. Verzi zjistíte spuštěním příkazu `az --version`. Pokud potřebujete instalaci nebo upgrade, přečtěte si téma [Instalace Azure CLI 2.0](/cli/azure/install-azure-cli). 


## <a name="prerequisites"></a>Požadavky
Tento kurz vyžaduje Linux virtuální počítač, který je chráněný pomocí zálohování Azure. Simulaci náhodnému jejich odstranění a procesu obnovení, odstranit stránky z webového serveru. Pokud potřebujete Linux virtuální počítač, který běží webovém serveru a je chráněný pomocí služby Azure Backup, přečtěte si [zálohování virtuálního počítače v Azure pomocí rozhraní příkazového řádku](quick-backup-vm-cli.md).


## <a name="backup-overview"></a>Přehled služby Backup
Pokud Azure zahájí zálohování, zálohování rozšíření ve virtuálním počítači pořídí snímek bodu v čase. Rozšíření zálohování je nainstalován ve virtuálním počítači, pokud se požaduje první zálohy. Zálohování Azure můžete také pořízení snímku podkladové úložiště, pokud virtuální počítač není spuštěn, když probíhá zálohování.

Ve výchozím nastavení trvá Azure Backup konzistentní zálohu systému souborů. Jakmile Azure Backup používá snímku, data se přenáší do trezoru služeb zotavení. Chcete-li maximalizovat efektivitu, Azure Backup identifikuje a přenáší pouze bloky dat, které se změnily od předchozí zálohy.

Po dokončení přenosu dat se odebere snímku a vytvoří bod obnovení.


## <a name="delete-a-file-from-a-vm"></a>Odstranění souboru z virtuálního počítače
Pokud omylem odstraníte nebo provést změny do souboru, můžete obnovit jednotlivé soubory z bodu obnovení. Tento proces vám umožňuje procházet soubory zálohovat v bodu obnovení a obnovit pouze soubory, které potřebujete. V tomto příkladu jsme odstranit soubor z webového serveru k předvedení proces obnovení na úrovni souborů.

1. Chcete-li připojit k virtuálnímu počítači, získat IP adresu vašeho virtuálního počítače s [az virtuálních počítačů zobrazit](/cli/azure/vm?view=azure-cli-latest#az_vm_show):

     ```azurecli-interactive
     az vm show --resource-group myResourceGroup --name myVM -d --query [publicIps] --o tsv
     ```

2. Potvrďte, že váš web z umístění aktuálně funguje, otevřete webový prohlížeč na veřejnou IP adresu vašeho virtuálního počítače. Ponechte okno prohlížeče otevřené.

    ![Výchozí NGINX webové stránky](./media/tutorial-restore-files/nginx-working.png)

3. Připojte k virtuálnímu počítači pomocí protokolu SSH. Nahraďte *publicIpAddress* s veřejnou IP adresu, kterou jste získali v předchozím příkazem:

    ```bash
    ssh publicIpAddress
    ```

4. Odstranit výchozí stránky z webového serveru na */var/www/html/index.nginx-debian.html* následujícím způsobem:

    ```bash
    sudo rm /var/www/html/index.nginx-debian.html
    ```

5. Ve webovém prohlížeči aktualizujte webové stránky. Webová stránka již načte stránku, jak je znázorněno v následujícím příkladu:

    ![NGINX webový server už načte výchozí stránka](./media/tutorial-restore-files/nginx-broken.png)

6. Ukončení relace SSH k virtuálnímu počítači následujícím způsobem:

    ```bash
    exit
    ```


## <a name="generate-file-recovery-script"></a>Generovat skript obnovení souboru
Obnovení souborů, poskytuje Azure Backup skriptu ke spuštění na vašem virtuálním počítači, který se připojuje bod obnovení jako místní disk. Můžete procházet tento místní disk, obnovení souborů na virtuální počítač a pak odpojit bod obnovení. Zálohování Azure i nadále zálohovat data na základě přiřazené zásad pro plán a jejich uchovávání.

1. Do seznamu bodů obnovení pro virtuální počítač, použijte [az zálohování recoverypoint seznamu](https://docs.microsoft.com/cli/azure/backup/recoverypoint?view=azure-cli-latest#az_backup_recoverypoint_list). V tomto příkladu jsme vyberte nejnovější bod obnovení pro virtuální počítač s názvem *Můjvp* které je chráněné v *myRecoveryServicesVault*:

    ```azurecli-interactive
    az backup recoverypoint list \
        --resource-group myResourceGroup \
        --vault-name myRecoveryServicesVault \
        --container-name myVM \
        --item-name myVM \
        --query [0].name \
        --output tsv
    ```

2. Chcete-li získat skript, který připojí nebo připojí k bodu obnovení do virtuálního počítače, použijte [az při obnovení zálohy souborů připojení rp](https://docs.microsoft.com/cli/azure/backup/restore/files?view=azure-cli-latest#az_backup_restore_files_mount_rp). Následující příklad získá skript pro virtuální počítač s názvem *Můjvp* které je chráněné v *myRecoveryServicesVault*.

    Nahraďte *myRecoveryPointName* s názvem bod obnovení, který jste získali v předchozím příkazu:

    ```azurecli-interactive
    az backup restore files mount-rp \
        --resource-group myResourceGroup \
        --vault-name myRecoveryServicesVault \
        --container-name myVM \
        --item-name myVM \
        --rp-name myRecoveryPointName
    ```

    Skript se stáhne a hesla se zobrazí, jako v následujícím příkladu:

    ```
    File downloaded: myVM_we_1571974050985163527.sh. Use password c068a041ce12465
    ```

3. Přenos skript do virtuálního počítače, použijte zabezpečené kopírování (SCP). Zadejte název staženého skriptu a nahraďte *publicIpAddress* s veřejnou IP adresu vašeho virtuálního počítače. Zajistěte, aby zahrnout koncový znak `:` na konci spojovací bod služby příkaz takto:

    ```bash
    scp myVM_we_1571974050985163527.sh 52.174.241.110:
    ```


## <a name="restore-file-to-your-vm"></a>Obnovte soubor k virtuálnímu počítači
Pomocí skriptu obnovení zkopírován do virtuálního počítače teď můžete připojit bod obnovení a obnovení souborů.

1. Připojte k virtuálnímu počítači pomocí protokolu SSH. Nahraďte *publicIpAddress* s veřejnou IP adresu vašeho virtuálního počítače následujícím způsobem:

    ```bash
    ssh publicIpAddress
    ```

2. Chcete-li povolit správné fungování vašeho skriptu, přidejte oprávnění s ke spouštění **chmod**. Zadejte název vlastní skript:

    ```bash
    chmod +x myVM_we_1571974050985163527.sh
    ```

3. Chcete-li přípojný bod obnovení, spusťte skript. Zadejte název vlastní skript:

    ```bash
    ./myVM_we_1571974050985163527.sh
    ```

    Jako skript se spustí, zobrazí se výzva k zadání hesla pro přístup k bodu obnovení. Zadejte heslo zobrazené ve výstupu z předchozí [az při obnovení zálohy souborů připojení rp](https://docs.microsoft.com/cli/azure/backup/restore/files?view=azure-cli-latest#az_backup_restore_files_mount_rp) příkaz, který generuje skript obnovení.

    Výstup ze skriptu získáte cestu pro bod obnovení. Následující příklad výstupu ukazuje, že bod obnovení je připojen */home/azureuser/myVM-20170919213536/Volume1*:

    ```
    Microsoft Azure VM Backup - File Recovery
    ______________________________________________
    Please enter the password as shown on the portal to securely connect to the recovery point. : c068a041ce12465
    
    Connecting to recovery point using ISCSI service...
    
    Connection succeeded!
    
    Please wait while we attach volumes of the recovery point to this machine...
    
    ************ Volumes of the recovery point and their mount paths on this machine ************
    
    Sr.No.  |  Disk  |  Volume  |  MountPath
    
    1)  | /dev/sdc  |  /dev/sdc1  |  /home/azureuser/myVM-20170919213536/Volume1
    
    ************ Open File Explorer to browse for files. ************
    ```

4. Použití **cp** zkopírujte NGINX výchozí webové stránky z bodu obnovení připojené zpět do původního umístění souboru. Nahraďte */home/azureuser/myVM-20170919213536/Volume1* přípojný bod s vlastní umístění:

    ```bash
    sudo cp /home/azureuser/myVM-20170919213536/Volume1/var/www/html/index.nginx-debian.html /var/www/html/
    ```

6. Ve webovém prohlížeči aktualizujte webové stránky. Na webu nyní načte správně znovu, jak je znázorněno v následujícím příkladu:

    ![NGINX webu nyní načte správně](./media/tutorial-restore-files/nginx-restored.png)

7. Ukončení relace SSH k virtuálnímu počítači následujícím způsobem:

    ```bash
    exit
    ```

8. Odpojit bod obnovení z virtuálního počítače s [obnovení zálohy az soubory odpojit rp](https://docs.microsoft.com/cli/azure/backup/restore/files?view=azure-cli-latest#az_backup_restore_files_unmount_rp). Následující příklad odpojí bod obnovení z virtuálního počítače s názvem *Můjvp* v *myRecoveryServicesVault*.

    Nahraďte *myRecoveryPointName* s názvem vaší bod obnovení, který jste získali v předchozích příkazů:
    
    ```azurecli-interactive
    az backup restore files unmount-rp \
        --resource-group myResourceGroup \
        --vault-name myRecoveryServicesVault \
        --container-name myVM \
        --item-name myVM \
        --rp-name myRecoveryPointName
    ```

## <a name="next-steps"></a>Další kroky
V tomto kurzu připojený k virtuálnímu počítači bod obnovení a obnovit soubory pro webový server. Naučili jste se tyto postupy:

> [!div class="checklist"]
> * Seznam a bodů obnovení vyberte
> * Připojení k virtuálnímu počítači bod obnovení
> * Obnovit soubory z bodu obnovení

Přechodu na v dalším kurzu se dozvíte o tom, jak zálohovat Windows serveru do Azure.

> [!div class="nextstepaction"]
> [Zálohování serveru Windows Azure](tutorial-backup-windows-server-to-azure.md)

