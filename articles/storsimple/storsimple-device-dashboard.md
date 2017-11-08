---
title: "Pomocí řídicího panelu zařízení StorSimple Manager | Microsoft Docs"
description: "Popisuje řídicí panel zařízení služby StorSimple Manager a způsobu jeho použití k zobrazení metriky úložiště a připojené iniciátory a najít sériové číslo a IQN."
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: 
ms.assetid: 6c213969-a385-461f-b698-78ef5b8a79cc
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 11/03/2017
ms.author: alkohli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: d2209790b54f5dd18a413f2e08ddc6ffd6aef662
ms.sourcegitcommit: 0930aabc3ede63240f60c2c61baa88ac6576c508
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/07/2017
---
# <a name="use-the-device-dashboard-in-storsimple-manager-service"></a>Pomocí řídicího panelu zařízení ve službě StorSimple Manager  
> [!NOTE]
> Portálu classic pro StorSimple je zastaralý. Váš správce zařízení StorSimple se automaticky přesune na nový portál Azure podle plánu vyřazení. Obdržíte e-mailu a portálu oznámení pro přesunutí. Tento dokument také vyřadí brzy k dispozici. Pokud chcete zobrazit verzi tohoto článku na nový portál Azure, přejděte na [pomocí řídicího panelu zařízení ve službě StorSimple Manager ](storsimple-8000-device-dashboard.md). Dotazy týkající se přechodu, najdete v části [– nejčastější dotazy: přesunout do portálu Azure](storsimple-8000-move-azure-portal-faq.md).


## <a name="overview"></a>Přehled
Řídicí panel zařízení StorSimple Manager bude stručně charakterizovat informace pro určité zařízení StorSimple, na rozdíl od řídicím panelu služby, která poskytuje informace o všech zařízení, které jsou součástí vašeho řešení Microsoft Azure StorSimple.

![Stránka řídicího panelu zařízení](./media/storsimple-device-dashboard/StorSimple_DeviceDashbaord1M.png)

Řídicí panel obsahuje následující informace:

* **Oblast grafu** – můžete zobrazit metriky relevantní úložiště v oblasti grafu v horní části řídicího panelu. V tomto grafu můžete zobrazit metrik pro celkový počet primárního úložiště (velikost dat zapsaných hostiteli k zařízení) a celkový počet cloudového úložiště spotřebovávají zařízení v časovém intervalu.
  
     V tomto kontextu *primárního úložiště* odkazuje na celkovém množství dat zapsaných hostitele a můžete rozdělit podle typu svazku: *primární vrstvené úložiště* zahrnuje obě místně uložená data a data vrstvené na cloudu; *primární místně vázaný úložiště* zahrnuje jenom místně uložená data. *Cloudového úložiště*, na druhé straně je měření celkové množství dat uložených v cloudu. To zahrnuje vrstvený dat a zálohování. Upozorňujeme, že se data uložená v cloudu s odstraněním duplicitních dat a komprimován, zatímco primárního úložiště určuje velikost úložiště využít, než je s odstraněním duplicitních dat a komprimované data. (Těchto dvou čísel, kde získáte představu míry komprese můžete porovnat.) Pro obě primární a cloudového úložiště, hodnoty uvedené bude možné na základě sledování četnosti nakonfigurujete. Například pokud zvolíte frekvencí jeden týden, pak grafu se zobrazí data pro každý den předchozího týdne.
  
     Graf můžete nakonfigurovat následujícím způsobem:
  
  * Pokud chcete zobrazit velikost úložiště v cloudu využívat v čase, vyberte **CLOUDOVÉ úložiště používá** možnost. Pokud chcete zobrazit celkový úložiště, která byla vytvořena pro hostitele, vyberte **primární VRSTVENÉ úložiště používá** a **primární MÍSTNĚ PŘIPNUTÝ úložiště používá** možnosti. Na obrázku jsou vybrány obě možnosti. graf zobrazuje proto objemy úložiště pro cloud a primárního úložiště. Všimněte si, že je reprezentována jakékoli primární úložiště používá před instalací aktualizace 2 **primární VRSTVENÉ úložiště používá** řádku.
  * Použijte rozevírací nabídky v pravém horním rohu grafu k určení 1 týden, měsíc 1, 3 měsíců nebo 1 rok časové období. Upozorňujeme, že nejvyšší úrovně grafu je aktualizovat jenom jednou denně a proto bude odrážet součty předchozí den.
    
    Další informace najdete v tématu [použít službu StorSimple Manager k monitorování zařízení StorSimple](storsimple-monitor-device.md).
* **Přehled využití** – v **přehled využití** oblasti, uvidíte velikost primárního úložiště používá, velikost zřízeného úložiště a maximální úložnou kapacitu pro vaše zařízení. Tak, že porovnáte tyto hodnoty využití pro maximální velikost úložiště, která je k dispozici, se zobrazí na první pohled Pokud potřebujete získat další úložiště. Všimněte si, že tento přehled je aktualizováno každých 15 minut a kvůli rozdílu ve četnosti aktualizace, může zobrazovat odlišné počty než ty zobrazené v oblasti grafu výše, která se aktualizuje denně. Další informace najdete v tématu [použít službu StorSimple Manager k monitorování zařízení StorSimple](storsimple-monitor-device.md).
* **Výstrahy** – **výstrahy** oblast obsahuje přehled výstrah pro vaše zařízení. Výstrahy jsou seskupené podle závažnosti a počet zajišťuje počtu výstrah na jednotlivé úrovně závažnosti. Kliknutím na tlačítko závažnost výstrahy otevře vymezenou zobrazení kartu upozornění k můžete zobrazit výstrahy této úrovně závažnosti pro toto zařízení.
* **Úlohy** – **úlohy** oblasti se dozvíte, výsledek poslední aktivitu na úlohu. To můžete zajistit, že systém funguje podle očekávání, nebo ji nechat vás informuje, že je potřeba provést nápravnou akci. Chcete-li zobrazit další informace o nedávno dokončené úlohy, klikněte na tlačítko **úspěšné úlohy za posledních 24 hodin**.
* **Rychlého přehledu** oblast na pravé straně řídicí panel poskytuje užitečné informace, jako je model zařízení, sériové číslo, stav, popisu a počtu svazků.

Můžete také konfigurovat převzetí služeb při selhání a zobrazit připojené iniciátory na řídicím panelu zařízení.

Běžné úlohy, které lze provést na této stránce jsou:

* Zobrazit připojené iniciátory
* Najít sériové číslo zařízení
* Nalezen element target zařízení IQN

## <a name="view-connected-initiators"></a>Zobrazit připojené iniciátory
Iniciátory iSCSI, které jsou připojené k zařízení kliknutím můžete zobrazit **zobrazit připojená iniciátory** uvedeného v odkazu **rychlého přehledu** oblasti řídicího panelu zařízení. Tato stránka obsahuje tabulkové seznam iniciátory, kterým úspěšně jste se připojili k zařízení. Pro každý iniciátor se zobrazí:

* ISCSI kvalifikovaný název IQN () připojené iniciátorů.
* Název záznam řízení přístupu (ACR), umožňuje tato připojené iniciátor.
* IP adresa iniciátoru připojené.
* Rozhraní sítě, které může iniciátor připojen k zařízení úložiště. Tyto rozsahu DATA 0 až DATA 5.
* Všechny svazky, které je povolen přístup podle aktuální konfigurace ACR iniciátoru připojené.

Pokud najdete v části neočekávané iniciátory v tomto seznamu nebo očekávanými výsledky nezobrazí, zkontrolujte konfiguraci ACR. Nesmí být delší než 512 iniciátory může připojit k zařízení.

## <a name="find-the-device-serial-number"></a>Najít sériové číslo zařízení
Sériové číslo zařízení může být nutné při konfiguraci Microsoft Multipath I/O (MPIO) na zařízení. Proveďte následující kroky k nalezení sériového čísla zařízení.

#### <a name="to-find-the-device-serial-number"></a>Najít sériové číslo zařízení
1. Přejděte na **zařízení** > **řídicí panel**.
2. V pravém podokně řídicí panel, vyhledejte **rychlého přehledu** oblasti.
3. Posuňte se dolů a najděte sériové číslo.

## <a name="find-the-device-target-iqn"></a>Nalezen element target zařízení IQN
Cílové zařízení IQN musíte při konfiguraci výzvu ověřování protokol CHAP (Handshake) v zařízení StorSimple. Proveďte následující kroky k nalezení cílové zařízení IQN.

### <a name="to-find-the-device-target-iqn"></a>Chcete-li nalezen element target zařízení IQN
1. Přejděte na **zařízení** > **řídicí panel**.
2. V pravém podokně řídicí panel, vyhledejte **rychlého přehledu** oblasti.
3. Přejděte dolů a vyhledejte příslušný cíl IQN.

## <a name="next-steps"></a>Další kroky
* Další informace o [řídicí panel služby StorSimple Manager](storsimple-service-dashboard.md).
* Další informace o [pomocí služby StorSimple Manager ke správě zařízení StorSimple](storsimple-manager-service-administration.md).

