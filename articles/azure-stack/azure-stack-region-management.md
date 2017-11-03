---
title: "Oblast správy v zásobníku Azure | Microsoft Docs"
description: "Přehled správy oblast v zásobníku Azure."
services: azure-stack
documentationcenter: 
author: efemmano
manager: dsavage
editor: 
ms.assetid: e94775d5-d473-4c03-9f4e-ae2eada67c6c
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/25/2017
ms.author: efemmano
ms.openlocfilehash: d1310f0cb9a820366ab8712a782785e955a24134
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="region-management-in-azure-stack"></a>Oblast správy v Azure zásobníku

*Platí pro: Azure zásobníku integrované systémy a Azure zásobníku Development Kit*

Sada Azure má koncept oblastí, které se skládá z hardwarové prostředky, které tvoří infrastruktury Azure zásobníku logických entit. V oblasti správy můžete najít všechny prostředky, které jsou nutné pro úspěšný provoz životního cyklu infrastruktury Azure zásobníku.

Jedna integrovaná nasazení systému (označované jako *zásobník Azure cloud*) tvoří jedné oblasti. Každý Azure zásobníku Development Kit má jedné oblasti, s názvem **místní**. Pokud nasadíte druhý zásobník Azure integrovaný systém nebo jinou instanci development kit na samostatných nastavíte, je tento cloud Azure zásobníku v jiné oblasti.

## <a name="information-available-through-the-region-management-tile"></a>Informace, které jsou k dispozici prostřednictvím dlaždici správy oblast
Obsahuje sadu oblast možnosti správy, které jsou dostupné v Azure zásobníku **oblast správy** dlaždici. Tato dlaždice není k dispozici pro operátor zásobník Azure na výchozí řídicí panel portálu správce. Prostřednictvím této dlaždice můžete monitorovat a aktualizovat vaši oblast Azure zásobníku a jeho komponenty, které jsou specifické pro oblast.

 ![Na dlaždici správy oblast](media/azure-stack-manage-region/image1.png)

 Pokud kliknete na oblast na dlaždici správy oblast, se můžete dostat následující informace:

  ![Popis podokna v okně správy oblast](media/azure-stack-manage-region/image2.png)

1. **V nabídce prostředků**. Zde můžete přístup k oblasti správy konkrétní infrastruktury a zobrazit a spravovat prostředky uživatele, například účty úložiště a virtuální sítě.

2. **Výstrahy**. Tuto dlaždici uvádí systémové výstrahy a poskytuje podrobné informace o každé z těchto výstrah.

3. **Aktualizace**. Na této dlaždici můžete zobrazit aktuální verzi vaší infrastruktury Azure zásobníku.

4. **Zprostředkovatelé prostředků**. Zprostředkovatelé prostředků je místo pro správu klienta funkce nabízené komponenty potřebné ke spuštění zásobník Azure. Každý poskytovatel prostředků se dodává s správu prostředí. Toto prostředí může zahrnovat výstrahy pro konkrétního poskytovatele, metriky a dalším funkcím správy specifické pro poskytovatele prostředků.
 
5. **Role infrastruktury**. Role infrastruktury jsou komponenty potřebné ke spuštění zásobník Azure. Jsou uvedeny pouze role infrastruktury, které sestavy výstrahy. Kliknutím na roli, můžete zobrazit výstrahy spojené s konkrétní roli a instance rolí, kde je spuštěna tato role. I když je možnost spustit, restartovat, nebo vypnout instanci role infrastruktury, proveďte **není** to provést ve vývojovém prostředí sady. Tyto možnosti jsou určeny pouze pro prostředí s několika uzly, kde existuje více než jedna instance role na role infrastruktury. Restartování instance role (zejména AzS-Xrp01) v sadě pro vývoj způsobí, že nestabilitě systému.

## <a name="next-steps"></a>Další kroky
[Sledování stavu a výstrahy v Azure zásobníku](azure-stack-monitor-health.md)

[Správa aktualizací v Azure zásobníku](azure-stack-updates.md)






