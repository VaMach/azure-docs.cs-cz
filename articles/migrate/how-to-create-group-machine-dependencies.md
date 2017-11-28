---
title: "Skupina počítačů pomocí Azure migrovat počítač závislosti | Microsoft Docs"
description: "Popisuje postup vytvoření posouzení závislosti počítače pomocí služby Azure migrovat."
services: migrate
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 0527e34e-a078-405e-aeb9-c91a5808112a
ms.service: migrate
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 11/21/2017
ms.author: raynew
ms.openlocfilehash: 5ed49f3dc71af6a8c1c7b6c9e38fd84452505aec
ms.sourcegitcommit: 5bced5b36f6172a3c20dbfdf311b1ad38de6176a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/27/2017
---
# <a name="group-machines-using-machine-dependency-mapping"></a>Skupina počítačů pomocí mapování závislostí počítače

Tento článek popisuje, jak vytvořit skupiny počítačů pro [Azure migrovat](migrate-overview.md) assessment pomocí mapování závislostí počítače. Obvykle použijete tuto metodu, když chcete posoudit skupiny virtuálních počítačů s vyšší úrovní spolehlivosti kontrola mezi závislostmi počítač, před spuštěním posouzení.



## <a name="prepare-machines-for-dependency-mapping"></a>Příprava počítače pro mapování závislostí
Mapování závislostí zahrnout počítače, musíte stáhnout a nainstalovat agenty na každý místní počítač, který chcete vyhodnotit. Kromě toho, pokud máte počítače bez připojení k Internetu, musíte stáhnout a nainstalovat [OMS brány](../log-analytics/log-analytics-oms-gateway.md) na ně.

### <a name="download-and-install-the-vm-agents"></a>Stáhněte a nainstalujte agenty virtuálních počítačů
1. V **přehled**, klikněte na tlačítko **spravovat** > **počítače**a vyberte požadovaný počítač.
2. V **závislosti** sloupce, klikněte na tlačítko **nainstalovat agenty**. 
3. Na **závislosti** stránky, stáhněte a nainstalujte Microsoft Monitoring Agent (MMA) a agent závislost na jednotlivé virtuální počítače, kterou chcete vyhodnotit.
4. Zkopírujte ID a klíč. Je třeba tyto při instalaci MMA v místním počítači.

### <a name="install-the-mma"></a>Nainstalujte

Instalace agenta na počítači s Windows:

1. Poklikejte na stažený agenta.
2. Na **úvodní** klikněte na tlačítko **Další**. Na **licenční podmínky** klikněte na tlačítko **souhlasím** tak, aby přijímal licence.
3. V **cílovou složku**, zachovat, nebo upravit výchozí instalační složku > **Další**. 
4. V **možnosti instalace agenta**, vyberte **Azure Log Analytics (OMS)** > **Další**. 
5. Klikněte na tlačítko **přidat** přidat nový pracovní prostor OMS. Vložte ID a klíč, který jste zkopírovali z portálu. Klikněte na **Další**.


Instalace agenta na počítač s Linuxem:

1. Přeneste příslušné sady (x86 nebo x64) do počítače Linux pomocí spojovací bod služby/sftp.
2. Instalaci sady pomocí argument--instalace.

    ```sudo sh ./omsagent-<version>.universal.x64.sh --install -w <workspace id> -s <workspace key>```


### <a name="install-the-dependency-agent"></a>Nainstalujte agenta závislostí
1. K instalaci agenta závislost na počítači s Windows, klikněte dvakrát na instalační soubor a postupujte podle pokynů průvodce.
2. Pro instalaci agenta závislost na počítač s Linuxem nainstalujte jako kořenového adresáře pomocí následujícího příkazu:

    ```sh InstallDependencyAgent-Linux64.bin```

[Další informace](../operations-management-suite/operations-management-suite-service-map-configure.md#supported-operating-systems) o operačních systémech podporovaných produktem agenta závislostí. 

## <a name="create-a-group"></a>Vytvoření skupiny

1. Po instalaci agentů, přejděte na portál a klikněte na tlačítko **spravovat** > **počítače**.
2. **Závislosti** sloupec by měl nyní zobrazovat jako **zobrazení závislostí**. Klikněte na sloupec zobrazení závislostí.
3. Pro každý počítač můžete ověřit:
    - Zda jsou nainstalovány MMA a závislostí agent a zda na počítači byla zjištěna.
    - Hostovaný operační systém běžící v počítači.
    - Příchozí a odchozí připojení, IP a porty.
    - Procesy spuštěné na počítačích.
    - Závislosti mezi počítači.

4. Podrobnější závislosti klikněte na tlačítko časový rozsah a upravit ho. Ve výchozím nastavení je rozsahu jednu hodinu. Můžete upravit časové rozmezí, nebo zadejte počáteční a koncová data a doby trvání.
5. Když jste zformulovali závislé počítače, které chcete seskupit, vyberte počítače, na mapě a klikněte na **skupiny počítačů**.
6. Zadejte název skupiny. Ověřte, že je počítač rozpozná migraci Azure. Pokud zjišťování procesu místní ho není spustit znovu. Posouzení můžete spustit okamžitě, pokud chcete.
7. Klikněte na tlačítko **OK** uložte skupinu.

    ![Vytvořte skupinu s počítač závislosti](./media/how-to-create-group-machine-dependencies/create-group.png)

## <a name="next-steps"></a>Další kroky

- [Zjistěte, jak](how-to-create-group-dependencies.md) upřesněte skupině kontrolou skupiny závislosti
- [Další informace](concepts-assessment-calculation.md) o tom, jak jsou vypočítávány vyhodnocování.
