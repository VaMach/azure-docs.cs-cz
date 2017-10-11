---
title: "Spravovat vaše kontejnery svazků zařízení StorSimple | Microsoft Docs"
description: "Vysvětluje, jak můžete použít stránku svazku kontejnery služby StorSimple Manager přidat, upravit nebo odstranit kontejner svazků."
services: storsimple
documentationcenter: NA
author: SharS
manager: carmonm
editor: 
ms.assetid: 1c64ce75-1fd3-4d3b-9304-d4dc0fc2b069
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 05/24/2016
ms.author: v-sharos
ms.openlocfilehash: bb55a7a4bff0fd4319de6f6ce958686ad8a4142b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="use-the-storsimple-manager-service-to-manage-storsimple-volume-containers"></a>Použít službu StorSimple Manager ke správě kontejnery svazků zařízení StorSimple
## <a name="overview"></a>Přehled
Tento kurz vysvětluje, jak používat službu StorSimple Manager k vytváření a správě kontejnery svazků zařízení StorSimple.

Kontejner svazků v zařízení s Microsoft Azure StorSimple obsahuje jeden nebo více svazků, které sdílejí účet úložiště, šifrování a nastavení spotřeba šířky pásma. Zařízení může mít několik kontejnery svazků pro všechny jeho svazky. 

Kontejner svazků má následující atributy:

* **Svazky** – vrstvené nebo místně vázaný svazky zařízení StorSimple, které jsou obsaženy v rámci kontejneru svazků. Kontejner svazků může obsahovat až 256 svazky zařízení StorSimple.
* **Šifrování** – šifrovací klíč, který lze definovat pro každý kontejner svazků. Tento klíč se používá pro šifrování dat, který se odesílá z zařízení StorSimple do cloudu. Klíč vojenských úrovni AES 256 bitů se používá s klíčem zadanou uživatelem. K zabezpečení dat, doporučujeme vždy povolit šifrování úložiště cloudu.
* **Účet úložiště** – účet úložiště, který je propojený s poskytovatele cloudových služeb úložiště. Všechny svazky, které se nacházejí v kontejneru svazků sdílet tento účet úložiště. Můžete vybrat účet úložiště z existujícího seznamu nebo vytvořte nový účet při vytváření kontejneru svazku a pak zadejte přihlašovací údaje pro tento účet přístup.
* **Cloud šířky pásma** – šířku pásma spotřebovávají zařízení při odesílání dat ze zařízení do cloudu. Zadejte hodnotu mezi 1 a 1000 Mb/s při definování tento kontejner může vynutit řízení šířky pásma. Pokud chcete spotřebovávat veškerou dostupnou šířku pásma na zařízení, nastavte pole na neomezený. Můžete také vytvořit a použít šablonu šířky pásma přidělení šířky pásma na základě plánu.

![Stránka kontejnery svazku](./media/storsimple-manage-volume-containers/HCS_VolumeContainersPage.png)

Tato následující postupy popisují, jak se StorSimple **kontejnery svazků** stránka dokončení následující běžné operace:

* Přidat kontejner svazků 
* Upravit kontejneru svazků 
* Odstranit kontejner svazků 

## <a name="add-a-volume-container"></a>Přidat kontejner svazků
Proveďte následující postup pro přidání kontejner svazků.

[!INCLUDE [storsimple-add-volume-container](../../includes/storsimple-add-volume-container.md)]

## <a name="modify-a-volume-container"></a>Upravit kontejneru svazků
Proveďte následující kroky k úpravě kontejner svazků.

[!INCLUDE [storsimple-modify-volume-container](../../includes/storsimple-modify-volume-container.md)]

## <a name="delete-a-volume-container"></a>Odstranit kontejner svazků
Kontejner svazků obsahuje svazky v něm. Lze odstranit pouze v případě, že všechny svazky, které v ní jsou nejprve odstranit. Proveďte následující kroky se odstranit kontejner svazků.

[!INCLUDE [storsimple-delete-volume-container](../../includes/storsimple-delete-volume-container.md)]

## <a name="next-steps"></a>Další kroky
* Další informace o [správu svazků zařízení StorSimple](storsimple-manage-volumes.md). 
* Další informace o [pomocí služby StorSimple Manager ke správě zařízení StorSimple](storsimple-manager-service-administration.md).

