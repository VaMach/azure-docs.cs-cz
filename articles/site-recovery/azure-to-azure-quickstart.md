---
title: "Replikaci virtuálního počítače Azure do jiné oblasti Azure (Preview)"
description: "Tento rychlý Start obsahuje kroky potřebné k replikaci virtuálního počítače Azure v jedné oblasti Azure v jiné oblasti."
services: site-recovery
author: rajani-janaki-ram
manager: carmonm
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/08/2017
ms.author: rajanaki
ms.custom: mvc
ms.openlocfilehash: 27066c3b5b9d52901dbd3790ba4b5abd97fe7d2b
ms.sourcegitcommit: 094061b19b0a707eace42ae47f39d7a666364d58
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/08/2017
---
# <a name="replicate-an-azure-vm-to-another-azure-region-preview"></a>Replikaci virtuálního počítače Azure do jiné oblasti Azure (Preview)

[Azure Site Recovery](site-recovery-overview.md) služby přispívá ke strategii obchodní kontinuitu a po havárii (BCDR) obnovení ponechat obchodních aplikací stále a spuštěním dostupné během plánovaných a neplánovaných výpadků. Site Recovery spravuje a orchestruje zotavení po havárii místní počítače a virtuální počítače Azure (VM), včetně replikace, převzetí služeb při selhání a obnovení.

Tento rychlý start popisuje, jak replikovat virtuální počítač Azure v jiné oblasti Azure.

Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) před tím, než začnete.

## <a name="log-in-to-azure"></a>Přihlaste se k Azure.

Přihlaste se k webu Azure Portal na adrese http://portal.azure.com.

## <a name="enable-replication-for-the-azure-vm"></a>Povolení replikace pro virtuální počítač Azure

1. Na portálu Azure klikněte na tlačítko **virtuální počítače**a vyberte virtuální počítač, který chcete replikovat.

2. V **nastavení**, klikněte na tlačítko **zotavení po havárii (preview)**.
3. V **konfigurace zotavení po havárii** > **cílová oblast** vyberte oblast cíl, do kterého budete replikovat.
4. Pro tento rychlý start přijměte výchozí nastavení.
5. Klikněte na tlačítko **povolit replikaci**. Spustí se úloha k povolení replikace pro virtuální počítač.

    ![Povolení replikace](media/azure-to-azure-quickstart/enable-replication1.png)



## <a name="verify-settings"></a>Ověřte nastavení

Po dokončení úlohy replikace můžete zkontrolovat stav replikace, upravit nastavení replikace a testování nasazení.

1. V nabídce virtuálního počítače klikněte na tlačítko **zotavení po havárii (preview)**.
2. Můžete zkontrolovat stav replikace, body obnovení, které byly vytvořeny a zdrojové a cílové oblasti na mapě.

   ![Stav replikace](media/azure-to-azure-quickstart/replication-status.png)

## <a name="clean-up-resources"></a>Vyčištění prostředků

Virtuální počítač v primární oblasti zastaví replikaci při zakázání pro něho replikaci:

- Nastavení replikace zdroje jsou automaticky vyčistí.
- Site Recovery fakturace pro virtuální počítač také zastaví.

Zastavení replikace následujícím způsobem:

1. Vyberte virtuální počítač.
2. V **zotavení po havárii (preview)**, klikněte na tlačítko **Další**.
3. Klikněte na tlačítko **zakažte replikaci**.

   ![Zakázat replikaci](media/azure-to-azure-quickstart/disable2-replication.png)

## <a name="next-steps"></a>Další kroky

V tento rychlý start replikují jeden virtuální počítač v sekundární oblasti.

> [!div class="nextstepaction"]
> [Konfigurace obnovení po havárii pro virtuální počítače Azure](azure-to-azure-tutorial-enable-replication.md)
