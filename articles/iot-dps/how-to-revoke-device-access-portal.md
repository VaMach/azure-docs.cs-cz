---
title: "Správa přístupu k zařízení služby Azure IoT Hub zařízení zřizování | Microsoft Docs"
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
ms.openlocfilehash: 12aebf3a56aa7469a765ab6fc67aa65b254db71a
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/01/2018
---
# <a name="revoke-device-access-to-your-provisioning-service-in-the-azure-portal"></a>Odebrat zařízení přístup ke službě zřizování na portálu Azure

Správné správu přihlašovacích údajů, zařízení je zásadní pro systémy vysokou profil jako řešení IoT. Osvědčeným postupem pro tyto systémy je mít zrušte plán o tom, jak odvolat přístup k zařízení při jejich přihlašovacích údajů, zda token sdílený přístupový podpis (SAS) nebo certifikátu X.509, může dojít k ohrožení. Tento článek popisuje, jak odvolat přístup k zařízením v kroku zřizování.

Další informace o odvolání přístupu zařízení do služby IoT hub po zřídil zařízení najdete v tématu [zakažte zařízení](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-identity-registry#disable-devices).

> [!NOTE] 
> Pamatujte na zařízení, která odvolání přístupu pro zásady opakování. Například zařízení, které má zásady opakování nekonečné může trvale pokusit zaregistrovat službu zřizování. Tato situace spotřebovává prostředky služby a pravděpodobně má vliv na výkon.

## <a name="blacklist-devices-by-using-an-individual-enrollment-entry"></a>Zařízení zakázaných pomocí položku jednotlivých registrace

Jednotlivé registrace platí pro jedno zařízení a můžete použít certifikáty X.509 nebo tokeny SAS (ve fyzických nebo virtuálních čipu TPM) jako mechanismus ověření. (Zařízení, které používají tokeny SAS jako mechanismus jejich ověření můžete zřídit pouze prostřednictvím jednotlivých zápisu.) Chcete-li blokovaných zařízení, které má jednotlivých zápisu, můžete zakázat nebo odstranit jeho záznam zápisu. 

Dočasně blokovaných zakázáním jeho položku registrace zařízení: 

1. Přihlaste se k Azure portálu a vyberte možnost **všechny prostředky** v levé nabídce.
2. V seznamu prostředků vyberte službu, kterou chcete blokovaných zařízení z zřizování.
3. Ve službě zřizování vyberte **spravovat registrace**a pak vyberte **jednotlivých registrace** kartě.
4. Vyberte položku registrace pro zařízení, které chcete blokovaných. 
5. Vyberte **zakázat** na **povolit položku** přepínače a potom vyberte **Uložit**.  

   ![Zakázat registrace jednotlivé položky na portálu](./media/how-to-revoke-device-access-portal/disable-individual-enrollment.png)
    
Chcete-li trvale blokovaných zařízení odstraněním jeho registrace položku:

1. Přihlaste se k Azure portálu a vyberte možnost **všechny prostředky** v levé nabídce.
2. V seznamu prostředků vyberte službu, kterou chcete blokovaných zařízení z zřizování.
3. Ve službě zřizování vyberte **spravovat registrace**a pak vyberte **jednotlivých registrace** kartě.
4. Zaškrtněte políčko u položky registrace pro zařízení, které chcete blokovaných. 
5. Vyberte **odstranit** v horní části okna a potom vyberte **Ano** potvrďte, že chcete odebrat registraci. 

   ![Odstranit položku jednotlivých registrace na portálu](./media/how-to-revoke-device-access-portal/delete-individual-enrollment.png)
    
Po dokončení procesu, měli byste vidět zadání odebrat ze seznamu jednotlivých registrace.  

## <a name="blacklist-an-x509-intermediate-or-root-ca-certificate-by-using-an-enrollment-group"></a>Blokovaných X.509 zprostředkující nebo certifikát kořenové certifikační Autority pomocí skupinu registrace

X.509 – certifikáty jsou obvykle uspořádány řetěz certifikátů. Pokud bude k ohrožení bezpečnosti certifikátu v jakékoli fázi v řetězu, vztah důvěryhodnosti se přeruší. Certifikát musí být zakázáno zabránit zřizování zařízení po proudu v řetězu certifikátů obsahující tento certifikát služby zřizování zařízení. Další informace o certifikátech X.509 a jak se používají službou zřizování najdete v tématu [certifikáty X.509](./concepts-security.md#x509-certificates). 

Skupinu registrace je záznam pro zařízení, která sdílejí společné mechanismus ověření certifikátů X.509 podepsaný zprostředkující stejné nebo kořenová certifikační Autorita. Zadání skupiny registrace je nakonfigurován s certifikátem X.509 přidružené mezilehlých nebo kořenová certifikační Autorita. Položka je také nakonfigurovaný se žádné konfigurace hodnoty, například twin stavu a IoT připojení rozbočovače, které jsou sdíleny zařízení pomocí certifikátu v jejich řetězu certifikátů. Chcete-li blokovaných certifikát, můžete zakázat nebo odstranit skupinu jeho registrace.

Chcete-li dočasně blokovaných certifikát zakázáním jeho registrace skupiny: 

1. Přihlaste se k Azure portálu a vyberte možnost **všechny prostředky** v levé nabídce.
2. V seznamu prostředků vyberte zřizování službu, kterou chcete blokovaných podpisový certifikát z.
3. Ve službě zřizování vyberte **spravovat registrace**a pak vyberte **registrace skupiny** kartě.
4. Vyberte skupinu zápisu pro certifikát, který chcete blokovaných.
5. V položce skupiny registrace, vyberte **upravit skupinu**.
6. Vyberte **zakázat** na **povolit položku** přepínače a potom vyberte **Uložit**.  

   ![Zakažte položky skupiny registrace na portálu](./media/how-to-revoke-device-access-portal/disable-enrollment-group.png)

    
Chcete-li trvale blokovaných certifikát odstraněním skupiny jeho registrace:

1. Přihlaste se k Azure portálu a vyberte možnost **všechny prostředky** v levé nabídce.
2. V seznamu prostředků vyberte službu, kterou chcete blokovaných zařízení z zřizování.
3. Ve službě zřizování vyberte **spravovat registrace**a pak vyberte **registrace skupiny** kartě.
4. Zaškrtněte políčko vedle skupiny zápisu pro certifikát, který chcete blokovaných. 
5. Vyberte **odstranit** v horní části okna a potom vyberte **Ano** k potvrzení odebrání skupiny pro zápis. 

   ![Odstranit položku skupiny registrace na portálu](./media/how-to-revoke-device-access-portal/delete-enrollment-group.png)

Po dokončení procesu, měli byste vidět zadání odebrat ze seznamu skupin registrace.  

> [!NOTE]
> Pokud odstraníte skupinu zápisu certifikátu, zařízení, které mají certifikát v řetězu jejich certifikátů může být stále moci zaregistrovat Pokud skupinu povoleno registrace pro kořenový certifikát nebo jiný certifikát zprostředkující nacházejí v jejich certifikátu existuje řetězec.

## <a name="blacklist-specific-devices-in-an-enrollment-group"></a>Zakázaných konkrétní zařízení ve skupině registrace

Zařízení, které implementují mechanismus ověření X.509 používají k ověření zařízení řetěz certifikátů a privátní klíč. Když se zařízení připojí a ověřuje se službou zřizování zařízení, služby nejprve hledá jednotlivých zápisu odpovídající přihlašovací údaje zařízení. Služba pak prohledá registrace skupiny k určení, jestli zařízení může být zřízen. Pokud službu najde zakázané jednotlivých registrace pro zařízení, brání zařízení v připojení. Služba brání připojení i v případě, že povoleno registrace skupiny pro středně pokročilé nebo kořenové certifikační Autority v řetězu certifikátů v zařízení existuje. 

Chcete-li blokovaných k jednotlivým zařízením ve skupině registrace, postupujte takto:

1. Přihlaste se k Azure portálu a vyberte možnost **všechny prostředky** v levé nabídce.
2. V seznamu zdrojů vyberte zřizování služba, která obsahuje skupinu registrace pro zařízení, které chcete blokovaných.
3. Ve službě zřizování vyberte **spravovat registrace**a pak vyberte **jednotlivých registrace** kartě.
4. Vyberte **přidat** tlačítka v horní části. 
5. Vyberte **X.509** jako mechanismus zabezpečení pro zařízení a nahrání certifikátu zařízení. Toto je nainstalovaný v zařízení certifikát podepsaný držitelem koncové entity. Zařízení se používá pro generování certifikátů pro ověřování.
6. Pro **ID zařízení IoT Hub**, zadejte ID zařízení. 
7. Vyberte **zakázat** na **povolit položku** přepínače a potom vyberte **Uložit**. 

   ![Zakázat registrace jednotlivé položky na portálu](./media/how-to-revoke-device-access-portal/disable-individual-enrollment.png)

Když vytvoříte úspěšně registraci, měli byste vidět zobrazí na zařízení **jednotlivých registrace** kartě.


