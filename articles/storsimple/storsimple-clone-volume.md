---
title: "Klonování svazku zařízení StorSimple | Microsoft Docs"
description: "Popisuje typy různých klonování a jejich použití a vysvětluje, jak můžete pomocí zálohovacího skladu klonování svazku."
services: storsimple
documentationcenter: NA
author: alkohli
manager: carmonm
editor: 
ms.assetid: b5d615f2-02a7-4222-9867-6c0385ce748c
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 08/17/2016
ms.author: alkohli
ms.openlocfilehash: 8f1936fac543f559a44ad0f9c35b30d1a92dce68
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="use-the-storsimple-manager-service-to-clone-a-volume"></a>Použít službu StorSimple Manager klonování svazku
[!INCLUDE [storsimple-version-selector-clone-volume](../../includes/storsimple-version-selector-clone-volume.md)]

## <a name="overview"></a>Přehled
Služby StorSimple Manager **zálohování katalogu** stránka zobrazuje všechny zálohovací sklady, které vytvářejí, když jsou provedeny ruční nebo automatické zálohy. Můžete tato stránka slouží k zobrazení seznamu všech zálohy pro zásady zálohování nebo svazek, vyberte nebo odstranit zálohy, nebo použít zálohu k obnovení nebo klonování svazku.

![Stránka zálohování katalogu](./media/storsimple-clone-volume/HCS_BackupCatalog.png)  

Tento kurz popisuje, jak můžete pomocí zálohovacího skladu klonování svazku. Také vysvětluje rozdíly mezi *přechodný* a *trvalé* provede klonování. 

## <a name="create-a-clone-of-a-volume"></a>Vytvořit klon svazku
Můžete vytvořit klon na stejné zařízení, jiná zařízení nebo i virtuální počítač pomocí místní nebo cloudový snímek.

#### <a name="to-clone-a-volume"></a>Klonování svazku
1. Na stránce služby StorSimple Manager, klepněte **katalog zálohování** a vyberte zálohovacího skladu.
2. Rozbalte zálohovací sklad zobrazíte přidružené svazky. Klikněte a vyberte svazek ze zálohovacího skladu.
   
     ![Klonování svazku](./media/storsimple-clone-volume/HCS_Clone.png) 
3. Klikněte na tlačítko **klon** zahájíte klonování vybraného svazku.
4. V Průvodci klonování svazku v části **zadejte název a umístění**:
   
   1. Určete cílové zařízení. Toto je umístění, kde bude vytvořen klonu. Můžete vybrat stejné zařízení nebo zadejte jiné zařízení. Pokud zvolíte svazku přidruženém k ostatní poskytovatele cloudových služeb (ne Azure), v rozevíracím seznamu cílové zařízení se zobrazí pouze fyzických zařízení. Nelze klonovat svazku přidruženém k ostatní poskytovatele cloudových služeb na virtuální zařízení.
      
      > [!NOTE]
      > Ujistěte se, že kapacitu, vyžaduje se pro klonování je nižší než kapacity, které jsou k dispozici na cílovém zařízení.
      > 
      > 
   2. Zadejte název jedinečné svazku pro vaše klon. Název musí obsahovat 3 až 127 znaků.
   3. Kliknutím na ikonu šipky ![ikona šipky](./media/storsimple-clone-volume/HCS_ArrowIcon.png) aby bylo možné pokračovat na další stránku.
5. V části **zadejte hostitelů, které můžete použít tento svazek**:
   
   1. Zadejte záznam řízení přístupu (ACR) pro klonování. Můžete přidat nové ACR nebo zvolte ze seznamu existující.
   2. Klikněte na ikonu zaškrtnutí ![ikona zaškrtnutí](./media/storsimple-clone-volume/HCS_CheckIcon.png)k dokončení operace.
6. Úloha klonování bude inicializován a budete informováni při klonování se úspěšně vytvořil. Klikněte na tlačítko **zobrazit úlohy** na monitoru úlohu klonování **úlohy** stránky.
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
Přechodný a trvalé klony vytvoří jenom v případě, že se klonování na jiném zařízení. Může klonovat svazek konkrétní ze zálohovacího skladu na jiném zařízení. Klon vytvořené v tomto případě je *přechodný* klonování. Přechodný klonu bude mít odkazy na původního svazku a bude používat tento svazek čtení během zápisu místně. 

Po provedení cloudový snímek přechodný klon, bude výsledný klonování *trvalé* klonování. Trvalé klon je nezávislá a nemá všechny odkazy na původní svazek, který byl klonovat z.  

## <a name="scenarios-for-transient-and-permanent-clones"></a>Scénáře pro přechodný a trvalé klony
Následující části popisují příklad situacích, ve kterých může být použit klony přechodný a trvalé.

### <a name="item-level-recovery-with-a-transient-clone"></a>Obnovení na úrovni položek s přechodný klon
Budete muset obnovit soubor s jedním. roku starý Microsoft PowerPoint. Váš správce IT identifikuje konkrétní zálohování z toto časové období a pak filtry svazku. Správce pak provede klonování svazku, vyhledá soubor, který hledáte a poskytuje vám. V tomto scénáři se používá přechodný klonu. 

![Dostupné video](./media/storsimple-clone-volume/Video_icon.png) **Dostupné video**

Pokud chcete přehrát video, které ukazuje, jak můžete použít klonu a funkce v zařízení StorSimple obnovit odstraněné soubory obnovit, klikněte na tlačítko [zde](https://azure.microsoft.com/documentation/videos/storsimple-recover-deleted-files-with-storsimple/).

### <a name="testing-in-the-production-environment-with-a-permanent-clone"></a>Testování v produkčním prostředí s trvalé klon
Je třeba ověřit chyby testování v produkčním prostředí. Při vytváření klon svazku v provozním prostředí pořízení snímku cloudu Tenhle klon. Klonovaný svazek je nyní nezávislé. V tomto scénáři se používá trvalá klonu.

## <a name="next-steps"></a>Další kroky
* Zjistěte, jak [obnovit svazek StorSimple ze zálohovacího skladu](storsimple-restore-from-backup-set.md).
* Zjistěte, jak [použít službu StorSimple Manager ke správě zařízení StorSimple](storsimple-manager-service-administration.md).

