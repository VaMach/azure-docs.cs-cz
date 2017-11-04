---
title: "Příprava cílového (VMware do Azure) | Microsoft Docs"
description: "Tento článek popisuje postup přípravy prostředí Azure pro zahájení replikace virtuálních počítačů VMware do Azure."
services: site-recovery
documentationcenter: 
author: bsiva
manager: abhemraj
editor: 
ms.assetid: 
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: backup-recovery
ms.date: 5/31/2017
ms.author: bsiva
ms.openlocfilehash: c84a775564769ddc796aa9d75add019ef1003175
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="prepare-target-vmware-to-azure"></a>Příprava cílového (VMware do Azure)
> [!div class="op_single_selector"]
> * [Z VMware do Azure](./site-recovery-prepare-target-vmware-to-azure.md)
> * [Fyzické do Azure](./site-recovery-prepare-target-physical-to-azure.md)

Tento článek popisuje postup přípravy prostředí Azure pro zahájení replikace virtuálních počítačů VMware do Azure.

## <a name="prerequisites"></a>Požadavky

Článek předpokládá následující:
- Jste vytvořili trezor služeb zotavení k ochraně virtuálních počítačů VMware. Můžete vytvořit trezor služeb zotavení z [portál Azure](http://portal.azure.com "portál Azure").
- Máte [instalaci prostředí místní](./site-recovery-set-up-vmware-to-azure.md) k replikaci virtuálních počítačů VMware do Azure.

## <a name="prepare-target"></a>Příprava cílového

Po dokončení **cíl ochrany krok 1: vyberte** a **krok 2: Příprava zdroj**, přejdete na **krok 3: cíl**

![Příprava cílového](./media/site-recovery-prepare-target-vmware-to-azure/prepare-target-vmware-to-azure.png)

1. **Předplatné:** z rozevírací nabídky vyberte odběr, který chcete replikovat virtuální počítače.
2. **Model nasazení:** vyberte model nasazení (Classic nebo Resource Manager)

Založená na modelu vybrané nasazení, je zajistit, že máte alespoň jeden účet kompatibilní úložiště a virtuální sítě v cílové předplatné pro replikaci a převzetí služeb při selhání virtuálního počítače do spusťte ověřování.

Jakmile ověřovací úspěšně dokončit, klikněte na tlačítko OK přejdete k dalšímu kroku.

Pokud nemáte účet úložiště kompatibilní Resource Manager nebo virtuální sítě, nebo chcete přidat více, můžete provést kliknutím **+ účet úložiště** nebo **+ síť** tlačítka nahoře v okně.

## <a name="next-steps"></a>Další kroky
[Konfigurace nastavení replikace](./site-recovery-setup-replication-settings-vmware.md).
