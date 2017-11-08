---
title: "Obnovit svazek StorSimple ze zálohy | Microsoft Docs"
description: "Vysvětluje, jak používat stránky zálohování katalogu služby StorSimple Manager obnovit svazek StorSimple ze zálohovacího skladu."
services: storsimple
documentationcenter: NA
author: alkohli
manager: carmonm
editor: 
ms.assetid: b979782e-3184-4465-ad5f-e4516a5885d2
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 11/03/2017
ms.author: alkohli
ms.openlocfilehash: 0ed433ba93cb08742e3f0b09cc7acaa2cd62461e
ms.sourcegitcommit: 0930aabc3ede63240f60c2c61baa88ac6576c508
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/07/2017
---
# <a name="restore-a-storsimple-volume-from-a-backup-set"></a>Obnovit svazek StorSimple ze zálohovacího skladu
> [!NOTE]
> Portálu classic pro StorSimple je zastaralý. Váš správce zařízení StorSimple se automaticky přesune na nový portál Azure podle plánu vyřazení. Obdržíte e-mailu a portálu oznámení pro přesunutí. Tento dokument také vyřadí brzy k dispozici. Dotazy týkající se přechodu, najdete v části [– nejčastější dotazy: přesunout do portálu Azure](storsimple-8000-move-azure-portal-faq.md).

[!INCLUDE [storsimple-version-selector-restore-from-backup](../../includes/storsimple-version-selector-restore-from-backup.md)]

## <a name="overview"></a>Přehled
**Zálohování katalogu** stránka zobrazuje všechny zálohovací sklady, které vytvářejí, když jsou provedeny ruční nebo automatické zálohy. Můžete tato stránka slouží k zobrazení seznamu všech zálohy pro zásady zálohování nebo svazek, vyberte nebo odstranit zálohy, nebo použít zálohu k obnovení nebo klonování svazku.

 ![Zálohování stránky katalogu](./media/storsimple-restore-from-backup-set/HCS_BackupCatalog.png)

Tento kurz vysvětluje, jak používat **zálohování katalogu** stránku obnovit svazek v zařízení ze zálohovacího skladu.

## <a name="how-to-use-the-backup-catalog"></a>Použití zálohování katalogu
**Zálohování katalogu** stránka obsahuje dotaz, který umožňuje zúžit zálohování nastavte výběr. Můžete filtrovat zálohovací sklady, které jsou načteny na základě následujících parametrů:

* **Zařízení** – zařízení, v němž byla vytvořena zálohovacího skladu.
* **Zásady zálohování** nebo **svazku** – zásady zálohování nebo svazku přidruženém k tohoto zálohovacího skladu.
* **Z** a **k** – rozsah data a času v okamžiku vytvoření zálohovacího skladu.

Filtrované zálohovací sklady jsou pak poskytovalo na základě následujících atributů:

* **Název** – název zásady zálohování nebo svazku přidruženém k zálohovacího skladu.
* **Velikost** – skutečná velikost zálohovacího skladu.
* **Vytvořit v** – datum a čas, kdy byly vytvořeny zálohy. 
* **Typ** – zálohovací sklady může být místní snímky nebo cloudových snímků. Místní snímek je zálohování všech dat uložených místně na zařízení, svazku, zatímco cloudový snímek odkazuje na zálohování svazku dat umístěných v cloudu. Místní snímky poskytují rychlejší přístup, že jsou pro záleží na odolnosti dat zvolena cloudových snímků.
* **Zahájit** – zálohy lze inicializovat automaticky podle plánu nebo ručně uživatelem. (Můžete použít zásady zálohování naplánovat zálohování. Alternativně můžete použít **provést zálohování** možnost provést zálohu interaktivní.)

## <a name="how-to-restore-your-storsimple-volume-from-a-backup"></a>Jak obnovit svazek StorSimple ze zálohy
Můžete použít **zálohování katalogu** stránku obnovit svazek StorSimple ze konkrétní zálohy. 

> [!WARNING]
> Obnovení ze zálohy nahradí existující svazky ze zálohy. To může způsobit ztrátu všechna data, která byla zapsána po vytvoření zálohy.
> 
> 

Než zahájíte obnovení ve svazku, ujistěte se, že je svazek offline. Budete muset provést offline svazek na hostiteli první a pak zařízení. Postupujte podle kroků v [do offline režimu svazku](storsimple-manage-volumes.md#take-a-volume-offline). Proveďte následující kroky k obnovení svazku ze zálohovacího skladu.

### <a name="to-restore-from-a-backup-set"></a>Obnovení ze zálohovacího skladu
1. Na stránce služby StorSimple Manager, klepněte **katalog zálohování** kartě.
   
    ![Zálohování katalogu](./media/storsimple-restore-from-backup-set/HCS_Restore.png)
2. Vyberte zálohovací sklad následujícím způsobem:
   
   1. Vyberte příslušné zařízení.
   2. V rozevíracím seznamu vyberte svazek nebo zálohování zásady pro zálohu, kterou chcete vybrat.
   3. Zadejte časový rozsah.
   4. Klikněte na ikonu zaškrtnutí ![ikona zaškrtnutí](./media/storsimple-restore-from-backup-set/HCS_CheckIcon.png) k provedení tohoto dotazu.
      
      Zálohování přidružené k vybranému svazku nebo zásady zálohování by se měla objevit v seznamu sad záloh.
3. Rozbalte zálohovací sklad zobrazíte přidružené svazky. Tyto svazky musí být převedeno do režimu offline v hostiteli a zařízení, než bude možné obnovit. Postupujte podle kroků v [do offline režimu svazku](storsimple-manage-volumes.md#take-a-volume-offline).
   
   > [!IMPORTANT]
   > Ujistěte se, že jste provedli svazky do offline režimu na hostiteli nejprve, před provedením svazky do režimu offline v zařízení. Pokud neprovedete offline svazky na hostiteli, může potenciálně vést k poškození dat.
   > 
   > 
4. Vyberte zálohovacího skladu. Klikněte na tlačítko **obnovení** v dolní části stránky.
5. Zobrazí se výzva k potvrzení. 
   
    ![Stránka potvrzení](./media/storsimple-restore-from-backup-set/HCS_ConfirmRestore.png)
6. Zkontrolovat informace o obnovení a klikněte na ikonu zaškrtnutí ![ikona zaškrtnutí](./media/storsimple-restore-from-backup-set/HCS_CheckIcon.png). Tím zahájíte obnovení úlohu, která si můžete zobrazit přístup **úlohy** stránky. 
7. Po dokončení obnovení můžete ověřit, že obsah svazků jsou nahrazovány svazků ze zálohy.

![Dostupné video](./media/storsimple-restore-from-backup-set/Video_icon.png)**Dostupné video**

Pokud chcete přehrát video, které ukazuje, jak můžete použít klonu a funkce v zařízení StorSimple obnovit odstraněné soubory obnovit, klikněte na tlačítko [zde](https://azure.microsoft.com/documentation/videos/storsimple-recover-deleted-files-with-storsimple/).

## <a name="next-steps"></a>Další kroky
* Zjistěte, jak [svazky spravovat zařízení StorSimple](storsimple-manage-volumes.md).
* Zjistěte, jak [použít službu StorSimple Manager ke správě zařízení StorSimple](storsimple-manager-service-administration.md).

