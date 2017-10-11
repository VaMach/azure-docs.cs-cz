---
title: "Změnit režim zařízení StorSimple | Microsoft Docs"
description: "Popisuje režimy zařízení StorSimple a vysvětluje, jak pomocí prostředí Windows PowerShell pro StorSimple ke změně režimu zařízení."
services: storsimple
documentationcenter: 
author: alkohli
manager: carmonm
editor: 
ms.assetid: e9d7d277-8a2f-45eb-9fef-355486e14cbc
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/17/2016
ms.author: alkohli
ms.openlocfilehash: 33c65bf2eecff3914f3227c76f7d638a4507e1f6
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="change-the-device-mode-on-your-storsimple-device"></a>Změnit režim zařízení na zařízení StorSimple
Tento článek obsahuje stručný popis v různých režimech, ve kterých může fungovat i zařízení StorSimple. Zařízení StorSimple, můžou fungovat v tři režimy: Normální, údržbu a obnovení. 

Po přečtení tohoto článku, budete vědět:

* Co režim pro zařízení StorSimple
* Jak zjistit, který režim zařízení StorSimple je v
* Postup změny mezi běžnou zátěží a režimu údržby a *naopak*

Výše uvedené úlohy správy lze provést pouze prostřednictvím rozhraní Windows PowerShell zařízení StorSimple.

## <a name="about-storsimple-device-modes"></a>O režimech zařízení StorSimple
Zařízení StorSimple můžou fungovat v režimu Normální, údržby nebo obnovení. Každá z těchto režimů stručně je popsána níže.

### <a name="normal-mode"></a>Normálním režimu
To je definován jako normální provozní režim pro zařízení StorSimple plně nakonfigurované. Ve výchozím nastavení musí být zařízení v normálním režimu.

### <a name="maintenance-mode"></a>Režim údržby
V některých případech zařízení StorSimple muset být umístěn do režimu údržby. Tento režim umožňuje provést údržbu na zařízení a instalovat aktualizace rušivým zásahům, například související s firmwarem disku.

Systém lze uvést do režimu údržby jenom prostřednictvím Windows Powershellu pro StorSimple. V tomto režimu jsou pozastavena všechny vstupně-výstupní požadavky. Také se zastaví službám, jako je paměť s náhodným přístupem stálé (paměti NVRAM) nebo službu clusteringu. Oběma řadičům se restartují, když zadáte nebo ukončit tento režim. Při ukončení režimu údržby se všechny služby bude pokračovat a musí být v pořádku. Může to trvat několik minut.

> [!NOTE]
> **Režim údržby je podporována pouze na zařízení správně funguje. Není podporována u zařízení, ve které jedna nebo obě řadičů nefungují.**
> </br>
> 
> 

### <a name="recovery-mode"></a>Obnovení režimu
Obnovení režimu lze popsat jako "Bezpečný režim pro Windows s podporou sítě". Obnovení režimu zapojí týmem Microsoft Support a umožňuje jim umožníte provádět diagnostiky v systému. Primární cílem režimu obnovení je načíst protokoly systému.

Pokud váš systém přejde do režimu obnovení, měli byste požádat Microsoft Support pro další kroky. Další informace, přejděte na [obraťte se na podporu společnosti Microsoft](storsimple-contact-microsoft-support.md).

> [!NOTE]
> **Zařízení nelze umístit v režimu obnovení. Pokud je ve špatném stavu zařízení, pokusí se režimu obnovení získat zařízení do stavu, ve kterém můžete zkontrolovat Microsoft Support pracovníky ho.**
> 
> 

## <a name="determine-storsimple-device-mode"></a>Určení režimu zařízení StorSimple
#### <a name="to-determine-the-current-device-mode"></a>Chcete-li zjistit aktuální režim zařízení
1. Přihlaste se k konzole sériového portu zařízení podle pokynů v [použití klienta PuTTY k připojení ke konzole sériového portu zařízení](storsimple-deployment-walkthrough.md#use-putty-to-connect-to-the-device-serial-console).
2. Podívejte se na zpráva hlavičky v nabídce konzoly sériového portu zařízení. Tato zpráva explicitně znamená, zda je zařízení v režimu údržby nebo obnovení. Pokud zpráva neobsahuje žádné konkrétní informace týkající se režimu systému, zařízení je v normálním režimu.

## <a name="change-the-storsimple-device-mode"></a>Změnit režim zařízení StorSimple
Zařízení StorSimple můžete umístit do režimu údržby (od normální režim) k provedení údržby nebo instalace aktualizací režimu údržby. Proveďte následující postupy k zadání nebo ukončení režimu údržby.

> [!IMPORTANT]
> Před přechodem do režimu údržby, ověřte, zda jsou oba řadiče zařízení v pořádku přímým přístupem **stavu hardwaru** na **údržby** na portálu Azure classic. Pokud jeden nebo oba řadiče nejsou v pořádku, požádejte o další kroky Microsoft Support. Další informace, přejděte na [obraťte se na podporu společnosti Microsoft](storsimple-contact-microsoft-support.md).
> 
> 

#### <a name="to-enter-maintenance-mode"></a>Přejít do režimu údržby
1. Přihlaste se k konzole sériového portu zařízení podle pokynů v [použití klienta PuTTY k připojení ke konzole sériového portu zařízení](storsimple-deployment-walkthrough.md#use-putty-to-connect-to-the-device-serial-console).
2. V nabídce konzoly sériového portu, zvolte možnost 1, **přihlásit úplný přístup**. Pokud budete vyzváni, zadejte **hesla správce zařízení**. Výchozí heslo je: `Password1`.
3. Na příkazovém řádku zadejte 
   
    `Enter-HcsMaintenanceMode`
4. Zobrazí se upozornění oznamující, že bude přerušit všechny vstupně-výstupní požadavky a severu připojení k portálu Azure classic režimu údržby a zobrazí se výzva k potvrzení. Typ **Y** vstoupit do režimu údržby.
5. Oba řadiče se restartuje. Po dokončení restartování se zobrazí další zpráva označující, že zařízení je v režimu údržby.

#### <a name="to-exit-maintenance-mode"></a>Chcete-li ukončit režim údržby
1. Přihlaste se ke konzole sériového portu zařízení. Ověřte ze zpráva hlavičky, která vaše zařízení je v režimu údržby.
2. Na příkazovém řádku zadejte:
   
    `Exit-HcsMaintenanceMode`
3. Zobrazí se zpráva s upozorněním a potvrzovací zpráva. Typ **Y** pro ukončení režimu údržby.
4. Oba řadiče se restartuje. Po dokončení restartování se zobrazí další zpráva označující, že zařízení je v normálním režimu.

## <a name="next-steps"></a>Další kroky
Zjistěte, jak [režim aktualizace normální a údržby](storsimple-update-device.md) zařízení StorSimple.

