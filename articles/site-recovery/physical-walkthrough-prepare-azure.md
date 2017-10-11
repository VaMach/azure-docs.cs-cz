---
title: "Příprava prostředků Azure, aby replikace místní fyzických serverů do Azure pomocí Azure Site Recovery | Microsoft Docs"
description: "Popisuje, co potřebujete zavedené v Azure před zahájením replikace místních serverů do Azure pomocí služby Azure Site Recovery"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 4e320d9b-8bb8-46bb-ba21-77c5d16748ac
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/25/2017
ms.author: raynew
ms.openlocfilehash: b7411fa6aba04ffd34f3f4bd03e706ca75afc9c8
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="step-5-prepare-azure-resources-for-physical-server-replication-to-azure"></a>Krok 5: Příprava prostředků Azure pro fyzický server replikaci do Azure.


Postupujte podle pokynů v tomto článku Příprava prostředků Azure tak, aby místních serverů se můžou replikovat do Azure pomocí [Azure Site Recovery](site-recovery-overview.md) služby.

POST dotazy a na konci tohoto článku nebo na [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="before-you-start"></a>Než začnete

Zajistěte, aby si přečetli [požadavky](physical-walkthrough-prerequisites.md).

## <a name="set-up-an-azure-account"></a>Nastavit účet Azure

- Získání [účet Microsoft Azure](http://azure.microsoft.com/).
- Můžete začít s [bezplatnou zkušební verzí](https://azure.microsoft.com/pricing/free-trial/).
- Zkontrolujte podporované oblasti pro obnovení lokality v části **geografickou dostupností** v [Azure Site Recovery podrobnosti o cenách](https://azure.microsoft.com/pricing/details/site-recovery/).
- Další informace o [cenách za Site Recovery](site-recovery-faq.md#pricing)a získat [podrobnosti o cenách](https://azure.microsoft.com/pricing/details/site-recovery/).



## <a name="set-up-an-azure-network"></a>Nastavení sítě Azure

- Nastavte síť Azure. Virtuální počítače Azure jsou umístěny v této síti, když jste vytvořili po převzetí služeb při selhání.
- Site Recovery na portálu Azure můžete použít nastavení sítích [Resource Manager](../resource-manager-deployment-model.md), nebo v klasickém režimu.
- Síť by měla být ve stejném umístění jako trezor služby Recovery Services.
- Další informace o [ceny virtuální sítě](https://azure.microsoft.com/pricing/details/virtual-network/).
- Další informace o [připojení virtuálního počítače Azure](physical-walkthrough-network.md) po převzetí služeb při selhání.


## <a name="set-up-an-azure-storage-account"></a>Nastavení účtu úložiště Azure

- Obnovení lokality se replikují na místní servery do úložiště Azure. Virtuální počítače Azure jsou vytvořené z úložiště po převzetí služeb při selhání.
- Nastavení [účtu úložiště Azure](../storage/common/storage-create-storage-account.md#create-a-storage-account) pro replikovaná data.
- Site Recovery na portálu Azure můžete použít účty úložiště nastavit ve službě Správce prostředků, nebo v klasickém režimu.
- Účet úložiště může být standardní nebo [premium](../storage/common/storage-premium-storage.md).
- Pokud jste nastavili prémiového účtu taky musíte další standardní účet pro data protokolu.


## <a name="next-steps"></a>Další kroky

Přejděte na [krok 6: nastavení trezoru](physical-walkthrough-create-vault.md)
