---
title: "Klonování svazku řady StorSimple 8000 | Microsoft Docs"
description: "Popisuje typy různých klonování a jejich použití a vysvětluje, jak můžete pomocí zálohovacího skladu klonování svazku."
services: storsimple
documentationcenter: NA
author: alkohli
manager: carmonm
editor: 
ms.assetid: 070ac53e-7388-4c48-b8a5-8ed7f9108b2c
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 07/26/2017
ms.author: alkohli
ms.openlocfilehash: 2b627250df62bbe3299869ef3812947afbb8f29f
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="use-the-storsimple-manager-service-to-clone-a-volume-update-2"></a>Použít službu StorSimple Manager klonovat svazek (Update 2)
[!INCLUDE [storsimple-version-selector-clone-volume](../../includes/storsimple-version-selector-clone-volume.md)]

## <a name="overview"></a>Přehled
Služby StorSimple Manager **zálohování katalogu** stránka zobrazuje všechny zálohovací sklady, které vytvářejí, když jsou provedeny ruční nebo automatické zálohy. Můžete tato stránka slouží k zobrazení seznamu všech zálohy pro zásady zálohování nebo svazek, vyberte nebo odstranit zálohy, nebo použít zálohu k obnovení nebo klonování svazku.

![Stránka zálohování katalogu](./media/storsimple-clone-volume-u2/backupCatalog.png)  

Tento kurz popisuje, jak můžete pomocí zálohovacího skladu klonování svazku. Také vysvětluje rozdíly mezi *přechodný* a *trvalé* provede klonování.

> [!NOTE]
> Místně vázaný svazek, budou se klonovat jako vrstvený svazek. Pokud potřebujete klonovaný být místně vázaný svazek, můžete klonu převést místně vázaný svazek po úspěšném dokončení operace klonování. Informace o převodu vrstvený svazek na místně vázaný svazek, přejděte na [změnit typ svazku](storsimple-manage-volumes-u2.md#change-the-volume-type).
> 
> Pokud se pokusíte převést svazek klonovaný ze zřízeny vrstvené místně vázaný okamžitě po klonování (Pokud je stále přechodný klon), převod selže s se následující chybová zpráva:
> 
> `Unable to modify the usage type for volume {0}. This can happen if the volume being modified is a transient clone and hasn’t been made permanent. Take a cloud snapshot of this volume and then retry the modify operation.` 
> 
> Tato chyba byl přijat pouze v případě, že se klonování na jiném zařízení. Pokud převedete přechodný klonu nejdřív na trvalé klon místně vázaný svazek můžete úspěšně převést. Převést přechodný klonu trvalé klon, pořízení snímku cloudu ho.
> 
> 

## <a name="create-a-clone-of-a-volume"></a>Vytvořit klon svazku
Můžete vytvořit klon na stejné zařízení, jiná zařízení nebo i virtuální počítač pomocí místní nebo cloudový snímek.

#### <a name="to-clone-a-volume"></a>Klonování svazku
1. Na stránce služby StorSimple Manager, klepněte **katalog zálohování** a vyberte zálohovacího skladu.
2. Rozbalte zálohovací sklad zobrazíte přidružené svazky. Klikněte a vyberte svazek ze zálohovacího skladu.
   
     ![Klonování svazku](./media/storsimple-clone-volume-u2/CloneVol.png) 
3. Klikněte na tlačítko **klon** zahájíte klonování vybraného svazku.
4. V Průvodci klonování svazku v části **zadejte název a umístění**:
   
   1. Určete cílové zařízení. Toto je umístění, kde bude vytvořen klonu. Můžete vybrat stejné zařízení nebo zadejte jiné zařízení. Pokud zvolíte svazku přidruženém k ostatní poskytovatele cloudových služeb (ne Azure), v rozevíracím seznamu cílové zařízení se zobrazí pouze fyzických zařízení. Nelze klonovat svazku přidruženém k ostatní poskytovatele cloudových služeb na virtuální zařízení.
      
      > [!NOTE]
      > Ujistěte se, že kapacitu, vyžaduje se pro klonování je nižší než kapacity, které jsou k dispozici na cílovém zařízení.
      > 
      > 
   2. Zadejte název jedinečné svazku pro vaše klon. Název musí obsahovat 3 až 127 znaků. 
      
      > [!NOTE]
      > **Klonování svazku jako** pole bude **nastavování** i v případě, že se klonování místně vázaný svazek. Nelze změnit toto nastavení; ale pokud budete potřebovat klonovaný svazku, který má být místně vázaný i, můžete převést klonu pro místně vázaný svazek po úspěšném vytvoření klonu. Informace o převodu vrstvený svazek na místně vázaný svazek, přejděte na [změnit typ svazku](storsimple-manage-volumes-u2.md#change-the-volume-type).
      > 
      > 
      
        ![Průvodce klon 1](./media/storsimple-clone-volume-u2/clone1.png) 
   3. Kliknutím na ikonu šipky ![ikona šipky](./media/storsimple-clone-volume-u2/HCS_ArrowIcon.png) aby bylo možné pokračovat na další stránku.
5. V části **zadejte hostitelů, které můžete použít tento svazek**:
   
   1. Zadejte záznam řízení přístupu (ACR) pro klonování. Můžete přidat nové ACR nebo zvolte ze seznamu existující.
      
        ![Průvodce klon 2](./media/storsimple-clone-volume-u2/clone2.png) 
   2. Klikněte na ikonu zaškrtnutí ![ikona zaškrtnutí](./media/storsimple-clone-volume-u2/HCS_CheckIcon.png)k dokončení operace.
6. Úloha klonování bude inicializován a budete informováni při klonování se úspěšně vytvořil. Klikněte na tlačítko **zobrazit úlohy** na monitoru úlohu klonování **úlohy** stránky. Po dokončení úlohy klonování, zobrazí se následující zpráva:
   
    ![Zpráva klonování](./media/storsimple-clone-volume-u2/CloneMsg.png) 
7. Po dokončení úlohy klonu:
   
   1. Přejděte na **zařízení** a vyberte **kontejnery svazků** kartě. 
   2. Vyberte kontejner svazků, který je přidružen zdrojový svazek, který jste naklonovali. V seznamu svazků měli byste vidět klonování, kterou jste právě vytvořili.

> [!NOTE]
> Monitorování a výchozí zálohování jsou automaticky zakázán na klonovaný svazku.
> 
> 

Klon, který je vytvořen tímto způsobem je přechodný klonu. Další informace o typech klonování najdete v tématu [přechodný oproti trvalé klony](#transient-vs-permanent-clones).

Tenhle klon. je teď regulární svazek a všechny operace, které je možné ve svazku, bude k dispozici pro klonování. Musíte nakonfigurovat tento svazek pro všechny zálohy.

## <a name="transient-vs-permanent-clones"></a>Pouze dočasné a trvalé klony
Přechodný klony vytvoří jenom v případě, že se klonování na jiném zařízení. Může klonovat svazek konkrétní ze zálohovacího skladu na jiném zařízení spravovat pomocí Správce zařízení StorSimple. Přechodný klonu bude mít odkazů na data v původním svazku a bude používat tato data ke čtení a zápisu místně na cílovém zařízení. 

Po provedení cloudový snímek přechodný klon, bude výsledný klonování *trvalé* klonování. Během tohoto procesu se vytvoří kopie dat v cloudu a čas zkopírovat tato data je určena velikostí dat a Azure latenci (Toto je kopie Azure do Azure). Tento proces může trvat dnů, týdnů. Přechodný klon tímto způsobem se změní na trvalé klonování a nemá všechny odkazy na původní data svazku, která byla naklonována ze. 

## <a name="scenarios-for-transient-and-permanent-clones"></a>Scénáře pro přechodný a trvalé klony
Následující části popisují příklad situacích, ve kterých může být použit klony přechodný a trvalé.

### <a name="item-level-recovery-with-a-transient-clone"></a>Obnovení na úrovni položek s přechodný klon
Budete muset obnovit soubor s jedním. roku starý Microsoft PowerPoint. Váš správce IT identifikuje konkrétní zálohování z toto časové období a pak filtry svazku. Správce pak provede klonování svazku, vyhledá soubor, který hledáte a poskytuje vám. V tomto scénáři se používá přechodný klonu. 

![Dostupné video](./media/storsimple-clone-volume-u2/Video_icon.png) **Dostupné video**

Pokud chcete přehrát video, které ukazuje, jak můžete použít klonu a funkce v zařízení StorSimple obnovit odstraněné soubory obnovit, klikněte na tlačítko [zde](https://azure.microsoft.com/documentation/videos/storsimple-recover-deleted-files-with-storsimple/).

### <a name="testing-in-the-production-environment-with-a-permanent-clone"></a>Testování v produkčním prostředí s trvalé klon
Je třeba ověřit chyby testování v produkčním prostředí. Vytvořit klon svazku v provozním prostředí a pak proveďte cloudový snímek Tenhle klon. k vytvoření klonovaného svazek nezávislé. V tomto scénáři se používá trvalá klonu.  

## <a name="next-steps"></a>Další kroky
* Zjistěte, jak [obnovit svazek StorSimple ze zálohovacího skladu](storsimple-restore-from-backup-set-u2.md).
* Zjistěte, jak [použít službu StorSimple Manager ke správě zařízení StorSimple](storsimple-manager-service-administration.md).

