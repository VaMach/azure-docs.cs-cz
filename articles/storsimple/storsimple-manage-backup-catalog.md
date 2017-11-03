---
title: "Spravovat katalog záloh StorSimple | Microsoft Docs"
description: "Vysvětluje, jak používat stránky zálohování katalogu služby StorSimple Manager k seznamu, vyberte a odstraňte zálohovací sklady pro svazek."
services: storsimple
documentationcenter: NA
author: SharS
manager: carmonm
editor: 
ms.assetid: ad81bee9-fe43-40b3-a384-b15fb274ecd9
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 04/28/2016
ms.author: v-sharos
ms.openlocfilehash: 5ee9855e1428c7a2d871d9c215d302c5c3b7101a
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="use-the-storsimple-manager-service-to-manage-your-backup-catalog"></a>Použít službu StorSimple Manager ke správě katalog záloh
## <a name="overview"></a>Přehled
Služby StorSimple Manager **zálohování katalogu** stránka zobrazuje všechny zálohovací sklady, které vytvářejí, když jsou provedeny ruční nebo plánované zálohy. Můžete tato stránka slouží k zobrazení seznamu všech zálohy pro zásady zálohování nebo svazek, vyberte nebo odstranit zálohy, nebo použít zálohu k obnovení nebo klonování svazku.

Tento kurz vysvětluje, jak zobrazit seznam, vyberte a odstraňte zálohovacího skladu. Chcete-li se dozvědět, jak obnovit ze zálohy zařízení, přejděte na [obnovit vaše zařízení ze zálohovacího skladu](storsimple-restore-from-backup-set.md). Chcete-li se dozvědět, jak vytvořit kopii svazku, přejděte na [klonovat svazek StorSimple](storsimple-clone-volume.md).

![Zálohování katalogu](./media/storsimple-manage-backup-catalog/backupcatalog.png) 

**Zálohování katalogu** stránka obsahuje dotaz, chcete-li zúžit zálohování sadu výběr. Můžete filtrovat zálohovací sklady, které jsou načteny, na základě následujících parametrů:

* **Zařízení** – zařízení, v němž byla vytvořena zálohovacího skladu.
* **Zálohování zásady nebo svazek** – zásady zálohování nebo svazku přidruženém k tohoto zálohovacího skladu.
* **Od a do** – rozsah data a času v okamžiku vytvoření zálohovacího skladu.

Filtrované zálohovací sklady jsou pak poskytovalo na základě následujících atributů:

* **Název** – název zásady zálohování nebo svazku přidruženém k zálohovacího skladu.
* **Velikost** – skutečná velikost zálohovacího skladu.
* **Vytvořit na** – datum a čas, kdy byly vytvořeny zálohy. 
* **Typ** – zálohovací sklady může být místní snímky nebo cloudových snímků. Místní snímek je zálohování všech dat uložených místně na zařízení, svazku, zatímco cloudový snímek odkazuje na zálohování svazku dat umístěných v cloudu. Místní snímky poskytují rychlejší přístup, že jsou pro záleží na odolnosti dat zvolena cloudových snímků.
* **Iniciováno** – zálohy lze inicializovat automaticky podle plánu nebo ručně uživatelem. Zásady zálohování můžete naplánovat zálohování. Alternativně můžete použít **provést zálohování** možnost provedení ruční zálohy.

## <a name="list-backup-sets-for-a-volume"></a>Seznam zálohovací sklady pro svazek clusteru
Pomocí následujících kroků na seznamu všechny zálohy svazku.

#### <a name="to-list-backup-sets"></a>Do seznamu zálohovací sklady
1. Na stránce služby StorSimple Manager, klepněte **katalog zálohování** kartě.
2. Výběr filtru následujícím způsobem:
   
   1. Vyberte příslušné zařízení.
   2. V rozevíracím seznamu vyberte svazek k zobrazení odpovídající zálohy.
   3. Zadejte časový rozsah.
   4. Klikněte na ikonu zaškrtnutí ![Ikona zaškrtnutí](./media/storsimple-manage-backup-catalog/HCS_CheckIcon.png) k provedení tohoto dotazu.
      
      Zálohování přidružené k vybranému svazku by se zobrazit v seznamu sad záloh.

## <a name="select-a-backup-set"></a>Vyberte zálohovací sklad
Pomocí následujících kroků vyberte zálohovacího skladu pro svazek nebo zásady zálohování.

#### <a name="to-select-a-backup-set"></a>Chcete-li vybrat zálohovacího skladu
1. Na stránce služby StorSimple Manager, klepněte **katalog zálohování** kartě.
2. Výběr filtru následujícím způsobem:
   
   1. Vyberte příslušné zařízení.
   2. V rozevíracím seznamu vyberte svazek nebo zálohování zásady pro zálohu, kterou chcete vybrat.
   3. Zadejte časový rozsah.
   4. Klikněte na ikonu zaškrtnutí ![Ikona zaškrtnutí](./media/storsimple-manage-backup-catalog/HCS_CheckIcon.png) k provedení tohoto dotazu.
      
      Zálohování přidružené k vybranému svazku nebo zásady zálohování by se měla objevit v seznamu sad záloh.
3. Vyberte a rozbalte zálohovacího skladu. **Obnovení** a **odstranit** možnosti se zobrazí v dolní části stránky. Můžete provést některé z těchto akcí na zálohovací sklad, který jste vybrali.

## <a name="delete-a-backup-set"></a>Odstranit zálohovacího skladu
Odstraňte zálohy, když už chcete zachovat data s ním spojená. Proveďte následující kroky a odstraňte zálohovacího skladu.

#### <a name="to-delete-a-backup-set"></a>Chcete-li odstranit zálohovacího skladu
1. Na stránce služby StorSimple Manager, klepněte **katalog zálohování karta**.
2. Výběr filtru následujícím způsobem:
   
   1. Vyberte příslušné zařízení.
   2. V rozevíracím seznamu vyberte svazek nebo zálohování zásady pro zálohu, kterou chcete vybrat.
   3. Zadejte časový rozsah.
   4. Klikněte na ikonu zaškrtnutí ![Ikona zaškrtnutí](./media/storsimple-manage-backup-catalog/HCS_CheckIcon.png) k provedení tohoto dotazu.
      
      Zálohování přidružené k vybranému svazku nebo zásady zálohování by se měla objevit v seznamu sad záloh.
3. Vyberte a rozbalte zálohovacího skladu. **Obnovení** a **odstranit** možnosti se zobrazí v dolní části stránky. Klikněte na **Odstranit**.
4. Při odstranění je v průběhu a kdy má úspěšně dokončena, budete upozorněni. Po odstranění probíhá, aktualizujte dotaz na této stránce. Odstraněné zálohovací sklad se nebude zobrazovat v seznamu sad záloh.

## <a name="next-steps"></a>Další kroky
* Zjistěte, jak [zálohování katalog používat k obnovení vašeho zařízení ze zálohovacího skladu](storsimple-restore-from-backup-set.md).
* Zjistěte, jak [použít službu StorSimple Manager ke správě zařízení StorSimple](storsimple-manager-service-administration.md).

