---
title: "Změna hesla pomocí Správce zařízení StorSimple | Microsoft Docs"
description: "Popisuje, jak změnit správce hesla služby StorSimple Snapshot Manager a zařízení pomocí služby StorSimple Manager."
services: storsimple
documentationcenter: NA
author: alkohli
manager: carmonm
editor: 
ms.assetid: f178509c-f4e1-48a8-90b2-d4ad050eeb30
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 08/17/2016
ms.author: alkohli
ms.openlocfilehash: d890b59595628ca3eeff1df258847c2bb54d29ff
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="use-the-storsimple-manager-service-to-change-your-storsimple-passwords"></a>Použití služby StorSimple Manager, chcete-li změnit hesla služby StorSimple
## <a name="overview"></a>Přehled
Portál Azure classic **konfigurace** stránka obsahuje všechny parametry zařízení, které můžete změnit konfiguraci na zařízení StorSimple, který je spravovaný nástrojem služby StorSimple Manager. Tento kurz popisuje, jak můžete použít **konfigurace** stránky změnit správce zařízení nebo heslo Snapshot Manager zařízení StorSimple.

## <a name="change-the-device-administrator-password"></a>Změna hesla správce zařízení
Pokud použijete rozhraní Windows PowerShell k přístupu k zařízení StorSimple, musíte k zadání hesla správce zařízení. Pokud služba není zaregistrována prvního zařízení StorSimple, výchozí heslo pro toto rozhraní je *Heslo1*. Pro zabezpečení vašich dat musíte změnit toto heslo na konci procesu registrace. Nelze ukončit proces registrace beze změny toto heslo. Další informace najdete v tématu [krok 3: Konfigurace a registrace zařízení pomocí Windows Powershellu pro StorSimple](storsimple-deployment-walkthrough-u2.md#step-3-configure-and-register-the-device-through-windows-powershell-for-storsimple).

Heslo, které se nejdřív nastavit pomocí rozhraní Windows PowerShell během registrace lze potom změnit prostřednictvím portálu Azure classic. Proveďte následující kroky, chcete-li změnit heslo správce zařízení.

#### <a name="to-change-the-device-administrator-password"></a>Chcete-li změnit heslo správce zařízení
1. Na portálu classic klikněte na tlačítko **zařízení** > **konfigurace** pro vaše zařízení.
2. Přejděte dolů k položce **heslo správce zařízení** části. Zadejte heslo správce, který obsahuje 8 až 15 znaků. Heslo musí být kombinací 3 nebo více velká písmena, malá písmena, číselné a speciální znaky.
3. Potvrzení hesla.
4. V dolní části stránky klikněte na **Uložit**.

Nyní je třeba aktualizovat heslo správce zařízení. Můžete toto upravené heslo pro přístup k rozhraní Windows PowerShell.

## <a name="change-the-storsimple-snapshot-manager-password"></a>Změňte heslo Snapshot Manager zařízení StorSimple
Software Snapshot Manager zařízení StorSimple se nachází na hostiteli systému Windows a umožňuje správcům spravovat zálohy zařízení StorSimple ve formě místních a cloudových snímků.

Při konfiguraci zařízení ve Snapshot Manageru zařízení StorSimple, budete vyzváni k zadání zařízení IP adresu a heslo k ověření zařízení úložiště. Toto heslo je nejprve nakonfigurovat přes rozhraní Windows PowerShell. Další informace najdete v tématu [krok 3: Konfigurace a registrace zařízení pomocí Windows Powershellu pro StorSimple](storsimple-deployment-walkthrough-u2.md#step-3-configure-and-register-the-device-through-windows-powershell-for-storsimple).

Heslo, které se nejdřív nastavit pomocí rozhraní Windows PowerShell během registrace lze potom změnit prostřednictvím portálu classic. Proveďte následující kroky, chcete-li změnit heslo Snapshot Manager zařízení StorSimple.

#### <a name="to-change-the-storsimple-snapshot-manager-password"></a>Chcete-li změnit heslo Snapshot Manager zařízení StorSimple
1. Na portálu classic klikněte na tlačítko **zařízení** > **konfigurace** pro vaše zařízení.
2. Přejděte dolů k položce **StorSimple Snapshot Manager** části. Zadejte heslo, které je tvořeno 14 až 15 znaků. Ujistěte se, zda heslo obsahuje kombinaci 3 nebo více velká písmena, malá písmena, číselné a speciální znaky.
3. Potvrzení hesla.
4. V dolní části stránky klikněte na **Uložit**.

Nyní je třeba aktualizovat heslo Snapshot Manager zařízení StorSimple.

## <a name="next-steps"></a>Další kroky
* Další informace o [zabezpečení zařízení StorSimple](storsimple-security.md).
* Další informace o [úprava konfiguraci zařízení](storsimple-modify-device-config.md).
* Další informace o [pomocí služby StorSimple Manager ke správě zařízení StorSimple](storsimple-manager-service-administration.md).

