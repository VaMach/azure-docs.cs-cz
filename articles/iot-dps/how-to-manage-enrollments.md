---
title: "Jak spravovat registrace zařízení pro Azure IoT Hub | Microsoft Docs"
description: "Jak spravovat registrace zařízení služby distribučních bodů na portálu Azure"
services: iot-dps
keywords: 
author: dsk-2015
ms.author: dkshir
ms.date: 09/05/2017
ms.topic: article
ms.service: iot-dps
documentationcenter: 
manager: timlt
ms.devlang: na
ms.custom: mvc
ms.openlocfilehash: c230e73f83d8acd0f142e037f70a80c9e0e4107e
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-manage-device-enrollments-in-the-iot-hub-device-provisioning-service"></a>Jak spravovat registrace zařízení ve službě zřizování zařízení IoT Hub

A *registrace zařízení* vytvoří záznam jednoho zařízení nebo skupinu zařízení, které může v určitém okamžiku zaregistrovat ve službě Azure IoT Hub zařízení zřizování Service. Záznam zápisu obsahuje počáteční požadované konfigurace pro zařízení v rámci této registrace, včetně požadované služby IoT hub. Tento článek ukazuje, jak spravovat registrace zařízení služby zřizování.


## <a name="create-a-device-enrollment"></a>Vytvoření registrace zařízení

Existují dva způsoby, kterými se můžete zaregistrovat svoje zařízení v zřizování služby:

1. **Registrace skupiny** je záznam pro skupinu zařízení, které sdílejí společný mechanismus ověření certifikátů X.509, podepsaný stejnou kořenovou certifikační Autoritu. Doporučujeme používat skupinu registrace pro velký počet zařízení, které sdílejí požadované počáteční konfigurace, nebo pro zařízení všechny má stejné klienta. Všimněte si, že můžete registrovat jenom zařízení, která používají tento mechanismus ověření X.509 jako *registrace skupiny*. 

    Na portálu pro skupinu zařízení pomocí následujících kroků můžete vytvořit skupinu registrace.

    1. Přihlaste se k portálu Azure a klikněte na tlačítko **všechny prostředky** v nabídce vlevo.
    2. Klikněte na zřizování zařízení službu, která chcete zaregistrovat zařízení ze seznamu prostředků.
    3. Ve službě zřizování, klikněte na tlačítko **spravovat registrace**, pak vyberte **registrace skupiny** kartě.
    4. Klikněte **přidat** nahoře a zadejte informace požadované pro položku seznamu pro zápis. Nahrajte kořenový certifikát pro skupinu zařízení. 
    5. Klikněte na **Uložit**. V úspěšném vytvoření skupiny pro zápis, měli byste vidět název skupiny, se zobrazí pod **registrace skupiny** kartě. 

        ![Registrace skupiny na portálu](./media/how-to-manage-enrollments/group-enrollment.png)

    
2. **Jednotlivých registrace** je záznam pro jedno zařízení, která může zaregistrovat. Jednotlivé registrace může použít buď x509 certifikáty nebo SAS tokeny (ve fyzických nebo virtuálních čipu TPM) jako mechanismů ověření. Doporučujeme používat jednotlivé registrace zařízení, které vyžadují jedinečné počáteční konfigurace, nebo pro zařízení, které lze použít pouze tokeny SAS prostřednictvím TPM nebo virtuální čipu TPM jako mechanismus ověření. Jednotlivé registrace může mít požadovaný IoT hub ID zařízení v zadané.

    Na portálu pomocí následujících kroků můžete vytvořit jednotlivé zápisu. 

    1. Přihlaste se k portálu Azure a klikněte na tlačítko **všechny prostředky** v nabídce vlevo.
    2. Klikněte na zřizování zařízení službu, která chcete zaregistrovat zařízení ze seznamu prostředků.
    3. Ve službě zřizování, klikněte na tlačítko **spravovat registrace**, pak vyberte **jednotlivých registrace** kartě.
    4. Klikněte **přidat** tlačítka v horní části. 
    5. Vyberte mechanismus zabezpečení pro zařízení a zadejte informace požadované pro položku seznamu pro zápis. Nahrajte certifikát podepsaný držitelem, pokud zařízení implementuje X.509. 
    6. Klikněte na **Uložit**. V úspěšném vytvoření skupiny pro zápis, měli byste vidět zařízení se zobrazí pod **jednotlivých registrace** kartě. 

        ![Jednotlivé registrace na portálu](./media/how-to-manage-enrollments/individual-enrollment.png)


## <a name="update-an-enrollment-entry"></a>Aktualizovat položku registrace
Můžete aktualizovat existující položku registrace na portálu pomocí následujících kroků.

1. Otevřete služby zřizování zařízení na portálu Azure a klikněte na tlačítko **spravovat registrace**. 
2. Přejděte na položku registrace, který chcete upravit. Klikněte na položku, otevře se souhrnné informace o registraci zařízení. 
3. Na této stránce můžete upravit položky než typ zabezpečení a přihlašovací údaje, jako je například IoT hub zařízení musí být propojena, a také ID zařízení. Může také upravit dvojici stavu počáteční zařízení. 
4. Po dokončení klikněte na tlačítko **Uložit** aktualizovat registraci zařízení. 

    ![Aktualizace registrace na portálu](./media/how-to-manage-enrollments/update-enrollment.png)


## <a name="remove-a-device-enrollment"></a>Odebrání registrace zařízení
V případech, kde vaše zařízení nemusí být zřízená žádné Centrum IoT můžete odebrat položku související registrace na portálu pomocí následujících kroků.

1. Otevřete služby zřizování zařízení na portálu Azure a klikněte na tlačítko **spravovat registrace**. 
2. Přejděte do a vyberte položku registrace, který chcete odebrat. 
3. Klikněte **odstranit** nahoře a potom vyberte **Ano** po zobrazení výzvy k potvrzení. 
5. Po dokončení akce, zobrazí se zadání odebrat ze seznamu registrace zařízení. 
 
    ![Odebrání registrace na portálu](./media/how-to-manage-enrollments/remove-enrollment.png)



