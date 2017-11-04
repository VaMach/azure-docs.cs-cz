---
title: "Azure soubory škálovatelnosti a cílech výkonnosti | Microsoft Docs"
description: "Další informace o škálovatelnosti a výkonu cíle pro soubory, Azure, včetně kapacitu, rychlost požadavků a omezení šířky pásma pro příchozí a odchozí."
services: storage
documentationcenter: na
author: wmgries
manager: klaasl
editor: tamram
ms.service: storage
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage
ms.date: 10/17/2017
ms.author: wgries
ms.openlocfilehash: 35d430b683224d1035cccdfb58b9db415d47ea3b
ms.sourcegitcommit: b723436807176e17e54f226fe00e7e977aba36d5
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/19/2017
---
# <a name="azure-files-scalability-and-performance-targets"></a>Azure soubory škálovatelnosti a cílech výkonnosti
[Soubory Azure](storage-files-introduction.md) nabízí plně spravované sdílené složky v cloudu, které jsou přístupné přes průmyslový standard protokolu SMB. Tento článek popisuje cíle škálovatelnost a výkon pro soubory Azure a synchronizace souborů Azure (Preview).

Škálovatelnost a výkon cíle, které jsou zde uvedeny jsou vyšší kategorie cíle, ale může mít vliv jiné proměnné ve vašem nasazení. Například propustnost pro soubor může také být omezen vaši dostupnou šířku pásma sítě, nikoli pouze servery, které hostují službu Azure Files. Důrazně doporučujeme testování vaší vzor používání a zjistit, zda škálovatelnost a výkon Azure Files splňují vaše požadavky. Můžeme také potvrdí na zvýšení těchto mezních hodnot v čase. Prosím neváhejte a sdělte nám svůj názor, buď v komentářích menší než nebo na [Azure soubory UserVoice](https://feedback.azure.com/forums/217298-storage/category/180670-files), o které omezení chcete nás zvýšit.

## <a name="azure-storage-account-scale-targets"></a>Cíle škálování účtu úložiště Azure
Nadřazený prostředek pro sdílení souborů Azure je účet úložiště Azure. Účet úložiště představuje fond úložiště v Azure, které je možné k ukládání dat pomocí více služby úložiště, včetně souborů Azure. Další služby, které ukládají data v účtech úložiště jsou Azure Blob storage, Azure Queue storage a Azure Table storage. Následující cíle použít všechny služby úložiště, ukládání dat v účtu úložiště:

[!INCLUDE [azure-storage-limits](../../../includes/azure-storage-limits.md)]

[!INCLUDE [azure-storage-limits-azure-resource-manager](../../../includes/azure-storage-limits-azure-resource-manager.md)]

> [!Important]  
> Využití účet úložiště z jiných služeb úložiště ovlivní vaše sdílené složky Azure File ve vašem účtu úložiště. Například Pokud dostanete maximální úložnou kapacitu účet s úložištěm Azure Blob, nebudete moci vytvořit nové soubory na Azure File ve sdílené složce, i když vaši sdílenou složku Azure File je menší než maximální sdílená.

## <a name="azure-files-scale-targets"></a>Azure soubory měřítka cíle
[!INCLUDE [storage-files-scale-targets](../../../includes/storage-files-scale-targets.md)]

## <a name="azure-file-sync-scale-targets"></a>Azure měřítka cíle synchronizace souboru
S Azure souboru Sync mají Pokusili jsme co nejvíce návrh pro neomezenou využití, ale to není možné. Níže uvedená tabulka určuje hranice našich testech a cíle, které jsou ve skutečnosti pevných limitů:

[!INCLUDE [storage-sync-files-scale-targets](../../../includes/storage-sync-files-scale-targets.md)]

## <a name="see-also"></a>Viz také
- [Plánování nasazení Azure Files](storage-files-planning.md)
- [Plánování nasazení Azure souboru Sync](storage-sync-files-planning.md)
- [Škálovatelnosti a cílech výkonnosti pro jiné služby úložiště](../common/storage-scalability-targets.md)