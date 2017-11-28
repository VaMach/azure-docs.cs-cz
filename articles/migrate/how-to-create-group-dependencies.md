---
title: "Upřesnit skupinu assessment pomocí mapování skupiny závislostí v Azure migrovat | Microsoft Docs"
description: "Popisuje, jak k zpřesnění posouzení pomocí mapování závislostí skupin ve službě Azure migrovat."
services: migrate
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 0527e34e-a078-405e-aeb9-c91a5808112a
ms.service: migrate
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 11/21/2017
ms.author: raynew
ms.openlocfilehash: b4d6861f147fbb6e65a9d529f17f78b54075eb90
ms.sourcegitcommit: 5bced5b36f6172a3c20dbfdf311b1ad38de6176a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/27/2017
---
# <a name="refine-a-group-using-group-dependency-mapping"></a>Upřesnit skupiny pomocí mapování skupin závislostí

Tento článek popisuje, jak nastavit mapování závislostí skupiny pro [Azure migrovat](migrate-overview.md) hodnocení. Obvykle použijete tuto metodu, pokud chcete Upřesnit nastavení pro existující skupiny, kontrola mezi skupiny závislostmi, před spuštěním posouzení. Skupiny, pro které chcete spustit mapování závislostí skupin nesmí obsahovat více než 10 počítačů.  

## <a name="modify-a-group"></a>Úprava skupiny

1. Ve službě Azure migrovat projektu, v části **spravovat**, klikněte na tlačítko **skupiny**a vyberte skupinu.
2. Na stránce skupiny klikněte na **zobrazení závislostí**, chcete-li otevřít mapu závislostí skupiny. 

     ![Zobrazení skupiny](./media/how-to-create-group-dependencies/create-group.png)

3. Chcete-li zobrazit podrobnější závislosti, klikněte na tlačítko časový rozsah a upravit ho. Ve výchozím nastavení rozsahu představuje jednu hodinu. Můžete upravit časové rozmezí, nebo zadejte počáteční a koncová data a doby trvání.
4. Pomocí kombinace kláves Ctrl + kliknutí vyberte počítače, na mapě. Přidat nebo odebrat počítače z mapování.
    - Můžete přidat pouze počítače, které byly zjištěny.
    - Přidávání a odebírání počítače ze skupiny zruší platnost po vyhodnocení pro ni.
    - Volitelně můžete vytvořit nové vyhodnocení po úpravě skupiny.
5. Klikněte na tlačítko **OK** uložte skupinu.

    ![Přidání a odebrání](./media/how-to-create-group-dependencies/add-remove.png)

Pokud chcete zkontrolovat závislosti konkrétní počítač, který se zobrazí v mapa závislostí skupiny [nastavit mapování závislostí počítač](how-to-create-group-machine-dependencies.md).


## <a name="next-steps"></a>Další kroky

[Další informace](concepts-assessment-calculation.md) o tom, jak jsou vypočítávány vyhodnocování.
