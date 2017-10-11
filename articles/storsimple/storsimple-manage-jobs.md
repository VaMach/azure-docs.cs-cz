---
title: "Zobrazit a spravovat úlohy StorSimple | Microsoft Docs"
description: "Popisuje stránku úlohy služby StorSimple Manager a způsobu jeho použití ke sledování poslední, aktuální a naplánované úlohy zálohování."
services: storsimple
documentationcenter: NA
author: alkohli
manager: carmonm
editor: 
ms.assetid: 55922cd0-d490-48eb-938a-012a67c1c09e
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 08/17/2016
ms.author: alkohli
ms.openlocfilehash: 7d9377bb8f3cb8c587823c2d71d61dfb9b50f52f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="use-the-storsimple-manager-service-to-view-and-manage-storsimple-jobs"></a>Pomocí služby StorSimple Manager můžete zobrazit a spravovat úlohy StorSimple
[!INCLUDE [storsimple-version-selector-manage-jobs](../../includes/storsimple-version-selector-manage-jobs.md)]

## <a name="overview"></a>Přehled
**Úlohy** stránka poskytuje jednu centrální portál pro zobrazení a Správa úloh, které byly zahájeny v zařízeních připojení ke službě StorSimple Manager. Můžete zobrazit naplánované, spuštěná, dokončené a k selhání úlohy pro více zařízení. Výsledky se zobrazí v tabulkovém formátu. 

![Stránka úlohy](./media/storsimple-manage-jobs/HCS_JobsPage.png)

Můžete rychle najít úlohy, které zajímá pomocí filtrování na pole, jako například:

* **Stav** – můžete spuštěny úlohy naplánované, se nezdařila, dokončené, ruší nebo zrušené.
* **Typ** – úloh lze vytvořit v důsledku zálohu na vyžádání nebo naplánované (**provést zálohování**), klonování, obnovení zařízení nebo operaci aktualizace.
* **Zařízení** – úloh se spouští na určité zařízení připojena k službě.
* **Od a do** – úlohy je možné filtrovat podle rozsahu datum a čas.

Filtrované úlohy jsou pak v tabulce na základě následující atributy:

* **Typ** – zálohování, obnovení, klonování převzetí služeb při selhání nebo aktualizace.
* **Stav** – systémem naplánované, se nezdařila, byla dokončena, zrušení nebo došlo ke zrušení.
* **Entity** – úloh může být přidružen svazku, zásady zálohování nebo zařízení. Úloha klonování je přidružen svazku, zatímco je přidružený k zásadě zálohování naplánované úlohy zálohování. V důsledku operaci obnovení nebo zotavení po havárii (DR), se vytvoří úloha zařízení.
* **Zařízení** – název zařízení, na kterém byla úloha spuštěna.
* **Spuštění na** – čas zahájení úlohy.
* **Průběh** – procento dokončení spuštěné úlohy. Dokončené úlohy to by měl být vždy 100 %.

Seznam úloh se aktualizují každých 30 sekund.

Na této stránce můžete provádět následující úlohy související akce:

* Zobrazení podrobností o úloze
* Zrušení úlohy

## <a name="view-job-details"></a>Zobrazení podrobností o úloze
Proveďte následující kroky, chcete-li zobrazit podrobnosti libovolné úlohy.

#### <a name="to-view-job-details"></a>Chcete-li zobrazit podrobnosti úlohy
1. Na **úlohy** stránky, zobrazení úloh se zajímá spuštěním dotazu s odpovídající filtry. Pro dokončení může hledat spuštěný, nebo došlo ke zrušení úlohy.
2. Vyberte úlohu.
3. V dolní části stránky klikněte na tlačítko **podrobnosti**.
4. V **podrobnosti úlohy zálohování** dialogové okno, můžete zobrazit stav, podrobnosti, čas statistiky a data statistiky.

## <a name="cancel-a-job"></a>Zrušení úlohy
Proveďte následující kroky, chcete-li zrušit probíhající úloze.

### <a name="to-cancel-a-job"></a>Zrušení úlohy
1. Na **úlohy** stránky, zobrazit spuštěné úlohy, kterou chcete zrušit spuštěním dotazu s odpovídající filtry.
2. Vyberte úlohu.
3. V dolní části stránky klikněte na tlačítko **zrušit**.
4. Po zobrazení výzvy k potvrzení klikněte na **Ano**.

Tato úloha je nyní zrušena.

## <a name="next-steps"></a>Další kroky
* Zjistěte, jak [spravovat vaše zásady zálohování StorSimple](storsimple-manage-backup-policies.md).
* Zjistěte, jak [použít službu StorSimple Manager ke správě zařízení StorSimple](storsimple-manager-service-administration.md).

