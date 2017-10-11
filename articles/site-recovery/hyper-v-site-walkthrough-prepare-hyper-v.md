---
title: "Příprava hostitele Hyper-V (bez System Center VMM) pro replikaci do Azure | Microsoft Docs"
description: "Popisuje postup přípravy hostitelů Hyper-V pro replikaci do Azure pomocí Azure Site Recovery"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 0f204e24-8d78-4076-95c5-8137d1be9c01
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 06/22/2017
ms.author: raynew
ms.openlocfilehash: f9bcaa8e55be6e8fddaf88ebc3f18f5dbb2811e4
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="step-6-prepare-hyper-v-hosts-for-replication-to-azure"></a>Krok 6: Příprava hostitele Hyper-V pro replikaci do Azure.

Příprava místního hostitele Hyper-V pro interakci s Azure Site Recovery, postupujte podle pokynů v tomto článku.

Po přečtení tohoto článku, post jakékoli komentáře v dolní části, nebo požádat technické dotazy na [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="prepare-hosts"></a>Příprava hostitele

- Ujistěte se, že splňují hostitelů Hyper-V [požadavky](site-recovery-prereq.md#disaster-recovery-of-hyper-v-vms-to-azure-no-vmm).
- Ujistěte se, že hostitelé mají přístup k požadované adresy URL:

    [!INCLUDE [site-recovery-URLS](../../includes/site-recovery-URLS.md)]
    
- Pokud máte zavedená pravidla brány firewall založená na IP adrese, zkontrolujte, že tato pravidla umožňují komunikaci s Azure.
- Povolte [Rozsahy IP adres datového centra Azure](https://www.microsoft.com/download/confirmation.aspx?id=41653) a port HTTPS (443).
- Povolte rozsahy IP adres pro oblast Azure svého předplatného a pro oblast Západní USA (používá se pro řízení přístupu a správu identit).

Během nasazování Site Recovery přidáte hostitele Hyper-V, které obsahují virtuální počítače, které chcete replikovat do lokality Hyper-V. Zprostředkovatele služby Site Recovery a agenta služeb zotavení jsou nainstalované na každém hostiteli. Lokality Hyper-V je zaregistrován v trezoru služeb zotavení.

## <a name="next-steps"></a>Další kroky

Přejděte na [krok 7: vytvoření trezoru](hyper-v-site-walkthrough-create-vault.md)

