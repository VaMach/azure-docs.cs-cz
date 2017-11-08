---
title: "Nahraďte diskovou jednotku na zařízení StorSimple | Microsoft Docs"
description: "Vysvětluje, jak nahradit diskovou jednotku na skříni primární zařízení StorSimple nebo EBOD skříň."
services: storsimple
documentationcenter: 
author: alkohli
manager: carmonm
editor: 
ms.assetid: 98890d36-b613-40fd-994e-330dd907a8a1
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 11/03/2017
ms.author: alkohli
ms.openlocfilehash: d236d43fd6c7595f5461204afb33cb38c821cb4b
ms.sourcegitcommit: 0930aabc3ede63240f60c2c61baa88ac6576c508
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/07/2017
---
# <a name="replace-a-disk-drive-on-your-storsimple-device"></a>Místo disku v zařízení StorSimple
> [!NOTE]
> Portálu classic pro StorSimple je zastaralý. Váš správce zařízení StorSimple se automaticky přesune na nový portál Azure podle plánu vyřazení. Obdržíte e-mailu a portálu oznámení pro přesunutí. Tento dokument také vyřadí brzy k dispozici. Pokud chcete zobrazit verzi tohoto článku na nový portál Azure, přejděte na [nahradit diskovou jednotku na zařízení StorSimple](storsimple-8000-disk-drive-replacement.md). Dotazy týkající se přechodu, najdete v části [– nejčastější dotazy: přesunout do portálu Azure](storsimple-8000-move-azure-portal-faq.md).

## <a name="overview"></a>Přehled
V tomto kurzu vysvětluje, jak můžete odeberete a nahradíte nefunkční nebo selhání jednotky pevného disku na zařízení s Microsoft Azure StorSimple. Chcete-li nahradit diskovou jednotku, je potřeba:

* Musí se vypnout antitamper zámek
* Odebrat z disku
* Nainstalujte na disk nahrazení

> [!IMPORTANT]
> Před odebírání a nahrazování diskovou jednotku, zkontrolujte informace zabezpečení v [StorSimple hardwarové součásti nahrazení](storsimple-hardware-component-replacement.md).
> 
> 

## <a name="disengage-the-antitamper-lock"></a>Musí se vypnout antitamper zámek
Tento postup vysvětluje, jak můžete antitamper zámky v zařízení StorSimple pověření nebo odpojí po nahrazení diskové jednotky. Antitamper zámky jsou umístěné v obslužných rutin nosného jednotky, a jsou přístupná prostřednictvím malou aperturou v části západky popisovač. Disky se dodává s zámky nastavena na poloze.

#### <a name="to-unlock-the-antitamper-lock"></a>K odemknutí antitamper zámek
1. Pečlivě vložení klávesy lock ("tamperproof" T10 šroubovák, které poskytuje Microsoft) do otvoru ve popisovač a do jeho soketu. 
   
   > [!NOTE]
   > Pokud je aktivovaná antitamper zámek, je zobrazen v otvoru red indikátoru.
   > 
   > 
   
    ![Uzamčení diskovou jednotku](./media/storsimple-disk-drive-replacement/IC741056.png)
   
    **Obrázek 1** proti zfalšování zámku pověření
   
   | Štítek | Popis |
   |:--- |:--- |
   | 1 |Otvoru indikátoru |
   | 2 |Antitamper zámku |
2. Otočte klíče v proti směru hodinových ručiček směr, dokud red indikátoru není zobrazená v otvoru nad klíčem.
3. Klíč odeberte.
   
    ![Odemknout diskovou jednotku](./media/storsimple-disk-drive-replacement/IC741057.png)
   
    **Obrázek 2** Odemknutý diskovou jednotku
4. Nyní nelze odebrat z disku.

Postupujte podle kroků v zpětného provozovat zámek.

## <a name="remove-the-disk-drive"></a>Odebrat z disku
Zařízení StorSimple podporuje konfiguraci RAID 10 jako úložiště prostorů. To znamená, že může fungovat normálně u jeden selhání disk SSD jednotky SSD (Solid-State Drive), nebo jednotku pevného disku (HDD). 

> [!IMPORTANT]
> * Pokud má váš systém více než jednoho disku, který selhal, neodebírejte víc než jeden SSD nebo HDD ze systému v libovolném bodě v čase. Díky tomu může dojít ke ztrátě dat.
> * Zajistěte, aby umístíte náhradní SSD ve slotu, která dříve obsahovala SSD. Podobně umístěte náhradní pevný disk ve slotu, která dříve obsahovala pevný disk.
> * Na portálu Azure classic jsou sloty číslo v rozsahu 0 – 11. Proto pokud portálu ukazuje, že na pozici 2 selhání disku, na zařízení, Hledat selhání disku ve třetí slotu z nahoře vlevo.
> 
> 

Jednotky můžete odebrat a nahradit, když je operační systém.

#### <a name="to-remove-a-drive"></a>Chcete-li odebrat jednotku
1. Chcete-li identifikovat selhání disku, na portálu Azure classic, přejděte na **zařízení** > **údržby** > **stavu hardwaru**. Vzhledem k tomu, že disk může selhat v primární skříň nebo EBOD skříň (Pokud používáte 8600 model), podívejte se na stav disků v části **sdílené součásti** a v části **EBOD skříň sdílené součásti**. Poškozený disk v buď skříň zobrazí červený stav.
2. Vyhledejte jednotky ve skříni primární nebo EBOD skříň. 
3. Pokud je disk odemknout, přejděte k dalšímu kroku. Pokud je na disku, odemknout pomocí postupu v [musí vypnout antitamper Zámek](#disengage-the-antitamper-lock).
4. Stiskněte klávesu černým západky v modulu poskytovatel jednotky a načítat popisovač poskytovatel jednotky odhlašování a rychle z před skříň. 
   
    ![Uvolnění popisovač diskovou jednotku](./media/storsimple-disk-drive-replacement/IC741051.png)
   
    **Obrázek 3** uvolnění popisovač jednotky
5. Když je poskytovatel popisovač jednotky plně rozšířeno, posuňte poskytovatel jednotky mimo skříň. 
   
    ![Klouzavé disku z disku](./media/storsimple-disk-drive-replacement/IC741052.png)
   
    **Obrázek 4** klouzavé diskové jednotce mimo zařadit

## <a name="install-the-replacement-disk-drive"></a>Nainstalujte na disk nahrazení
Jakmile jednotku se nezdařila v zařízení StorSimple a jste ji odstranili, pomocí následujícího postupu nahraďte ji na nový disk.

#### <a name="to-insert-a-drive"></a>Chcete-li vložit na jednotku
1. Ujistěte se, že poskytovatel popisovač jednotky je plně rozšířené, jak je znázorněno na následujícím obrázku.
   
    ![Diskovou jednotku s popisovačem rozšířené](./media/storsimple-disk-drive-replacement/IC741044.png)
   
    **Obrázek 5** jednotku s popisovačem rozšířené
2. Vysuňte poskytovatel jednotky úplně do skříň. 
   
    ![Klouzavé disku do poskytovatel diskovou jednotku](./media/storsimple-disk-drive-replacement/IC741045.png)
   
    **Obrázek 6** klouzavé poskytovatel jednotku do skříň.
3. S poskytovatel jednotky, vložit zavřete popisovač poskytovatel jednotky přitom dál tak, aby nabízel poskytovatel jednotku do skříň, dokud popisovač poskytovatel jednotky přichytí k uzamčení umístění.
4. Používá se zámek od společnosti Microsoft (tamperproof Torx šroubovák) zabezpečování popisovače poskytovatel do místní vypnutím šroubovacím Zámek čtvrtletí zapnout po směru hodinových ručiček.
5. Ověřte, zda nahrazení byla úspěšná a je tak, že přístup k portálu Azure classic a přejdete na funkční jednotku **údržby** > **stavu hardwaru**. V části **sdílené součásti** nebo **EBOD skříň sdílené součásti**, stav jednotky by měla být zeleně, která udává, že je v pořádku.
   
   > [!NOTE]
   > Může trvat několik hodin stav disku, chcete-li zelený po nahrazení.
   > 
   > 

## <a name="next-steps"></a>Další kroky
Další informace o [StorSimple hardwarové součásti nahrazení](storsimple-hardware-component-replacement.md).

