---
title: "Zálohování virtuálních počítačů Azure | Microsoft Docs"
description: "Zjistit, zaregistrujte a zálohování virtuálních počítačů Azure do trezoru služeb zotavení."
services: backup
documentationcenter: 
author: markgalioto
manager: carmonm
editor: 
keywords: "zálohování virtuálních počítačů; zálohování virtuálního počítače; zálohování a zotavení po havárii; zálohování virtuálních počítačů arm"
ms.assetid: 5c68481d-7be3-4e68-b87c-0961c267053e
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 8/15/2017
ms.author: trinadhk;jimpark;markgal;
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 892a88a2bc9d492f8a3afe59c05b4729f4830e6d
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/21/2017
---
# <a name="back-up-azure-virtual-machines-to-a-recovery-services-vault"></a>Zálohování virtuálních počítačů Azure do trezoru služby Recovery Services

Tento článek podrobné informace o tom, jak zálohovat virtuální počítače Azure (nasazení Resource Manager i nasazení Classic) do trezoru služeb zotavení. Většina práce pro zálohování virtuálních počítačů je přípravy. Před zahájením zálohování nebo chránit virtuální počítač, musíte provést [požadavky](backup-azure-arm-vms-prepare.md) při přípravě svého prostředí pro ochranu virtuálních počítačů. Po dokončení požadavky, můžete spustit zálohování udělat snímky virtuálního počítače.


[!INCLUDE [learn about backup deployment models](../../includes/backup-deployment-models.md)]

Další informace najdete v článcích na [plánování infrastruktury zálohování virtuálních počítačů v Azure](backup-azure-vms-introduction.md) a [virtuální počítače Azure](https://azure.microsoft.com/documentation/services/virtual-machines/).

## <a name="triggering-the-backup-job"></a>Spuštění úlohy zálohování
Zásady zálohování, které jsou přidružené k trezoru služeb zotavení definuje, jak často a kdy se spustí operace zálohování. Ve výchozím nastavení je prvním plánovaným zálohováním prvotní zálohování. Než proběhne prvotní zálohování, bude Stav poslední zálohy v okně **Úlohy zálohování** ukazovat **Upozornění (nedokončené prvotní zálohování)**.

![Zálohování čeká na zpracování](./media/backup-azure-vms-first-look-arm/initial-backup-not-run.png)

Pokud nemá prvotní zálohování brzy začít, doporučujeme spustit **Zálohovat nyní**. Následující postup se spustí na řídicím panelu trezoru. Tento postup slouží ke spuštění úlohu prvotního zálohování po dokončení všech požadavků. Pokud již byl spuštěn úlohu prvotního zálohování, tento postup není k dispozici. Přidružené zásady zálohování Určuje další úlohy zálohování.  

Spuštění úlohy prvotního zálohování:

1. Na řídicím panelu trezoru klikněte na číslo pod záznamem **Zálohování položek** nebo klikněte na dlaždici **Zálohování položek**. <br/>
  ![Ikona nastavení](./media/backup-azure-vms-first-look-arm/rs-vault-config-vm-back-up-now-1.png)

  Otevře se okno **Zálohování položek**

  ![Zálohování položek](./media/backup-azure-vms-first-look-arm/back-up-items-list.png)

2. V okně **Zálohování položek** vyberte položku.

  ![Ikona nastavení](./media/backup-azure-vms-first-look-arm/back-up-items-list-selected.png)

  Otevře se seznam **Zálohování položek**. <br/>

  ![Aktivovaná úloha zálohování.](./media/backup-azure-vms-first-look-arm/backup-items-not-run.png)

3. V seznamu **Zálohování položek** klikněte na **...** (tři tečky) a otevřete místní nabídku.

  ![Místní nabídka](./media/backup-azure-vms-first-look-arm/context-menu.png)

  Zobrazí se místní nabídka.

  ![Místní nabídka](./media/backup-azure-vms-first-look-arm/context-menu-small.png)

4. V místní nabídce klikněte na **Zálohovat nyní**.

  ![Místní nabídka](./media/backup-azure-vms-first-look-arm/context-menu-small-backup-now.png)

  Otevře se okno Zálohovat nyní.

  ![ukazuje okno Zálohovat nyní](./media/backup-azure-vms-first-look-arm/backup-now-blade-short.png)

5. V okně Zálohovat nyní klikněte na ikonu kalendáře, pomocí ovládacího prvku kalendáře vyberte poslední den uchování tohoto bodu obnovení a klikněte na **Zálohovat**.

  ![nastavte poslední den uchování bodu obnovení vytvořeného pomocí možnosti Zálohovat nyní](./media/backup-azure-vms-first-look-arm/backup-now-blade-calendar.png)

  Oznámení nasazení vás budou informovat o aktivaci úlohy zálohování a možnosti sledovat průběh úlohy na stránce Úlohy zálohování. V závislosti na velikosti virtuálního počítače může vytváření prvotní zálohy chvíli trvat.

6. Pokud chcete zobrazit nebo sledovat stav prvotní zálohy, na řídicím panelu trezoru na dlaždici **Úlohy zálohování** klikněte na **Probíhající**.

  ![Dlaždice Úlohy zálohování](./media/backup-azure-vms-first-look-arm/open-backup-jobs-1.png)

  Otevře se okno Úlohy zálohování

  ![Dlaždice Úlohy zálohování](./media/backup-azure-vms-first-look-arm/backup-jobs-in-jobs-view-1.png)

  V okně **Úlohy zálohování** vidíte stav všech úloh. Zkontrolujte, jestli úloha zálohování vašeho virtuálního počítače stále probíhá, nebo se dokončila. Po dokončení úlohy zálohování se stav změní na *Dokončeno*.

  > [!NOTE]
  > Jako součást operace zálohování vydá služba Azure Backup rozšířením zálohování v každém virtuálním počítači příkaz k vyprázdnění všech zápisů a pořízení konzistentního snímku.
  >
  >

## <a name="troubleshooting-errors"></a>Řešení potíží s chybami
Pokud narazíte na problémy při zálohování virtuálního počítače, přečtěte si téma [článku Poradce při potížích virtuálních počítačů](backup-azure-vms-troubleshoot.md) nápovědu.

## <a name="next-steps"></a>Další kroky
Teď, když chráníte virtuální počítač, najdete v následujících článcích Další informace o úlohách správy virtuálních počítačů a obnovení virtuálních počítačů.

* [Správa a monitorování virtuálních počítačů](backup-azure-manage-vms.md)
* [Obnovení virtuálních počítačů](backup-azure-arm-restore-vms.md)
