---
title: "Zobrazit a spravovat úlohy StorSimple | Microsoft Docs"
description: "Popisuje stránku úlohy služby StorSimple Manager a způsobu jeho použití ke sledování poslední, aktuální a naplánované úlohy zálohování."
services: storsimple
documentationcenter: NA
author: alkohli
manager: carmonm
editor: 
ms.assetid: cdd3e9e8-7ccd-40a3-8c07-0ab1338c0e59
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 11/03/2017
ms.author: alkohli
ms.openlocfilehash: 44b3550a2457a9f43a54117c6164858a0ef6f095
ms.sourcegitcommit: 0930aabc3ede63240f60c2c61baa88ac6576c508
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/07/2017
---
# <a name="use-the-storsimple-manager-service-to-view-and-manage-storsimple-jobs-update-2"></a>Pomocí služby StorSimple Manager můžete zobrazit a spravovat úlohy StorSimple (Update 2)
> [!NOTE]
> Portálu classic pro StorSimple je zastaralý. Váš správce zařízení StorSimple se automaticky přesune na nový portál Azure podle plánu vyřazení. Obdržíte e-mailu a portálu oznámení pro přesunutí. Tento dokument také vyřadí brzy k dispozici. Pokud chcete zobrazit verzi tohoto článku na nový portál Azure, přejděte na [pomocí služby StorSimple Manager můžete zobrazit a spravovat úlohy StorSimple (Update 2)](storsimple-8000-manage-jobs-u2.md). Dotazy týkající se přechodu, najdete v části [– nejčastější dotazy: přesunout do portálu Azure](storsimple-8000-move-azure-portal-faq.md).

[!INCLUDE [storsimple-version-selector-manage-jobs](../../includes/storsimple-version-selector-manage-jobs.md)]

## <a name="overview"></a>Přehled
**Úlohy** stránka poskytuje jednu centrální portál pro zobrazení a Správa úloh, které byly zahájeny v zařízeních připojení ke službě StorSimple Manager. Můžete zobrazit naplánované, spuštěná, dokončené, zrušené a k selhání úlohy pro více zařízení. Výsledky se zobrazí v tabulkovém formátu. 

![Stránka úlohy](./media/storsimple-manage-jobs-u2/jobs.png)

Můžete rychle najít úlohy, které zajímá pomocí filtrování na pole, jako například:

* **Stav** – úloh může být spuštěn, dokončené, zrušené, se nezdařila, ruší nebo dokončena s chybami.
* **Od a do** – úlohy je možné filtrovat podle rozsahu datum a čas.
* **Typ** – typ úlohy může být zálohování, ruční zálohy, obnovení, klonování, zařízení převzetí služeb při selhání, vytváření místně vázaný svazek, upravit svazek, aktualizovat, podporují balíček nebo zřizování virtuální zařízení.
* **Zařízení** – úloh se spouští na určité zařízení připojena k službě.
  Filtrované úlohy jsou pak v tabulce na základě následující atributy:
  
  * **Typ** – zálohování, ruční zálohy, obnovení, klonování, zařízení převzetí služeb při selhání, vytváření místně vázaný svazek, upravit svazek, aktualizovat, podporují balíček nebo zřizování virtuální zařízení.
  * **Stav** – spuštěný, dokončené, zrušené, se nezdařila, ruší nebo dokončena s chybami.
  * **Entity** – úloh může být přidružen svazku, zásady zálohování nebo zařízení. Například úlohu klonování je přidružen svazku, zatímco je přidružený k zásadě zálohování naplánované úlohy zálohování. V důsledku operaci obnovení nebo zotavení po havárii (DR), se vytvoří úloha zařízení.
  * **Zařízení** – název zařízení, na kterém byla úloha spuštěna.
  * **Bylo zahájeno** – čas zahájení úlohy.
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
   
    ![Stránka Podrobnosti úlohy](./media/storsimple-manage-jobs-u2/JobDetails.png)

## <a name="cancel-a-job"></a>Zrušení úlohy
Proveďte následující kroky, chcete-li zrušit probíhající úloze.

> [!NOTE]
> Některé úlohy, jako jsou úpravy svazek, chcete-li změnit typ svazku nebo rozšíření svazku, nelze zrušit.
> 
> 

### <a name="to-cancel-a-job"></a>Zrušení úlohy
1. Na **úlohy** stránky, zobrazit spuštěné úlohy, kterou chcete zrušit spuštěním dotazu s odpovídající filtry.
2. Vyberte úlohu.
3. V dolní části stránky klikněte na tlačítko **zrušit**.
4. Po zobrazení výzvy k potvrzení klikněte na **Ano**.

Tato úloha je nyní zrušena.

## <a name="next-steps"></a>Další kroky
* Zjistěte, jak [spravovat vaše zásady zálohování StorSimple](storsimple-manage-backup-policies.md).
* Zjistěte, jak [použít službu StorSimple Manager ke správě zařízení StorSimple](storsimple-manager-service-administration.md).

