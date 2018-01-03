---
title: "Spravovat registrace zařízení s Azure zařízení zřizování služby SDK | Microsoft Docs"
description: "Postup registrace zařízení IoT Hub zařízení zřizování služby pomocí sady SDK služby správy"
services: iot-dps
keywords: 
author: yzhong94
ms.author: yizhon
ms.date: 12/01/2017
ms.topic: article
ms.service: iot-dps
documentationcenter: 
manager: arjmands
ms.devlang: na
ms.custom: mvc
ms.openlocfilehash: 82da49924e71a38ca557f244f2830e1da45826b1
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/21/2017
---
# <a name="how-to-manage-device-enrollments-with-azure-device-provisioning-service-sdks"></a>Jak spravovat registrace zařízení s Azure zařízení zřizování služby SDK
A *registrace zařízení* vytvoří záznam jednoho zařízení nebo skupinu zařízení, které může v určitém okamžiku zaregistrovat službu zřizování zařízení. Záznam zápisu obsahuje počáteční požadované konfigurace pro zařízení v rámci této registrace, včetně požadované služby IoT hub. Tento článek ukazuje, jak spravovat registrace zařízení služby zřizování programově pomocí sady Azure IoT zřizování služby SDK.  Sady SDK jsou k dispozici na Githubu v úložišti stejné jako SDK služby Azure IoT.

## <a name="samples"></a>Ukázky
Tento článek zkontroluje nejvyšších úrovní koncepty pro správu registrace zařízení služby zřizování programově pomocí sady Azure IoT zřizování služby SDK.  Přesný volání rozhraní API se může lišit kvůli jazyk rozdíly.  Přečtěte si ukázky, které poskytujeme na Githubu podrobnosti:
* [Ukázky Java zřizování klienta služby](https://github.com/Azure/azure-iot-sdk-java/tree/master/provisioning/provisioning-samples)
* [Ukázky zřizování klienta služby Node.js](https://github.com/Azure/azure-iot-sdk-node/tree/master/provisioning/service/samples)

## <a name="prerequisites"></a>Požadavky
* Připojovací řetězec z instance služby zřizování zařízení
* Artefakty zabezpečení zařízení:
    * [**ČIP TPM**](https://docs.microsoft.com/en-us/azure/iot-dps/concepts-security):
        * Jednotlivé registrace: ID registrace a TPM ověřovací klíč z fyzického zařízení nebo z simulátoru TPM.
        * Registrace skupiny se nevztahuje na ověření čipu TPM.
    * [**X.509**](https://docs.microsoft.com/en-us/azure/iot-dps/concepts-security):
        * Jednotlivé registrace: [listu certifikát](https://docs.microsoft.com/en-us/azure/iot-dps/concepts-security#leaf-certificate) z fyzického zařízení nebo emulátoru ROZČLENĚNÍ.
        * Registrace skupiny: [kořenový certifikát](https://docs.microsoft.com/en-us/azure/iot-dps/concepts-security#root-certificate) nebo [zprostředkující certifikát](https://docs.microsoft.com/en-us/azure/iot-dps/concepts-security#intermediate-certificate)používané k vytvoření certifikátu zařízení na fyzické zařízení.  Lze ji také vygenerovat z emulátoru ROZČLENĚNÍ.

## <a name="create-a-device-enrollment"></a>Vytvoření registrace zařízení

Existují dva způsoby, kterými se můžete zaregistrovat svoje zařízení v zřizování služby:

* **Registrace skupiny** je záznam pro skupinu zařízení, které sdílejí společný mechanismus ověření certifikátů X.509, podepsaný [kořenový certifikát](https://docs.microsoft.com/en-us/azure/iot-dps/concepts-security#root-certificate) nebo [zprostředkující certifikát ](https://docs.microsoft.com/en-us/azure/iot-dps/concepts-security#intermediate-certificate). Doporučujeme používat skupinu registrace pro velký počet zařízení, které sdílejí požadované počáteční konfigurace, nebo pro zařízení všechny má stejné klienta. Všimněte si, že můžete registrovat jenom zařízení, která používají tento mechanismus ověření X.509 jako *registrace skupiny*. 

    Můžete vytvořit skupinu registrace pomocí sady SDK následující tento pracovní postup:

    1. Pro skupinu pro zápis používá mechanismus ověření kořenový certifikát X.509.  Volání rozhraní API sady SDK služby ```X509Attestation.createFromRootCertificate``` kořenový certifikát k vytvoření ověření pro zápis.  Kořenový certifikát X.509 je k dispozici v souboru PEM nebo jako řetězec.
    1. Vytvořte novou ```EnrollmentGroup``` proměnné pomocí ```attestation``` vytvořený a jedinečný ```enrollmentGroupId```.  Volitelně můžete nastavit parametry, jako je ```Device ID```, ```IoTHubHostName```, ```ProvisioningStatus```.
    2. Volání rozhraní API sady SDK služby ```createOrUpdateEnrollmentGroup``` v back-end aplikace s ```EnrollmentGroup``` k vytvoření skupiny pro zápis.

* **Jednotlivých registrace** je záznam pro jedno zařízení, která může zaregistrovat. Jednotlivé registrace používat certifikáty X.509 nebo tokeny SAS (ve fyzických nebo virtuálních čipu TPM), jak mechanismů ověření. Doporučujeme používat jednotlivé registrace zařízení, které vyžadují jedinečné počáteční konfigurace, nebo pro zařízení, které lze použít pouze tokeny SAS prostřednictvím TPM nebo virtuální čipu TPM jako mechanismus ověření. Jednotlivé registrace může mít požadovaný IoT hub ID zařízení v zadané.

    Jednotlivé zápisu můžete vytvořit pomocí sady SDK následující tento pracovní postup:
    
    1. Zvolte vaše ```attestation``` mechanismus, který může být TPM nebo X.509.
        1. **Čip TPM**: pomocí ověřovacího klíče z fyzického zařízení nebo z simulátoru čipu TPM jako vstup, můžete volat rozhraní API sady SDK služby ```TpmAttestation``` k vytvoření ověření pro zápis. 
        2. **X.509**: jako vstup pomocí klientského certifikátu, můžete volat rozhraní API sady SDK služby ```X509Attestation.createFromClientCertificate``` k vytvoření ověření pro zápis.
    2. Vytvořte novou ```IndividualEnrollment``` proměnné s použitím ```attestation``` vytvořený a jedinečný ```registrationId``` jako vstup, která je na vašem zařízení nebo vytvořena z simulátoru TPM.  Volitelně můžete nastavit parametry, jako je ```Device ID```, ```IoTHubHostName```, ```ProvisioningStatus```.
    3. Volání rozhraní API sady SDK služby ```createOrUpdateIndividualEnrollment``` v back-end aplikace s ```IndividualEnrollment``` vytvořit jednotlivé zápisu.

Po úspěšném vytvoření zápisu službu zřizování zařízení by vrátit výsledek registrace.

Tento pracovní postup ukazují [ukázky](#samples).

## <a name="update-an-enrollment-entry"></a>Aktualizovat položku registrace

Po vytvoření záznam zápisu, můžete při aktualizaci registrace.  Možných scénářů zahrnovat aktualizace požadované vlastnosti, aktualizace metodu ověření nebo odvolání přístupu k zařízení.  Existují různé rozhraní API pro jednotlivé registrace a registrace ve skupině, ale žádný rozdíl pro ověření mechanismu.

Můžete aktualizovat položku registrace následující tento pracovní postup:
* **Jednotlivé registrace**:
    1. Získat nejnovější registraci ze služby zřizování první pomocí rozhraní API sady SDK služby ```getIndividualEnrollment```.
    2. Upravte parametr nejnovější zápisu podle potřeby. 
    3. Pomocí nejnovější registrace volání rozhraní API sady SDK služby ```createOrUpdateIndividualEnrollment``` aktualizovat zadání registrace.
* **Registrace skupiny**:
    1. Získat nejnovější registraci ze služby zřizování první pomocí rozhraní API sady SDK služby ```getEnrollmentGroup```.
    2. Upravte parametr nejnovější zápisu podle potřeby.
    3. Pomocí nejnovější registrace volání rozhraní API sady SDK služby ```createOrUpdateEnrollmentGroup``` aktualizovat zadání registrace.

Tento pracovní postup ukazují [ukázky](#samples).

## <a name="remove-an-enrollment-entry"></a>Odebrat záznam zápisu

* **Jednotlivé registrace** odstraněním voláním rozhraní API sady SDK služby ```deleteIndividualEnrollment``` pomocí ```registrationId```.
* **Registrace skupiny** odstraněním voláním rozhraní API sady SDK služby ```deleteEnrollmentGroup``` pomocí ```enrollmentGroupId```.

Tento pracovní postup ukazují [ukázky](#samples).

## <a name="bulk-operation-on-individual-enrollments"></a>Hromadné operace na jednotlivé registrace

Můžete provádět hromadné operace vytvoření, aktualizace nebo odebrání více jednotlivých přihlášení následující tento pracovní postup:

1. Vytvoření proměnné, která obsahuje více ```IndividualEnrollment```.  Implementace této proměnné se liší pro každý jazyk.  Zkontrolujte hromadné operace ukázce na Githubu podrobnosti.
2. Volání rozhraní API sady SDK služby ```runBulkOperation``` s ```BulkOperationMode``` pro požadovanou operaci a vaše proměnná pro jednotlivé registrace. Jsou podporovány čtyři režimy: vytvářet, aktualizovat, updateIfMatchEtag a odstraňovat.

Po úspěšném provedení operace, službu zřizování zařízení by vrácení výsledku hromadné operace.

Tento pracovní postup ukazují [ukázky](#samples).
