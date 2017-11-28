---
title: "Skupiny virtuálních počítačů s VMware vCenter označování | Microsoft Docs"
description: "Popisuje postup vytvoření skupiny před spuštěním posouzení se službou Azure migrovat."
services: migration-planner
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 5c279804-aa30-4946-a222-6b77c7aac508
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 11/21/2017
ms.author: raynew
ms.openlocfilehash: db33e31cdef143aa70809442457e447446a1681a
ms.sourcegitcommit: 5bced5b36f6172a3c20dbfdf311b1ad38de6176a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/27/2017
---
# <a name="group-vms-with-vcenter-tagging"></a>Virtuální počítače skupiny s vCenter označování

Tento článek popisuje, jak vytvořit skupiny počítačů pro [Azure migrovat](migrate-overview.md) assessment pomocí označování do systému VMware vCenter. Zadáte značky kategorii, kterou chcete použít, když vytvoříte skupinu. 

## <a name="set-up-tagging"></a>Nastavit označování

Během nasazování Azure migrovat místní migraci virtuálního počítače Azure zjišťuje počítače, které běží na hostitelích ESXi spravuje vCenter server. Budete muset nastavit vCenter označování před proces zjišťování.

1. V VMware vSphere webovému klientovi přejděte na vCenter server.
2. Klikněte na tlačítko **značky**, chcete-li zkontrolovat všechny aktuální značky.
3. K označení virtuálního počítače, vyberte **související objekty** > **virtuální počítače**a potom vyberte virtuální počítač, který chcete značky.
4. V **Souhrn** > **značky**, klikněte na tlačítko **přiřadit**. 
5. Klikněte na tlačítko **novou značku**a zadejte název značky a popis.
6. Chcete-li crate kategorii pro značku, vyberte **novou kategorii** v rozevíracím seznamu.
7. Zadejte název kategorie a popis a nastavení kardinality. Pak klikněte na **OK**.

    ![Virtuální počítač značky](./media/how-to-tag-v-center/vm-tag.png)

## <a name="use-tagging-to-create-groups"></a>Pomocí označování můžete vytvořit skupiny

1. Nastavení zjišťování místní počítače, jak je popsáno v [kurzu assessment VMware](tutorial-assessment-vmware.md#run-the-collector-to-discover-vms).
2. V **značky kategorii pro seskupení**, vyberte kategorii vCenter značky, na kterém by měla být založena skupině hodnocení. Migrace Azure automaticky vytvoří skupinu pro vybranou kategorii.

    

## <a name="next-steps"></a>Další kroky

[Nastavit hodnocení virtuálního počítače VMware](tutorial-assessment-vmware.md).
