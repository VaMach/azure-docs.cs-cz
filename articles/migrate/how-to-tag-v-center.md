---
title: "Skupiny virtuálních počítačů s VMware vCenter označování | Microsoft Docs"
description: "Popisuje postup vytvoření skupiny před spuštěním posouzení se službou Azure migrovat."
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: article
ms.date: 12/12/2017
ms.author: raynew
ms.openlocfilehash: 967c2a1fc0e5144c6c7d5c8c4a81cec3d77ffdb5
ms.sourcegitcommit: aaba209b9cea87cb983e6f498e7a820616a77471
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/12/2017
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
