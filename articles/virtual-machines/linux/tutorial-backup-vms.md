---
title: "Zálohování virtuálních počítačů Azure Linux | Microsoft Docs"
description: "Chraňte vaše virtuální počítače s Linuxem pomocí zálohování pomocí služby Azure Backup."
services: virtual-machines-linux
documentationcenter: virtual-machines
author: cynthn
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 07/27/2017
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: 0e659c1906c99415ab1b53785a606330ef9068c8
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="back-up-linux--virtual-machines-in-azure"></a>Zálohovat virtuální počítače s Linuxem v Azure

Svá data můžete chránit prováděním záloh v pravidelných intervalech. Zálohování Azure vytvoří body obnovení, které jsou uložené v geograficky redundantní obnovení trezorů. Při obnovení z bodu obnovení můžete obnovit celý virtuální počítač nebo jenom určité soubory. Tento článek vysvětluje, jak k obnovení virtuálního počítače s Linuxem systémem nginx jeden soubor. Pokud ještě nemáte virtuální počítač používat, můžete vytvořit jeden pomocí [rychlý start Linux](quick-create-cli.md). V tomto kurzu se naučíte:

> [!div class="checklist"]
> * Vytvoření zálohy virtuálního počítače
> * Naplánovat denní zálohování
> * Obnovte soubor ze zálohy



## <a name="backup-overview"></a>Přehled služby Backup

Když služba Azure Backup zahájí zálohu, aktivuje rozšíření zálohování k pořízení snímku v daném okamžiku. Použití služby Azure Backup _VMSnapshotLinux_ rozšíření v systému Linux. Rozšíření je nainstalován během první zálohování virtuálních počítačů, pokud je virtuální počítač spuštěný. Pokud virtuální počítač není spuštěný, služba zálohování pořídí snímek podkladové úložiště (protože žádné aplikace zápisy dojít při zastavení virtuálního počítače).

Ve výchozím nastavení, zálohování Azure trvá konzistentní zálohu systému souborů pro virtuální počítač s Linuxem ale dá se provést [aplikace konzistentní zálohování pomocí skriptů před a po skript rozhraní](https://docs.microsoft.com/azure/backup/backup-azure-linux-app-consistent). Jakmile služba Azure Backup používá snímku, data se přenáší do trezoru. Pokud chcete maximalizovat efektivitu, služba identifikuje a přenáší pouze bloky dat, které se změnily od předchozí zálohy.

Po dokončení přenosu dat se odebere snímku a vytvoří bod obnovení.


## <a name="create-a-backup"></a>Vytvoření zálohy
Vytvořte jednoduché plánované denní zálohování do trezoru služby Recovery Services. 

1. Přihlaste se k webu [Azure Portal](https://portal.azure.com/).
2. V nabídce na levé straně vyberte **Virtuální počítače**. 
3. V seznamu vyberte virtuální počítač, který chcete zálohovat.
4. V okně virtuálního počítače v **nastavení** klikněte na tlačítko **zálohování**. **Povolit zálohování** otevře se okno.
5. V **trezor služeb zotavení**, klikněte na tlačítko **vytvořit nový** a zadejte název pro nový trezor. Nový trezor se vytvoří ve stejné skupině prostředků a umístění jako virtuální počítač.
6. Klikněte na tlačítko **zálohování zásad**. V tomto příkladu ponechejte výchozí hodnoty a klikněte na tlačítko **OK**.
7. Na **povolit zálohování** okně klikněte na tlačítko **povolit zálohování**. Tím se vytvoří denní zálohování podle plánu, výchozí.
10. Vytvořit bod obnovení počáteční na **zálohování** okně klikněte na tlačítko **zálohovat nyní**.
11. Na **zálohovat nyní** okně klikněte na ikonu Kalendář, pomocí ovládacího prvku Kalendář vyberte poslední den tohoto bodu obnovení se zachovává a klikněte na tlačítko **zálohování**.
12. V **zálohování** okno pro virtuální počítač, zobrazí se počet bodů obnovení, které jsou dokončeny.

    ![Body obnovení](./media/tutorial-backup-vms/backup-complete.png)

První zálohování trvá asi 20 minut. Po dokončení zálohování, přejděte k další části tohoto kurzu.

## <a name="restore-a-file"></a>Obnovit soubor

Pokud omylem odstraníte nebo provést změny do souboru, můžete obnovit soubor z trezoru zálohování obnovení souborů. Obnovení souborů používá skript, který běží na virtuální počítač, přípojný bod obnovení jako místní disk. Tyto disky zůstanou připojené 12 hodin, aby mohli zkopírovat soubory z bodu obnovení a obnovit je do virtuálního počítače.  

V tomto příkladu ukážeme, jak obnovit /var/www/html/index.nginx-debian.html výchozí nginx webové stránky. Veřejná IP adresa naše virtuálního počítače v tomto příkladu je *13.69.75.209*. Můžete najít IP adresu virtuální počítač pomocí:

 ```bash 
 az vm show --resource-group myResourceGroup --name myVM -d --query [publicIps] --o tsv
 ```

 
1. V místním počítači otevřete prohlížeč a zadejte veřejnou IP adresu vašeho virtuálního počítače zobrazíte výchozí nginx webové stránky.

    ![Výchozí nginx webové stránky](./media/tutorial-backup-vms/nginx-working.png)

1. SSH ke svému virtuálnímu počítači.

    ```bash
    ssh 13.69.75.209
    ```
2. Odstraňte /var/www/html/index.nginx-debian.html.

    ```bash
    sudo rm /var/www/html/index.nginx-debian.html
    ```
    
4. V místním počítači aktualizujte stránku prohlížeče zasažení kombinaci kláves CTRL + F5 zobrazíte této stránce nginx výchozí je pryč.

    ![Výchozí nginx webové stránky](./media/tutorial-backup-vms/nginx-broken.png)
    
1. V místním počítači, přihlaste se k [portál Azure](https://portal.azure.com/).
6. V nabídce na levé straně vyberte **Virtuální počítače**. 
7. V seznamu vyberte virtuální počítač.
8. V okně virtuálního počítače v **nastavení** klikněte na tlačítko **zálohování**. **Zálohování** otevře se okno. 
9. V nabídce v horní části okna vyberte **obnovení souboru**. **Obnovení souboru** otevře se okno.
10. V **krok 1: Vyberte bod obnovení**, vyberte bod obnovení z rozevíracího seznamu.
11. V **krok 2: stáhnout skript a procházet a obnovit soubory**, klikněte **spustitelný soubor stáhnout** tlačítko. Uložte stažený soubor do místního počítače.
7. Klikněte na tlačítko **stáhnout skript** ke stažení souboru skriptu místně.
8. Otevřete Bash řádku a zadejte následující příkaz, nahraďte *Linux_myVM_05-05-2017.sh* se správnou cestu a název souboru pro skript, který jste stáhli, *azureuser* s uživatelským jménem pro virtuální počítač a *13.69.75.209* s veřejnou IP adresu pro virtuální počítač.
    
    ```bash
    scp Linux_myVM_05-05-2017.sh azureuser@13.69.75.209:
    ```
    
9. V místním počítači otevřete připojení SSH pro virtuální počítač.

    ```bash
    ssh 13.69.75.209
    ```
    
10. Na virtuální počítač, přidejte oprávnění k souboru skriptu ke spouštění.

    ```bash
    chmod +x Linux_myVM_05-05-2017.sh
    ```
    
11. Na vašem virtuálním počítači spusťte skript pro přípojný bod obnovení jako systému souborů.

    ```bash
    ./Linux_myVM_05-05-2017.sh
    ```
    
12. Výstup ze skriptu vám cestu pro přípojného bodu. Výstup vypadá podobně jako tento:

    ```bash
    Microsoft Azure VM Backup - File Recovery
    ______________________________________________
                          
    Connecting to recovery point using ISCSI service...
    
    Connection succeeded!
    
    Please wait while we attach volumes of the recovery point to this machine...
                         
    ************ Volumes of the recovery point and their mount paths on this machine ************

    Sr.No.  |  Disk  |  Volume  |  MountPath 

    1)  | /dev/sdc  |  /dev/sdc1  |  /home/azureuser/myVM-20170505191055/Volume1

    ************ Open File Explorer to browse for files. ************

    After recovery, to remove the disks and close the connection to the recovery point, please click 'Unmount Disks' in step 3 of the portal.

    Please enter 'q/Q' to exit...
    ```

12. Na virtuální počítač zkopírujte z přípojného bodu zpět do které jste odstranili soubor nginx výchozí webové stránky.

    ```bash
    sudo cp ~/myVM-20170505191055/Volume1/var/www/html/index.nginx-debian.html /var/www/html/
    ```
    
17. V místním počítači otevřete kartu prohlížeče, kde jste připojeni k IP adresu virtuálního počítače výchozí stránkou nginx. Stisknutím kláves CTRL + F5 aktualizujte stránku prohlížeče. Teď byste měli vidět, že výchozí stránky znovu funguje.

    ![Výchozí nginx webové stránky](./media/tutorial-backup-vms/nginx-working.png)

18. V místním počítači, přejděte zpět na záložce prohlížeče pro portál Azure a v **krok 3: odpojení disky po obnovení** klikněte na tlačítko **odpojit disky** tlačítko. Pokud zapomenete tento krok, připojení k přípojný bod je automaticky zavřít po 12 hodinách. Po těchto 12 hodin budete muset stáhnout nový skript pro vytvoření nové přípojný bod.


## <a name="next-steps"></a>Další kroky

V tomto kurzu jste se naučili:

> [!div class="checklist"]
> * Vytvoření zálohy virtuálního počítače
> * Naplánovat denní zálohování
> * Obnovte soubor ze zálohy

Přechodu na v dalším kurzu se dozvíte o monitorování virtuálních počítačů.

> [!div class="nextstepaction"]
> [Monitorování virtuálních počítačů](tutorial-monitoring.md)

