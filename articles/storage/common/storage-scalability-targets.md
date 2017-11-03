---
title: "Škálovatelnost a cíle výkonnosti Azure Storage | Microsoft Docs"
description: "Další informace o škálovatelnosti a výkonu cíle pro Azure Storage, včetně kapacitu, rychlost požadavků a příchozí a odchozí šířky pásma pro oba účty úložiště standard a premium. Pochopení cíle výkonnosti pro oddíly v rámci každé ze služby Azure Storage."
services: storage
documentationcenter: na
author: tamram
manager: timlt
editor: tysonn
ms.assetid: be721bd3-159f-40a1-88c1-96418537fe75
ms.service: storage
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage
ms.date: 07/12/2017
ms.author: tamram
ms.openlocfilehash: 805b0eee46846345ee1f33faf0c28393c3e8ebb1
ms.sourcegitcommit: d41d9049625a7c9fc186ef721b8df4feeb28215f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/02/2017
---
# <a name="azure-storage-scalability-and-performance-targets"></a>Škálovatelnost a výkonnostní cíle Azure Storage
## <a name="overview"></a>Přehled
Toto téma popisuje témata škálovatelnost a výkon pro Microsoft Azure Storage. Souhrn další omezení Azure, najdete v části [předplatné Azure a omezení služby, kvóty a omezení](../../azure-subscription-service-limits.md).

> [!NOTE]
> Všechny účty úložiště spustit na nové ploché síťové topologie a podporovat škálovatelnost a výkon cíle, které jsou uvedené níže, bez ohledu na to, kdy byly vytvořeny. Další informace o architektuře ploché sítě Azure Storage a o škálovatelnost, najdete v části [Microsoft Azure Storage: A vysoce dostupné cloudového úložiště se silnou konzistencí](http://blogs.msdn.com/b/windowsazurestorage/archive/2011/11/20/windows-azure-storage-a-highly-available-cloud-storage-service-with-strong-consistency.aspx).
> 
> [!IMPORTANT]
> Škálovatelnost a výkon cíle, které jsou zde uvedeny vyšší kategorie cíle, ale jsou dosažitelné. Ve všech případech, rychlost požadavků a šířku pásma vašeho úložiště účet závisí na velikosti objekty uložené, vzory přístupu k použity, a typ úlohy aplikace provede. Ujistěte se, že testovací služby k určení, zda jeho výkon vyhovovat vašim požadavkům. Pokud je to možné vyhnout nečekané špičky v míru přenosu dat a zajistěte, aby byl provoz dobře distribuované napříč oddíly.
> 
> Když aplikace dosáhne limitu co dokáže zpracovat oddíl pro úlohy, začnou Azure Storage, vrátí kód chyby 503 (zaneprázdněný Server) nebo kód chyby 500 odpovědí (časový limit operace). V takovém případě by aplikace měla použít zásadu exponenciálního omezení rychlosti pro opakování. Exponenciálního omezení rychlosti umožňuje zatížení v oddílu snížit a doběhu špičky na provoz oddílu.
> 
> 

Pokud potřebám vaší aplikace přesahují cíle škálovatelnosti účtu jedno úložiště, můžete sestavit aplikace k používání více účtů úložiště a oddílu datové objekty mezi tyto účty úložiště. V tématu [Azure Storage – ceny](https://azure.microsoft.com/pricing/details/storage/) informace o cenách.

## <a name="scalability-targets-for-a-storage-account"></a>Cíle škálovatelnosti účtu úložiště
[!INCLUDE [azure-storage-limits](../../../includes/azure-storage-limits.md)]

[!INCLUDE [azure-storage-limits-azure-resource-manager](../../../includes/azure-storage-limits-azure-resource-manager.md)]

## <a name="azure-blob-storage-scale-targets"></a>Azure Blob storage měřítka cíle
[!INCLUDE [storage-blob-scale-targets](../../../includes/storage-blob-scale-targets.md)]

## <a name="azure-files-scale-targets"></a>Azure soubory měřítka cíle
Další informace o škálování a výkon cíle pro soubory Azure a synchronizace souborů Azure, najdete v tématu [Azure Files škálovatelnosti a cílech výkonnosti](../files/storage-files-scale-targets.md).

[!INCLUDE [storage-files-scale-targets](../../../includes/storage-files-scale-targets.md)]

### <a name="azure-file-sync-scale-targets"></a>Azure měřítka cíle synchronizace souboru
[!INCLUDE [storage-sync-files-scale-targets](../../../includes/storage-sync-files-scale-targets.md)]

## <a name="azure-queue-storage-scale-targets"></a>Azure fronty úložiště měřítka cíle
[!INCLUDE [storage-queues-scale-targets](../../../includes/storage-queues-scale-targets.md)]

## <a name="azure-table-storage-scale-targets"></a>Azure Table úložiště měřítka cíle
[!INCLUDE [storage-table-scale-targets](../../../includes/storage-tables-scale-targets.md)]

<!-- conceptual info about disk limits -- applies to unmanaged and managed -->
## <a name="scalability-targets-for-virtual-machine-disks"></a>Cíle škálovatelnosti pro disky virtuálního počítače
[!INCLUDE [azure-storage-limits-vm-disks](../../../includes/azure-storage-limits-vm-disks.md)]

V tématu [velikosti virtuálních počítačů Windows](../../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) nebo [velikosti virtuálního počítače s Linuxem](../../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) další podrobnosti.

## <a name="managed-virtual-machine-disks"></a>Disky spravovaných virtuálních počítačů

[!INCLUDE [azure-storage-limits-vm-disks-managed](../../../includes/azure-storage-limits-vm-disks-managed.md)]

## <a name="unmanaged-virtual-machine-disks"></a>Disky nespravované virtuálního počítače
[!INCLUDE [azure-storage-limits-vm-disks-standard](../../../includes/azure-storage-limits-vm-disks-standard.md)]

[!INCLUDE [azure-storage-limits-vm-disks-premium](../../../includes/azure-storage-limits-vm-disks-premium.md)]

## <a name="see-also"></a>Viz také
* [Podrobnosti o cenách úložiště](https://azure.microsoft.com/pricing/details/storage/)
* [Předplatné Azure a omezení služby, kvóty a omezení](../../azure-subscription-service-limits.md)
* [Storage úrovně Premium: Vysoce výkonné úložiště pro úlohy virtuálních počítačů Azure](../../virtual-machines/windows/premium-storage.md)
* [Replikace Azure Storage](../storage-redundancy.md)
* [Microsoft Azure Storage výkon a škálovatelnost kontrolní seznam](../storage-performance-checklist.md)
* [Microsoft Azure Storage: Vysoce dostupný cloudové úložiště služby se silnou konzistenci](http://blogs.msdn.com/b/windowsazurestorage/archive/2011/11/20/windows-azure-storage-a-highly-available-cloud-storage-service-with-strong-consistency.aspx)

