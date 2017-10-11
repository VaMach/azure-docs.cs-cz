---
title: "Příprava cílového (fyzické do Azure) | Microsoft Docs"
description: "Tento článek popisuje postup přípravy prostředí Azure pro zahájení replikace fyzických serverů s Windows nebo Linuxem do Azure."
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
ms.openlocfilehash: aa7a32ace8354f615a8b8cc137f6bdf48fbadf48
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="prepare-target-vmware-to-azure"></a>Příprava cílového (VMware do Azure)
> [!div class="op_single_selector"]
> * [Z VMware do Azure](./site-recovery-prepare-target-vmware-to-azure.md)
> * [Fyzické do Azure](./site-recovery-prepare-target-physical-to-azure.md)

Tento článek popisuje postup přípravy prostředí Azure pro zahájení replikace fyzických serverů (x 64) se systémem Windows nebo Linux do Azure.

## <a name="prerequisites"></a>Požadavky

Článek předpokládá následující:
- Jste vytvořili trezor služeb zotavení k ochraně fyzických serverů. Můžete vytvořit trezor služeb zotavení z [portál Azure](http://portal.azure.com "portál Azure").
- Máte [instalaci prostředí místní](./site-recovery-set-up-physical-to-azure.md) replikace fyzických serverů do Azure.

## <a name="prepare-target"></a>Příprava cílového

Po dokončení **cíl ochrany krok 1: vyberte** a **krok 2: Příprava zdroj**, přejdete na **krok 3: cíl**

![Příprava cílového](./media/site-recovery-prepare-target-physical-to-azure/prepare-target-physical-to-azure.png)

1. **Předplatné:** z rozevírací nabídky vyberte odběr, který chcete replikovat fyzických serverů.
2. **Model nasazení:** vyberte model nasazení (Classic nebo Resource Manager)

Podle modelu vybrané nasazení, je zajistit, že máte alespoň jeden účet kompatibilní úložiště a virtuální sítě v cílové předplatné pro replikaci a převzetí služeb při selhání vaší fyzických serverů na spusťte ověřování.

Jakmile ověřovací úspěšně dokončit, klikněte na tlačítko OK přejdete k dalšímu kroku.

Pokud nemáte účet úložiště kompatibilní Resource Manager nebo virtuální sítě, nebo chcete přidat více, můžete provést kliknutím **+ účet úložiště** nebo **+ síť** tlačítka nahoře v okně.

## <a name="next-steps"></a>Další kroky
[Konfigurace nastavení replikace](./site-recovery-setup-replication-settings-vmware.md).
