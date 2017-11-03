---
title: "Konfigurace protokolů CHAP pro zařízení řady StorSimple 8000 | Microsoft Docs"
description: "Popisuje, jak nakonfigurovat výzvu ověřování protokol CHAP (Handshake) na zařízení StorSimple."
services: storsimple
documentationcenter: 
author: alkohli
manager: carmonm
editor: 
ms.assetid: 467044d7-7885-4382-90bd-3148dbbd341f
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: TBD
ms.date: 08/17/2016
ms.author: alkohli
ms.openlocfilehash: 36b4e73d0336deb9560d44163fc5330d1c9d775c
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="configure-chap-for-your-storsimple-device"></a>Konfigurace protokolů CHAP pro vaše zařízení StorSimple
Tento kurz vysvětluje postup konfigurace CHAP pro vaše zařízení StorSimple. Postup popsaný v tomto článku platí pro řady StorSimple 8000 a také zařízení StorSimple 1200.

CHAP znamená Challenge Handshake Authentication Protocol. Je příslušné schéma ověřování používají servery pro ověření identity vzdálených klientů. Ověření je založena na sdílené heslo nebo tajný klíč. CHAP může být jednosměrná (Jednosměrný) nebo vzájemné (obousměrné). Jednosměrné CHAP je při cíl ověření iniciátor. Vzájemné nebo zpětného CHAP, na druhé straně vyžaduje, aby cíl ověření iniciátoru a pak iniciátoru ověření cíl. Iniciátor ověřování se dá implementovat bez cíl ověřování. Cíl ověřování však můžete implementovat, pouze v případě, že se také implementuje iniciátor ověřování. 

Jako osvědčený postup doporučujeme použít CHAP pro zvýšení zabezpečení iSCSI.

> [!NOTE]
> Uvědomte si, že protokol IPSEC není aktuálně podporován zařízení StorSimple.
> 
> 

CHAP nastavení v zařízení StorSimple můžete nakonfigurovat následujícím způsobem:

* Jednosměrný nebo jednosměrné ověřování
* Obousměrné nebo vzájemné nebo zpětného ověřování

V každé z těchto případech je potřeba nakonfigurovat na portálu pro zařízení a serverového softwaru iniciátoru iSCSI. Podrobné pokyny pro tuto konfiguraci jsou popsané v následujícím kurzu.

## <a name="unidirectional-or-one-way-authentication"></a>Jednosměrný nebo jednosměrné ověřování
Cíl v jednosměrný ověřování ověřuje iniciátor. Toto ověřování vyžaduje, že nakonfigurujete nastavení iniciátoru CHAP na zařízení StorSimple a iSCSI software Initiator na hostiteli. Podrobné pokyny k zařízení StorSimple a hostitele Windows jsou popsané dále.

#### <a name="to-configure-your-device-for-one-way-authentication"></a>Pro konfiguraci zařízení pro jednosměrné ověřování
1. Na portálu Azure classic na **zařízení** klikněte na tlačítko **konfigurace** kartě.
   
    ![Iniciátoru CHAP](./media/storsimple-configure-chap/IC740943.png)
2. Posuňte se dolů na této stránce a v **iniciátoru CHAP** části:
   
   1. Zadejte uživatelské jméno pro vaše iniciátoru CHAP.
   2. Zadejte heslo pro vaše iniciátoru CHAP.
      
    > [!IMPORTANT]
    > Uživatelské jméno CHAP musí obsahovat méně než 233 znaků. CHAP heslo musí být 12 až 16 znaků. Delší uživatelské jméno nebo heslo povede k chybě ověřování na hostiteli systému Windows.
   
   3. Potvrzení hesla.
3. Klikněte na **Uložit**. Zobrazí potvrzovací zpráva. Klikněte na tlačítko **OK** a uložte změny.

#### <a name="to-configure-one-way-authentication-on-the-windows-host-server"></a>Jednosměrné ověřování nakonfigurujete na hostitelském serveru Windows
1. Na hostitelském serveru systému Windows spusťte iniciátor iSCSI.
2. V **vlastnosti iniciátoru iSCSI** okno, proveďte následující kroky:
   
   1. Klikněte **zjišťování** kartě.
      
       ![Vlastnosti iniciátoru iSCSI](./media/storsimple-configure-chap/IC740944.png)
   2. Klikněte na tlačítko **zjistit portál**.
3. V **zjistit cílový portál** dialogové okno:
   
   1. Zadejte IP adresu vašeho zařízení.
   2. Klikněte na tlačítko **rozšířené**.
      
       ![Zjistit cílový portál](./media/storsimple-configure-chap/IC740945.png)
4. V **Upřesnit nastavení** dialogové okno:
   
   1. Vyberte **povolit CHAP přihlásit** zaškrtávací políčko.
   2. V **název** pole, zadejte uživatelské jméno, které jste zadali pro iniciátoru CHAP portálu classic.
   3. V **tajný klíč cíle** pole, zadejte heslo, které jste zadali pro iniciátoru CHAP portálu classic.
   4. Klikněte na **OK**.
      
       ![Upřesňující nastavení, obecné](./media/storsimple-configure-chap/IC740946.png)
5. Na **cíle** kartě **vlastnosti iniciátoru iSCSI** okně Stav zařízení mají zobrazit jako **připojeno**. Pokud používáte zařízení StorSimple 1200, pak každý svazek se připojí jako cíl iSCSI jak je uvedeno níže. Proto kroky 3 a 4 muset opakovat pro každý svazek.
   
    ![Svazky připojené jako samostatné cíle](./media/storsimple-configure-chap/chap4.png)
   
   > [!IMPORTANT]
   > Pokud změníte název iSCSI, nový název se použije pro nové relace iSCSI. Nové nastavení nejsou znovu použít pro existující až do odhlášení a přihlášení.
   > 
   > 

Další informace o konfiguraci CHAP na hostitelském serveru Windows, přejděte na [další aspekty](#additional-considerations).

## <a name="bidirectional-or-mutual-authentication"></a>Obousměrné nebo vzájemné ověřování
V obousměrné ověřování cíl ověřuje iniciátoru a pak iniciátoru ověřuje cíl. To vyžaduje, aby uživatel ke konfiguraci nastavení iniciátoru CHAP, jakož i zpětné CHAP nastavení na zařízení a iSCSI Initiator software na hostiteli. Následující postupy popisují postup konfigurace vzájemného ověřování v zařízení a na hostiteli systému Windows.

#### <a name="to-configure-your-device-for-mutual-authentication"></a>Pro konfiguraci zařízení pro vzájemné ověřování
1. Na portálu Azure classic na **zařízení** klikněte na tlačítko **konfigurace** kartě.
   
    ![Cíle CHAP](./media/storsimple-configure-chap/IC740948.png)
2. Posuňte se dolů na této stránce a v **CHAP cíl** části:
   
   1. Zadejte **Reverse CHAP uživatelské jméno** pro vaše zařízení.
   2. Zadejte **hesla Reverse CHAP** pro vaše zařízení.
   3. Potvrzení hesla.
3. V **iniciátoru CHAP** části:
   
   1. Zadejte **uživatelské jméno** pro vaše zařízení.
   2. Zadejte **heslo** pro vaše zařízení.
   3. Potvrzení hesla.
4. Klikněte na **Uložit**. Zobrazí potvrzovací zpráva. Klikněte na tlačítko **OK** a uložte změny.

#### <a name="to-configure-bidirectional-authentication-on-the-windows-host-server"></a>Postup konfigurace obousměrné ověřování na hostitelském serveru Windows
1. Na hostitelském serveru systému Windows spusťte iniciátor iSCSI.
2. V **vlastnosti iniciátoru iSCSI** okně klikněte na tlačítko **konfigurace** kartě.
3. Klikněte na tlačítko **CHAP**.
4. V **tajný klíč pro vzájemné CHAP iniciátor iSCSI** dialogové okno:
   
   1. Typ **hesla Reverse CHAP** nakonfigurovaný na portálu Azure classic.
   2. Klikněte na **OK**.
      
       ![iSCSI initiator vzájemné CHAP tajný klíč](./media/storsimple-configure-chap/IC740949.png)
5. Klikněte **cíle** kartě.
6. Klikněte **Connect** tlačítko. 
7. V **připojení k cílové** dialogové okno, klikněte na tlačítko **Upřesnit**.
8. V **Upřesnit vlastnosti** dialogové okno:
   
   1. Vyberte **povolit CHAP přihlásit** zaškrtávací políčko.
   2. V **název** pole, zadejte uživatelské jméno, které jste zadali pro iniciátoru CHAP portálu classic.
   3. V **tajný klíč cíle** pole, zadejte heslo, které jste zadali pro iniciátoru CHAP portálu classic.
   4. Vyberte **provést vzájemné ověřování** zaškrtávací políčko.
      
       ![Upřesnit nastavení vzájemného ověření](./media/storsimple-configure-chap/IC740950.png)
   5. Klikněte na tlačítko **OK** k dokončení konfigurace CHAP

Další informace o konfiguraci CHAP na hostitelském serveru Windows, přejděte na [další aspekty](#additional-considerations).

## <a name="additional-considerations"></a>Další aspekty
**Rychlé připojení** funkce nepodporuje připojení, které mají povolené CHAP. Pokud je povoleno CHAP, ujistěte se, že používáte **připojit** tlačítko, která je dostupná na **cíle** kartě připojit k cíli.

![Připojit k cíli](./media/storsimple-configure-chap/IC740947.png)

V **připojit k cíli** dialogu, který se zobrazí, vyberte **přidejte toto připojení do seznamu oblíbených cílů** zaškrtávací políčko. Tím se zajistí, že pokaždé, když se počítač restartuje, je proveden pokus o obnovení připojení k cílům iSCSI oblíbených položek.

## <a name="errors-during-configuration"></a>Chyby během konfigurace
Pokud vaše konfigurace CHAP je nesprávná, pak budete chtít nejspíš najdete v článku **selhání ověření** chybová zpráva.

## <a name="verification-of-chap-configuration"></a>Ověření konfigurace CHAP
Můžete ověřit, jestli se používá protokol CHAP podle následujících kroků.

#### <a name="to-verify-your-chap-configuration"></a>Chcete-li ověřit konfiguraci CHAP
1. Klikněte na tlačítko **Oblíbené cíle**.
2. Vyberte cíl, pro které jste povolili ověřování.
3. Klikněte na tlačítko **podrobnosti**.
   
    ![Oblíbené cíle vlastnosti iniciátoru iSCSI](./media/storsimple-configure-chap/IC740951.png)
4. V **Oblíbené podrobnosti cíl** dialogové okno pole si všimněte, položky v **ověřování** pole. Pokud konfigurace proběhla úspěšně, by mělo být uvedeno **CHAP**.
   
    ![Podrobnosti o oblíbeného cíle](./media/storsimple-configure-chap/IC740952.png)

## <a name="next-steps"></a>Další kroky
* Další informace o [zabezpečení zařízení StorSimple](storsimple-security.md).
* Další informace o [pomocí služby StorSimple Manager ke správě zařízení StorSimple](storsimple-manager-service-administration.md).

