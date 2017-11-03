---
title: "Spravovat zásady zálohování pro vaše zařízení StorSimple | Microsoft Docs"
description: "Vysvětluje, jak je možné použít službu StorSimple Manager k vytváření a správě ručního zálohování, plánů zálohování a uchovávání záloh."
services: storsimple
documentationcenter: NA
author: SharS
manager: carmonm
editor: 
ms.assetid: 4a2db707-bbfc-425c-bfeb-bc5c97781873
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 05/10/2016
ms.author: v-sharos
ms.openlocfilehash: 5448247428ab96887470c6b53f7a9b3dcd9238f0
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="use-the-storsimple-manager-service-to-manage-backup-policies-update-2"></a>Použít službu StorSimple Manager ke správě zásady zálohování (Update 2)
[!INCLUDE [storsimple-version-selector-manage-backup-policies](../../includes/storsimple-version-selector-manage-backup-policies.md)]

## <a name="overview"></a>Přehled
Tento kurz vysvětluje, jak používat službu StorSimple Manager **zásady zálohování** stránky k řízení procesů zálohování a uchovávání záloh pro formátujte svazky zařízení StorSimple. Také popisuje, jak provést ruční zálohy.

Při zálohování svazku, můžete vytvořit místní snímek nebo cloudový snímek. Pokud zálohujete místně vázaný svazek, doporučujeme vám, že zadáváte cloudový snímek. Pořízení velký počet místní snímky místně vázaný svazek kombinaci s datovou sadou, která obsahuje mnoho změn způsobí v situaci, ve kterém můžete rychle spustit nedostatek místa na místní. Pokud se rozhodnete převést místní snímky, doporučujeme provést méně snímky denní zálohování nejnovější stav, je zachování za den a pak odstraňte je.

Pokud pořídíte snímek cloudu místně vázaný svazek, zkopírovat pouze změněné data do cloudu, kde je s odstraněním duplicitních dat a komprimované. 

## <a name="the-backup-policies-page"></a>Stránka zásady zálohování
**Zásady zálohování** stránka umožňuje spravovat zásady zálohování a plánování místních a cloudových snímků. (Zásady zálohování se používají ke konfiguraci plánů zálohování a uchovávání záloh pro kolekci svazků). Zásady zálohování umožňují pořízení snímku více svazků najednou. To znamená, že zálohy vytvořené zásady zálohování bude kopie konzistentní při selhání. **Zásady zálohování** stránce zobrazí zásady zálohování, jejich typy, přidružené svazky, počet záloh uchována a možnost povolit tyto zásady.

**Zásady zálohování** stránce můžete také filtrovat existující zásady zálohování jednu nebo více z následujících polí:

* **Název zásady** – název přidružených k zásadě. Různé typy zásady patří:
  
  * Naplánované zásady, které jsou explicitně vytvořený uživatelem.
  * Automatické zásady, které vytvářejí, když výchozí zálohování pro tuto možnost svazek byl povolen v době vytváření svazků. Tyto zásady jsou pojmenované jako *VolumeName*_výchozí kde *VolumeName* odkazuje na název svazku zařízení StorSimple nakonfigurovaná uživatelem na portálu Azure classic. Výsledkem automatické zásad denní cloudových snímků od okamžiku zařízení 22:30.
  * Importované zásady, které byly původně vytvořil v Snapshot Manager zařízení StorSimple. Tyto mají značku, která popisuje StorSimple Snapshot Manager hostitele, který zásady, které byly naimportovány z.
* **Svazky** – svazky přidružených k zásadě. Všechny svazky, které jsou přidružené k zásadě zálohování jsou seskupeny dohromady při vytvoření zálohy.
* **Poslední úspěšná zálohy** – datum a čas poslední úspěšné zálohy pořízené touto zásadou.
* **Další zálohování** – datum a čas další naplánované zálohování, které bude inicializován pomocí těchto zásad.
* **Plány** – počet plány přidružené zásady zálohování.

Jsou často používaných operace, které můžete provádět z této stránky:

* Přidání zásady zálohování 
* Přidat nebo změnit plán 
* Odstranit zásady zálohování 
* Proveďte ruční zálohy 
* Vytvořit vlastní zásady zálohování s více svazků a plány 

## <a name="add-a-backup-policy"></a>Přidání zásady zálohování
Přidáte zásadu zálohování automaticky naplánovat zálohování. Proveďte následující kroky na portálu Azure classic přidat zásady zálohování pro zařízení StorSimple. Po přidání zásady můžete definovat plán (viz [přidat nebo změnit plán](#add-or-modify-a-schedule)).

[!INCLUDE [storsimple-add-backup-policy-u2](../../includes/storsimple-add-backup-policy-u2.md)]

![Dostupné video](./media/storsimple-manage-backup-policies-u2/Video_icon.png)**Dostupné video**

Pokud chcete přehrát video, které ukazuje, jak vytvořit místní nebo cloudové zásady zálohování, klikněte na tlačítko [zde](https://azure.microsoft.com/documentation/videos/create-storsimple-backup-policies/).

## <a name="add-or-modify-a-schedule"></a>Přidat nebo změnit plán
Můžete přidat nebo změnit plán, který je připojen k existující zásady zálohování v zařízení StorSimple. Proveďte následující kroky na portálu Azure classic k přidání nebo úpravě plánu.

[!INCLUDE [storsimple-add-modify-backup-schedule](../../includes/storsimple-add-modify-backup-schedule-u2.md)]

## <a name="delete-a-backup-policy"></a>Odstranit zásady zálohování
Proveďte následující kroky na portálu Azure classic odstranit zásady zálohování v zařízení StorSimple.

[!INCLUDE [storsimple-delete-backup-policy](../../includes/storsimple-delete-backup-policy.md)]

## <a name="take-a-manual-backup"></a>Proveďte ruční zálohy
Proveďte následující kroky na portálu Azure classic vytvořit zálohu na vyžádání (ručně) pro jeden svazek.

[!INCLUDE [storsimple-create-manual-backup](../../includes/storsimple-create-manual-backup.md)]

## <a name="create-a-custom-backup-policy-with-multiple-volumes-and-schedules"></a>Vytvořit vlastní zásady zálohování s více svazků a plány
Proveďte následující kroky na portálu Azure classic k vytvoření vlastní zásady zálohování, který má více svazků a plány.

[!INCLUDE [storsimple-create-custom-backup-policy](../../includes/storsimple-create-custom-backup-policy-u2.md)]

## <a name="next-steps"></a>Další kroky
Další informace o [pomocí služby StorSimple Manager ke správě zařízení StorSimple](storsimple-manager-service-administration.md).

