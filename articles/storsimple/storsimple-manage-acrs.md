---
title: "Spravovat záznamy řízení přístupu v zařízení StorSimple | Microsoft Docs"
description: "Popisuje, jak používat záznamy řízení přístupu (ACRs) k určení, které hostitele může připojit k svazek v zařízení StorSimple."
services: storsimple
documentationcenter: 
author: alkohli
manager: carmonm
editor: 
ms.assetid: 2f1475d8-36a5-4cc4-84b9-adf8a310b60c
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/03/2017
ms.author: alkohli
ms.openlocfilehash: 320569e302d145d695f3be9684e97786fa82e80b
ms.sourcegitcommit: 0930aabc3ede63240f60c2c61baa88ac6576c508
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/07/2017
---
# <a name="use-the-storsimple-manager-service-to-manage-access-control-records"></a>Použít službu StorSimple Manager ke správě záznamy řízení přístupu
> [!NOTE]
> Portálu classic pro StorSimple je zastaralý. Váš správce zařízení StorSimple se automaticky přesune na nový portál Azure podle plánu vyřazení. Obdržíte e-mailu a portálu oznámení pro přesunutí. Tento dokument také vyřadí brzy k dispozici. Pokud chcete zobrazit verzi tohoto článku na nový portál Azure, přejděte na [použít službu StorSimple Manager ke správě záznamy řízení přístupu](storsimple-8000-manage-acrs.md). Dotazy týkající se přechodu, najdete v části [– nejčastější dotazy: přesunout do portálu Azure](storsimple-8000-move-azure-portal-faq.md).

## <a name="overview"></a>Přehled
Záznamy řízení přístupu (ACRs) umožňují určit, které hostitele může připojit k svazek v zařízení StorSimple. ACRs jsou nastaveny na konkrétním svazku a obsahovat kvalifikované názvy iSCSI (IQN) hostitele. Když hostitel pokusí o připojení ke svazku, zařízení se kontroluje ACR přidružený tento svazek pro název IQN a pokud je nalezena shoda, pak připojení. Řízení přístupu na zaznamenává části **konfigurace** stránka zobrazuje všechny záznamy řízení přístupu s odpovídající IQN hostitelů.

Tento kurz vysvětluje následující běžné úlohy související s ACR:

* Přidání záznamu o řízení přístupu 
* Upravit záznam řízení přístupu 
* Odstranit záznam řízení přístupu 

> [!IMPORTANT]
> * Při přiřazování ACR svazku, vezměte v potaz, že svazek není přístup souběžně více než jednomu hostiteli neclusterované protože to může způsobit poškození svazku. 
> * Při odstraňování ACR ze svazku, ujistěte se, že odpovídající hostitele není přístup k svazku vzhledem k tomu, že odstranění může mít za následek přerušení pro čtení a zápis.
> 
> 

## <a name="add-an-access-control-record"></a>Přidání záznamu o řízení přístupu
Použít službu StorSimple Manager **konfigurace** stránku přidáte ACRs. Obvykle přidružíte jednu ACR jeden svazek.

Proveďte následující postup pro přidání ACR.

#### <a name="to-add-an-access-control-record"></a>Chcete-li přidat záznam řízení přístupu
1. Na stránce cílové služby, vyberte svoji službu, dvakrát klikněte na název služby a klikněte **konfigurace** kartě.
2. V tabulkovém seznamu pod **záznamy řízení přístupu**, dodávky **název** acr.
3. Zadejte název IQN hostitele s Windows v rámci **název iniciátoru iSCSI**. Získání názvu IQN hostitele vašeho systému Windows Server, postupujte takto:
   
   * Na hostiteli s Windows spusťte iniciátor iSCSI od Microsoftu.
   * V okně **iSCSI Initiator Properties** (Vlastnosti iniciátoru iSCSI) na kartě **Konfigurace** vyberte a zkopírujte řetězec z pole **Název iniciátoru**.
   * Vložte tento řetězec v **název iniciátoru iSCSI** pole v tabulce ACRs na portálu Azure classic.
4. Klikněte na tlačítko **Uložit** uložte nově vytvořený ACR. Tabulkové výpis budou aktualizovány tak, aby odrážela přidání.

## <a name="edit-an-access-control-record"></a>Upravit záznam řízení přístupu
Můžete použít **konfigurace** na portálu Azure classic upravit ACRs. 

> [!NOTE]
> Můžete upravit pouze ACRs, které nejsou aktuálně používá. Chcete-li upravit ACR přidružené k svazku, který je aktuálně používán, musíte nejdřív udělat svazek offline.
> 
> 

Proveďte následující kroky, chcete-li upravit ACR.

#### <a name="to-edit-an-access-control-record"></a>Chcete-li upravit záznam řízení přístupu
1. Na stránce cílové služby, vyberte svoji službu, dvakrát klikněte na název služby a klikněte **konfigurace** kartě.
2. V tabulkovém seznam záznamy řízení přístupu, pozastavte ukazatel myši nad ACR, který chcete upravit.
3. Zadejte nový název nebo název IQN ACR.
4. Klikněte na tlačítko **Uložit** uložit upravené ACR. Tabulkové výpis budou aktualizovány tak, aby odrážela tuto změnu.

## <a name="delete-an-access-control-record"></a>Odstranit záznam řízení přístupu
Můžete použít **konfigurace** na portálu Azure classic odstranit ACRs. 

> [!NOTE]
> Lze odstranit pouze ACRs, které nejsou aktuálně používá. Pokud chcete odstranit ACR přidružené k svazku, který je aktuálně používán, musíte nejdřív udělat svazek offline.
> 
> 

Proveďte následující kroky a odstraňte záznam řízení přístupu.

#### <a name="to-delete-an-access-control-record"></a>Odstranit záznam řízení přístupu
1. Na stránce cílové služby, vyberte svoji službu, dvakrát klikněte na název služby a klikněte **konfigurace** kartě.
2. V tabulkovém seznam záznamy řízení přístupu (ACRs), přejděte myší ACR, který chcete odstranit.
3. Ikony odstranění (**x**) se zobrazí v pravém sloupci extrémně pro ACR, kterou jste vybrali. Klikněte **x** ikonu Odstranit ACR.
4. Po zobrazení výzvy k potvrzení, klikněte na tlačítko **Ano** Chcete-li pokračovat v odstraňování. Tabulkové výpis budou aktualizovány tak, aby odrážela odstranění.

## <a name="next-steps"></a>Další kroky
* Další informace o [správu svazků zařízení StorSimple](storsimple-manage-volumes.md).
* Další informace o [pomocí služby StorSimple Manager ke správě zařízení StorSimple](storsimple-manager-service-administration.md).

