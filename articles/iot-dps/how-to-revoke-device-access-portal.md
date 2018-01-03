---
title: "Jak spravovat přístup k zařízení služby Azure IoT Hub zařízení zřizování | Microsoft Docs"
description: "Tom, jak odebrat zařízení přístup k službě distribučních bodů na portálu Azure"
services: iot-dps
keywords: 
author: JimacoMS
ms.author: v-jamebr
ms.date: 12/22/2017
ms.topic: article
ms.service: iot-dps
documentationcenter: 
manager: timlt
ms.devlang: na
ms.custom: mvc
ms.openlocfilehash: 7985fba68ef2c6f651c64756f8c534928b573de5
ms.sourcegitcommit: 4256ebfe683b08fedd1a63937328931a5d35b157
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/23/2017
---
# <a name="how-to-revoke-device-access-to-your-provisioning-service-in-the-azure-portal"></a>Tom, jak odebrat zařízení přístup ke službě zřizování na portálu Azure

Správné správu přihlašovacích údajů, zařízení je zásadní pro systémy vysokou profil jako řešení IoT. Osvědčeným postupem pro tyto systémy je mít zrušte plán o odvolání přístupu pro zařízení v případech, kde přihlašovacích údajů, zda SAS token nebo certifikátu X.509, může dojít k ohrožení. Tento článek popisuje, jak odvolat přístup k zařízením v kroku zřizování.

Další informace o odvolání přístupu zařízení do služby IoT hub po zařízení zřízená. v tématu [zakázat zařízení](https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-devguide-identity-registry#disable-devices).

> [!NOTE] 
> Pamatujte na zařízení, která odvolání přístupu pro zásady opakování. Zařízení s zásadu nekonečné opakování například nepřetržitě pokusit zaregistrovat službu zřizování náročné na prostředky služby a které by mohly mít mělo vliv na výkon.

## <a name="blacklist-devices-with-an-individual-enrollment-entry"></a>Zakázaných zařízení se záznamem jednotlivých registrace

Jednotlivé registrace platí pro jedno zařízení a mohou používat certifikáty X.509 nebo tokeny SAS (ve fyzických nebo virtuálních čipu TPM) jako mechanismus ověření. (Zařízení, které používají tokeny SAS jako mechanismus jejich ověření může být zřízen pouze prostřednictvím jednotlivých zápisu.) Chcete-li blokovaných zařízení, které má jednotlivých zápisu, můžete zakázat nebo odstranit jeho záznam zápisu: 

- Chcete-li dočasně blokovaných zařízení, můžete zakázat jeho registrace položku. 

    1. Přihlaste se k portálu Azure a klikněte na tlačítko **všechny prostředky** z nabídky na levé straně.
    2. Klikněte na zřizování službu, kterou chcete blokovaných ze zařízení v seznamu prostředků.
    3. Ve službě zřizování, klikněte na tlačítko **spravovat registrace**, pak vyberte **jednotlivých registrace** kartě.
    4. Klikněte na položku registrace pro zařízení, které chcete blokovaných ho otevřete. 
    5. Klikněte na tlačítko **zakázat** v dolní části seznamu položka registrace, pak klikněte na tlačítko **Uložit**.  

        ![Zakázat registrace jednotlivé položky na portálu](./media/how-to-revoke-device-access-portal/disable-individual-enrollment.png)
    
- Chcete-li trvale blokovaných zařízení, můžete odstranit jeho registrace položku.

    1. Přihlaste se k portálu Azure a klikněte na tlačítko **všechny prostředky** z nabídky na levé straně.
    2. Klikněte na zřizování službu, kterou chcete blokovaných ze zařízení v seznamu prostředků.
    3. Ve službě zřizování, klikněte na tlačítko **spravovat registrace**, pak vyberte **jednotlivých registrace** kartě.
    4. Zaškrtněte políčko vedle položky registrace pro zařízení, které chcete blokovaných. 
    5. Klikněte na tlačítko **odstranit** v horní části okna klikněte **Ano** potvrďte, že chcete odebrat registraci. 

        ![Odstranit položku jednotlivých registrace na portálu](./media/how-to-revoke-device-access-portal/delete-individual-enrollment.png)
    
    6. Po dokončení akce, zobrazí se zadání odebrat ze seznamu jednotlivých registrace.  

## <a name="blacklist-an-x509-intermediate-or-root-ca-certificate-using-an-enrollment-group"></a>Blokovaných X.509 zprostředkující nebo certifikát kořenové certifikační Autority pomocí skupinu registrace

X.509 – certifikáty jsou obvykle uspořádány řetěz certifikátů. Pokud bude k ohrožení bezpečnosti certifikátu v jakékoli fázi v řetězu, vztah důvěryhodnosti je poškozený a certifikát musí být zakázáno, aby zařízení dolů datový proud v řetězu certifikátů obsahující tento certifikát z se zřídí pomocí služby zřizování zařízení. Další informace o certifikátech X.509 a jak se používají službou zřizování najdete v tématu [certifikáty X.509](./concepts-security.md#x509-certificates). 

Skupinu registrace je záznam pro zařízení, která sdílejí společné mechanismus ověření certifikátů X.509 podepsaný zprostředkující stejné nebo kořenová certifikační Autorita. Zadání skupiny registrace je nakonfigurován s certifikátem X.509 přidružené mezilehlých nebo kořenové certifikační Autority, stejně jako všechny konfigurace hodnoty, například twin stavu a IoT připojení rozbočovače, které jsou sdíleny zařízení pomocí certifikátu v jejich certifikátu řetězec. Na blokovaných certifikátu, můžete zakázat nebo odstranit skupinu jeho registrace:

- Chcete-li dočasně blokovaných certifikátu, můžete zakázat jeho registrace skupiny. 

    1. Přihlaste se k portálu Azure a klikněte na tlačítko **všechny prostředky** z nabídky na levé straně.
    2. Klikněte na zřizování službu, kterou chcete blokovaných podpisový certifikát z v seznamu prostředků.
    3. Ve službě zřizování, klikněte na tlačítko **spravovat registrace**, pak vyberte **registrace skupiny** kartě.
    4. Klikněte na skupinu zápisu certifikátu, který chcete blokovaných ho otevřete.
    5. Klikněte na tlačítko **upravit skupinu** v levé horní části skupiny položky pro zápis.
    6. Položka registrace zakázat, vyberte **zakázat** na **povolit položku** přepínače a potom klikněte na **Uložit**.  

        ![Zakažte položky skupiny registrace na portálu](./media/how-to-revoke-device-access-portal/disable-enrollment-group.png)

    
- Chcete-li trvale blokovaných certifikátu, můžete odstranit jeho registrace skupiny.

    1. Přihlaste se k portálu Azure a klikněte na tlačítko **všechny prostředky** z nabídky na levé straně.
    2. Klikněte na zřizování službu, kterou chcete blokovaných ze zařízení v seznamu prostředků.
    3. Ve službě zřizování, klikněte na tlačítko **spravovat registrace**, pak vyberte **registrace skupiny** kartě.
    4. Zaškrtněte políčko vedle skupiny zápisu certifikátu, který chcete blokovaných. 
    5. Klikněte na tlačítko **odstranit** v horní části okna klikněte **Ano** k potvrzení odebrání skupiny pro zápis. 

        ![Odstranit položku skupiny registrace na portálu](./media/how-to-revoke-device-access-portal/delete-enrollment-group.png)

    6. Po dokončení akce, zobrazí se zadání odebrat ze seznamu skupin registrace.  

> [!NOTE]
> Pokud odstraníte skupinu zápisu certifikátu, zařízení, která mají tento certifikát v řetězu jejich certifikátů může být stále moci zaregistrovat Pokud skupinu povoleno registrace pro kořenový certifikát nebo jiný certifikát zprostředkující nacházejí v jejich certifikátu existuje řetězec.

## <a name="blacklist-specific-devices-in-an-enrollment-group"></a>Zakázaných konkrétní zařízení ve skupině registrace

Zařízení, které implementují mechanismus ověření X.509 používají k ověření zařízení řetěz certifikátů a privátní klíč. Když se zařízení připojí a ověřuje se službou zřizování zařízení, služby nejprve hledá jednotlivých zápisu odpovídající přihlašovací údaje zařízení před vyhledáváním skupiny registrace k určení, jestli zařízení může být zřízen. Pokud službu najde zakázané jednotlivých registrace pro zařízení, zabrání zařízení připojují, i v případě, že povoleno registrace skupiny pro středně pokročilé nebo kořenové certifikační Autority v řetězu certifikátů v zařízení existuje. Chcete-li blokovaných k jednotlivým zařízením ve skupině registrace, postupujte takto:

1. Přihlaste se k portálu Azure a klikněte na tlačítko **všechny prostředky** v nabídce vlevo.
2. Ze seznamu prostředků klikněte na tlačítko zřizování služba, která obsahuje skupinu registrace pro zařízení, které chcete blokovaných.
3. Ve službě zřizování, klikněte na tlačítko **spravovat registrace**, pak vyberte **jednotlivých registrace** kartě.
4. Klikněte **přidat** tlačítka v horní části. 
5. Vyberte **X.509** jako mechanismus zabezpečení pro zařízení a nahrání certifikátu zařízení. Toto je nainstalovaný na zařízení, která se používá pro generování certifikátů pro ověřování certifikát podepsaný držitelem koncové entity.
6. Zadejte **ID zařízení IoT Hub** pro zařízení. 
7. Položka registrace zakázat, vyberte **zakázat** na **povolit položku** přepínače. 
8. Klikněte na **Uložit**. Na úspěšné vytvoření registraci, měli byste vidět zařízení se zobrazí pod **jednotlivých registrace** kartě. 

    ![Zakázat registrace jednotlivé položky na portálu](./media/how-to-revoke-device-access-portal/disable-individual-enrollment.png)




