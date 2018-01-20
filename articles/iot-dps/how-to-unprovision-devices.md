---
title: "Postup zrušení zřízení zařízení zaregistrovaná službou Azure IoT Hub zařízení zřizování | Microsoft Docs"
description: "Postup zrušení zřízení zařízení zaregistrovaná službou distribučních bodů na portálu Azure"
services: iot-dps
keywords: 
author: JimacoMS
ms.author: v-jamebr
ms.date: 01/08/2018
ms.topic: article
ms.service: iot-dps
documentationcenter: 
manager: timlt
ms.devlang: na
ms.custom: mvc
ms.openlocfilehash: 83ed72c0f2eb342c372ef97e5443d60eab1e2174
ms.sourcegitcommit: 817c3db817348ad088711494e97fc84c9b32f19d
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/20/2018
---
# <a name="how-to-unprovision-devices-enrolled-by-your-provisioning-service"></a>Postup zrušení zřízení zařízení zaregistrovaná ve službě zřizování

Možná bude potřeba unprovision zařízení, které se zřizují prostřednictvím služby zřizování zařízení. Například může být za účelem prodeje nebo přesunout do jiné služby IoT hub zařízení, nebo ho může být ztráty, krádeže nebo jinému ohrožení zabezpečení. 

Obecně platí rušení zajišťování zařízení zahrnuje dva kroky:

1. Odvolání přístupu pro zařízení pro zřizování služby. V závislosti na tom, jestli chcete odvolat přístup dočasně nebo trvale, nebo v případě mechanismus ověření X.509, v hierarchii existující registrace skupiny můžete zakázat nebo odstranit položku, která registrace. 
 
   - Informace o tom odvolat přístup k zařízení pomocí portálu, najdete v části [odvolat přístup k zařízení](how-to-revoke-device-access-portal.md).
   - Informace o tom odvolat přístup k zařízení pomocí jedné z zřizování sady SDK služby prostřednictvím kódu programu, najdete v části [spravovat registrace zařízení pomocí služby SDK](how-to-manage-enrollments-sdks.md).

2. Zakázat nebo odstranit záznam zařízení v registru identit pro službu IoT Hub, kde byla zřízený. Další informace najdete v tématu [Správa identit zařízení](https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-devguide-identity-registry#disable-devices) v dokumentaci k Azure IoT Hub. 

Přesné kroky, které je třeba provést zrušit zřízení zařízení bude záviset na ověření mechanismus, který používá.

Zařízení, která používají ověření identity TPM nebo X.509 ověření pomocí certifikátu podepsaného svým držitelem typu list (bez řetězu certifikátů) jsou zřízené prostřednictvím položku jednotlivých registrace. Pro tato zařízení můžete odstranit položku trvale odvolat přístup zařízení ke službě zřizování nebo zakázat položky určené k odstranění nebo zakázání zařízení v registru identit s IoT dočasně odvolat jeho přístup a pak následnou akci centra, který byl připravené.

S X.509 ověření můžete zařízení také zřízené prostřednictvím skupiny služby zápisu. Registrace skupiny jsou nakonfigurovány s podpisový certifikát, buď středně pokročilé nebo kořenový certifikát certifikační Autority a řízení přístupu ke službě zřizování pro zařízení s certifikátu v jejich řetězu certifikátů. Další informace o skupinách registrace a certifikáty X.509 službou zřizování najdete v tématu [certifikáty X.509](concepts-security.md#x509-certificates). 

Pokud chcete zobrazit seznam zařízení, které se zřizují prostřednictvím skupinu zápisu, můžete zobrazit podrobnosti o registraci skupiny. Toto je snadný způsob, jak pochopit, které zřídil každé zařízení do služby IoT hub. Chcete-li zobrazit seznam zařízení: 

1. Přihlaste se k portálu Azure a klikněte na tlačítko **všechny prostředky** v levé nabídce.
2. Klikněte na tlačítko zřizování služby v seznamu prostředků.
3. Ve službě zřizování, klikněte na tlačítko **spravovat registrace**, pak vyberte **registrace skupiny** kartě.
4. Klikněte na skupinu registrace ho otevřete.

   ![Zobrazit položku skupiny registrace na portálu](./media/how-to-unprovision-devices/view-enrollment-group.png)

Registrace skupiny pro existují dva scénáře vzít v úvahu:

- Chcete-li zrušit zřízení všechna zařízení, které se zřizují prostřednictvím registrace skupiny, třeba nejdříve zakázat skupiny registrace můžete blokovaných jeho podpisový certifikát. Pak můžete použít seznam zřízené zařízení pro tuto skupinu registrace zakázat nebo odstranit každé zařízení z registru identit jeho odpovídající centra IoT. Po zakázání nebo odstranění všechna zařízení z jejich odpovídajících centra IoT, můžete volitelně můžete odstranit skupinu registrace. Nezapomeňte však, že pokud odstranit skupinu registrace a je skupina povoleno registrace pro podpisový certifikát nacházejí v řetězu certifikátů jednoho nebo více zařízení, tato zařízení znovu zaregistrovat. 
- Chcete-li zrušit zřízení jedno zařízení ze skupiny pro zápis, musíte nejdřív vytvořit zakázaný jednotlivých registrace pro svůj certifikát typu list (zařízení). To odvolá přístup ke službě zřizování pro toto zařízení, přičemž dál umožňuje přístup pro jiná zařízení, které mají podpisový certifikát skupiny pro registraci v jejich řetězu. Pak můžete použít seznam zařízení, zřízené v podrobnostech registrace skupiny k vyhledání zřízenou zařízení do služby IoT hub a zakázat nebo odstranit z registru identit v daném rozbočovači. Neodstraňujte zakázané jednotlivých registrace zařízení. Díky tomu vám umožní zařízení znovu zaregistrovat prostřednictvím registrace skupiny. 










