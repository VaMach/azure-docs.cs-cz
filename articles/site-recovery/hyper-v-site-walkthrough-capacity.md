---
title: "Plánování kapacity a škálování pro replikaci virtuálních počítačů technologie Hyper-V (bez VMM) do Azure s Azure Site Recovery | Microsoft Docs"
description: "Použijte tento článek k plánování kapacity a škálování při replikaci virtuálních počítačů Hyper-V do Azure s Azure Site Recovery"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 8687af60-5b50-481c-98ee-0750cbbc2c57
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 06/21/2017
ms.author: raynew
ms.openlocfilehash: c7891c188c2cecbbf056fa79672a13bb16fa7fcf
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="step-3-plan-capacity-and-scaling-for-hyper-v-to-azure-replication"></a>Krok 3: Plánování kapacity a škálování pro Hyper-V na Azure replikaci

Pomocí tohoto článku a pokuste se zjistit plánování kapacity a škálování, při replikaci virtuálních počítačů technologie Hyper-V v místě (bez System Center VMM) do Azure s [Azure Site Recovery](site-recovery-overview.md).

Po přečtení tohoto článku, post jakékoli komentáře v dolní části, nebo požádat technické dotazy na [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="how-do-i-start-capacity-planning"></a>Jak spustit plánování kapacity?


Shromážděte informace o prostředí replikace a pak plánování kapacity, na základě těchto informací, společně s aspekty zvýrazněných v tomto článku.


## <a name="gather-information"></a>Shromážděte informace

1. Získat informace o prostředí replikace, včetně virtuální počítačů, disků na virtuální počítač a úložišti na disk.
2. Určete vaše denní míry změn s ohledem pro replikovaná data. Stažení [nástroj plánování kapacity služby technologie Hyper-V](https://www.microsoft.com/download/details.aspx?id=39057) získat míru změn. Doporučujeme, aby že tento nástroj spustíte přes týden k zachycení průměry.
 

## <a name="figure-out-capacity"></a>Zjistěte, kapacity

Na základě informací jste shromáždění, spusťte [nástroje Plánovač kapacity obnovení lokality](http://aka.ms/asr-capacity-planner-excel) analyzovat vaše zdrojové prostředí a úlohy, odhadnout požadavky na šířku pásma a prostředky serveru pro umístění zdroje a prostředky (virtuální počítače a úložiště atd), které je třeba v cílovém umístění. Nástroj můžete spustit v několika režimy:

- Rychlé plánování: spuštění nástroje v tomto režimu získat sítě a serveru projekce založené na průměrném počtu virtuálních počítačů, disků, úložiště a míru změn.
- Podrobné plánování: Spusťte nástroj v tomto režimu a uveďte podrobné informace o každé zatížení na úrovni virtuálního počítače. Analýza kompatibility virtuálních počítačů a získat projekce sítě a serveru.

Nyní spusťte nástroj:

1. Stažení [nástroj](http://aka.ms/asr-capacity-planner-excel)
2. Chcete-li spustit Rychlý planner, postupujte podle [tyto pokyny](site-recovery-capacity-planner.md#run-the-quick-planner)a vyberte scénáři **technologie Hyper-V do Azure**.
3. Chcete-li spustit podrobné planner, postupujte podle [tyto pokyny](site-recovery-capacity-planner.md#run-the-detailed-planner)a vyberte scénáři **technologie Hyper-V do Azure**.

## <a name="control-network-bandwidth"></a>Řídí šířku pásma sítě

Poté, co jste vypočítat šířku pásma, které potřebujete, máte několik možností pro řízení šířky pásma sítě používané pro replikaci:

* **Omezení šířky pásma**: přenos Hyper-V, která se replikují do Azure prochází konkrétního hostitele technologie Hyper-V. Šířku pásma na hostitelském serveru můžete omezit.
* **Ovlivnění šířky pásma**: můžete ovlivnit šířku pásma používanou pro replikaci pomocí několika klíčů registru.

### <a name="throttle-bandwidth"></a>Omezení šířky pásma
1. Otevřete modul snap-in Microsoft Azure Backup konzoly MMC na hostitelském serveru Hyper-V. Ve výchozím nastavení je na ploše nebo v umístění C:\Program Files\Microsoft Azure Recovery Services Agent\bin\wabadmin zástupce služby Microsoft Azure Backup.
2. V modulu snap-in klikněte na **Změnit vlastnosti**.
3. Na kartě **Omezování** vyberte **Povolit omezování šířky pásma internetu u operací zálohování** a nastavte limity pro pracovní a nepracovní dobu. Platné rozsahy jsou 512 kB/s až 102 MB/s.

    ![Omezení šířky pásma](./media/hyper-v-site-walkthrough-capacity/throttle2.png)

Pro nastavení omezování můžete také použít rutinu [Set OBMachineSetting](https://technet.microsoft.com/library/hh770409.aspx). Tady je ukázkový skript:

    $mon = [System.DayOfWeek]::Monday
    $tue = [System.DayOfWeek]::Tuesday
    Set-OBMachineSetting -WorkDay $mon, $tue -StartWorkHour "9:00:00" -EndWorkHour "18:00:00" -WorkHourBandwidth  (512*1024) -NonWorkHourBandwidth (2048*1024)

**Set-OBMachineSetting -NoThrottle** označuje, že není požadováno žádné omezování.

### <a name="influence-network-bandwidth"></a>Ovlivnění šířky pásma sítě
1. V registru přejděte na **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Replication**.
   * K ovlivnění šířky pásma provoz na replikaci disku, změňte hodnotu **UploadThreadsPerVM**, nebo vytvořte klíč, pokud neexistuje.
   * K ovlivnění šířky pásma pro přenosy navrácení služeb po obnovení z Azure, změňte hodnotu **DownloadThreadsPerVM**.
2. Výchozí hodnota je 4. V síti s „nadměrným zřízením“ je třeba tyto klíče registru změnit z výchozích hodnot. Maximum je 32. Monitorováním provozu hodnotu optimalizujte.

## <a name="next-steps"></a>Další kroky

Přejděte na [krok 4: plánování sítě](hyper-v-site-walkthrough-network.md).
