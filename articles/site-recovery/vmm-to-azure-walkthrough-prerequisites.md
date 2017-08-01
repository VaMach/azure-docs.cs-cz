---
title: "Kontrola požadavků pro replikaci Hyper-V do Azure (s nástrojem System Center VMM) s využitím služby Azure Site Recovery | Dokumentace Microsoftu"
description: "Popisuje požadavky pro nastavení replikace, převzetí služeb při selhání a obnovení místních virtuálních počítačů Hyper-V v cloudech VMM do Azure s využitím služby Azure Site Recovery."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: a1c30fd5-c979-473c-af44-4f725ad3e3ba
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/24/2017
ms.author: raynew
ms.translationtype: HT
ms.sourcegitcommit: 74b75232b4b1c14dbb81151cdab5856a1e4da28c
ms.openlocfilehash: 87a5d721ca785329b407d31126bd0b211b17ccf3
ms.contentlocale: cs-cz
ms.lasthandoff: 07/26/2017

---



# <a name="step-2-review-the-prerequisites-for-hyper-v-with-vmm-to-azure-replication"></a>Krok 2: Kontrola požadavků pro replikaci Hyper-V (s nástrojem VMM) do Azure

Po kontrole [architektury scénáře](vmm-to-azure-walkthrough-architecture.md) si přečtěte tento článek a ujistěte se, že rozumíte požadavkům na nasazení. 

## <a name="prerequisites-and-limitations"></a>Požadavky a omezení

**Požadavek** | **Podrobnosti**
--- | ---
**Účet Azure** | Potřebujete [účet Microsoft Azure](http://azure.microsoft.com/).
**Úložiště Azure** | K ukládání replikovaných dat potřebujete účet úložiště Azure.<br/><br/> Účet úložiště musí být ve stejné oblasti jako trezor služby Azure Recovery Services.<br/><br/>Můžete použít [geograficky redundantní úložiště](../storage/storage-redundancy.md#geo-redundant-storage) nebo místně redundantní úložiště. Doporučujeme použít geograficky redundantní úložiště. S geograficky redundantním úložištěm je zajištěna odolnost dat v případě výpadku oblasti nebo pokud není možné obnovit primární oblast.<br/><br/> Můžete použít účet úložiště Azure úrovně Standard nebo [Azure Storage úrovně Premium](../storage/storage-premium-storage.md). Služba Storage úrovně Premium může být hostitelem úloh náročných na vstupně-výstupní operace a obvykle se používá pro virtuální počítače, které potřebují trvale vysoký výkon vstupně-výstupních operací a nízkou latenci. Pokud použijete službu Storage úrovně Premium pro replikovaná data, potřebujete také účet úložiště úrovně Standard. V účtu úložiště úrovně Standard se ukládají protokoly replikace, do kterých se zaznamenávají průběžné změny místních dat.
**Síť Azure** | Potřebujete [síť Azure](../virtual-network/virtual-network-get-started-vnet-subnet.md), ke které se připojí virtuální počítače Azure po převzetí služeb při selhání. Síť Azure musí být ve stejné oblasti jako trezor služby Recovery Services.
**Místní servery VMM** | Potřebujete jeden nebo více serverů VMM s nástrojem System Center 2012 R2 nebo novějším.<br/><br/> Každý server VMM musí mít jeden nebo více privátních cloudů. Každý cloud potřebuje jednu nebo více skupin hostitelů.<br/><br/> Server VMM potřebuje mít přístup k internetu.
**Místní Hyper-V** | Na hostitelských serverech Hyper-V musí běžet minimálně Windows Server 2012 R2 s povolenou rolí Hyper-V nebo Microsoft Hyper-V Server 2012 R2. Musí být nainstalované nejnovější aktualizace.<br/><br/> Hostitel Hyper-V musí být umístěný ve skupině hostitelů VMM (umístěné v cloudu VMM).<br/><br/> Hostitel musí mít jeden nebo několik virtuálních počítačů, které chcete replikovat.<br/><br/> Hostitelé Hyper-V musí být pro replikaci do Azure připojeni k internetu, a to buď přímo nebo s použitím proxy. Na serverech Hyper-V musí být opravy popsané v článku [2961977](https://support.microsoft.com/kb/2961977).
**Místní virtuální počítače Hyper-V** | Na virtuálních počítačích, které chcete replikovat, musí běžet [podporovaný operační systém](site-recovery-support-matrix-to-azure.md#support-for-replicated-machine-os-versions) a počítače musí splňovat [požadavky na Azure](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements). Název virtuálního počítače je možné změnit po povolení replikace. 




## <a name="next-steps"></a>Další kroky

Přejděte na [Krok 3: Plánování kapacity](vmm-to-azure-walkthrough-capacity.md)

