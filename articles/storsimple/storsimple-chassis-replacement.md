---
title: "Nahraďte skříň na zařízení řady StorSimple 8000 | Microsoft Docs"
description: "Popisuje, jak odeberete a nahradíte skříň pro StorSimple primární skříň nebo EBOD skříň."
services: storsimple
documentationcenter: 
author: alkohli
manager: carmonm
editor: 
ms.assetid: 537659ed-4c46-49c1-b1e4-186262f2542d
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 11/02/2017
ms.author: alkohli
ms.openlocfilehash: 7bc23e64331ad18c604ffaa29476766827119cd4
ms.sourcegitcommit: 3df3fcec9ac9e56a3f5282f6c65e5a9bc1b5ba22
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/04/2017
---
# <a name="replace-the-chassis-on-your-storsimple-device"></a>Nahraďte skříň zařízení StorSimple
> [!NOTE]
> Portálu classic pro StorSimple je zastaralý. Váš správce zařízení StorSimple se automaticky přesune na nový portál Azure podle plánu vyřazení. Obdržíte e-mailu a portálu oznámení pro přesunutí. Tento dokument také vyřadí brzy k dispozici. Pokud chcete zobrazit verzi tohoto článku na nový portál Azure, přejděte na [nahradit skříň zařízení StorSimple](storsimple-8000-chassis-replacement.md). Dotazy týkající se přechodu, najdete v části [– nejčastější dotazy: přesunout do portálu Azure](storsimple-8000-move-azure-portal-faq.md).

## <a name="overview"></a>Přehled
Tento kurz vysvětluje, jak odeberete a nahradíte skříň v zařízení řady StorSimple 8000. Model StorSimple 8100 je zařízení jeden skříň (jednom šasi), zatímco 8600 je zařízení duální skříň (dvě chassis). Pro 8600 model jsou potenciálně dvě skříň, které by mohlo selhat v zařízení: Skříň pro primární skříň nebo skříň pro EBOD skříň.

V obou případech skříň nahrazení, který je dodáván společností Microsoft je prázdný. Žádné napájení a chlazení moduly (PCMs), moduly řadič diskové jednotky SSD (Solid-State Drive), pevných disků (HDD) nebo EBOD moduly budou zahrnuty.

> [!IMPORTANT]
> Před odebráním a nahraďte skříň, informace zabezpečení v [StorSimple hardwarové součásti nahrazení](storsimple-hardware-component-replacement.md).
> 
> 

## <a name="remove-the-chassis"></a>Odebrat skříň.
Proveďte následující kroky k odebrání skříň zařízení StorSimple.

#### <a name="to-remove-a-chassis"></a>Chcete-li odebrat skříň
1. Ujistěte se, že je zařízení StorSimple vypnout a odpojit od všech zdrojů energie.
2. Odeberte všechny sítě a SAS kabely, pokud je k dispozici.
3. Odeberte jednotku z racku.
4. Odeberte všechny jednotky a poznamenejte si sloty, ze kterých budou odstraněny. Další informace najdete v tématu [odebrat z disku](storsimple-disk-drive-replacement.md#remove-the-disk-drive).
5. Ve skříni EBOD (Pokud se jedná skříň, která se nezdařilo) odebrat moduly EBOD řadiče. Další informace najdete v tématu [odebrat řadič EBOD](storsimple-ebod-controller-replacement.md#remove-an-ebod-controller). 
   
    Na primární skříň (Pokud se jedná skříň, která se nezdařilo) odeberte řadiče a poznamenejte si sloty, ze kterých budou odstraněny. Další informace najdete v tématu [odebrání řadiče](storsimple-controller-replacement.md#remove-a-controller).

## <a name="install-the-chassis"></a>Nainstalujte skříň.
Proveďte následující kroky instalace skříň na zařízení StorSimple.

#### <a name="to-install-a-chassis"></a>Chcete-li nainstalovat skříň
1. Připojte skříň v racku. Další informace najdete v tématu [Rack připojení vaší 8100 StorSimple zařízení](storsimple-8100-hardware-installation.md#rack-mount-your-storsimple-8100-device) nebo [zařízení 8600 vaší StorSimple Rack připojení](storsimple-8600-hardware-installation.md#rack-mount-your-storsimple-8600-device).
2. Po skříň připojena v racku, nainstalujte na stejném místě, které byly dříve nainstalovány v moduly řadiče.
3. Nainstalujte na discích ve stejné pozic a sloty, které byly dříve nainstalovány v.
   
   > [!NOTE]
   > Doporučujeme nejprve nainstalovat SSD v přihrádkách a pak nainstalujte pevné disky.
   > 
   > 
4. U zařízení připojené v racku a komponenty nainstalované připojení zařízení k příslušné power zdroje a zapněte zařízení. Podrobnosti najdete v tématu [zapojení kabeláže zařízení StorSimple 8100](storsimple-8100-hardware-installation.md#cable-your-storsimple-8100-device) nebo [zapojení kabeláže zařízení StorSimple 8600](storsimple-8600-hardware-installation.md#cable-your-storsimple-8600-device).

## <a name="next-steps"></a>Další kroky
Další informace o [StorSimple hardwarové součásti nahrazení](storsimple-hardware-component-replacement.md).

