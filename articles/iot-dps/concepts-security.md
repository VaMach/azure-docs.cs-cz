---
title: "Koncepty zabezpečení ve službě Azure IoT Hub zařízení zřizování Service | Microsoft Docs"
description: "Popisuje zabezpečení zřizování koncepty, které jsou specifické pro zařízení s služby zřizování zařízení a služby IoT Hub"
services: iot-dps
keywords: 
author: nberdy
ms.author: nberdy
ms.date: 09/05/2017
ms.topic: article
ms.service: iot-dps
documentationcenter: 
manager: timlt
ms.devlang: na
ms.custom: mvc
ms.openlocfilehash: 3ccbaaf55d2bdfedffcdb5ca069798328e2d75fd
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="iot-hub-device-provisioning-service-security-concepts"></a>Koncepty zabezpečení služby zřizování zařízení IoT Hub 

Služba zřizování zařízení IoT Hub je služba Pomocník pro IoT Hub, který použijete ke konfiguraci zařízení nula touch zřizování do zadané služby IoT hub. Se službou zřizování zařízení můžete zřídit miliony zařízení zabezpečené a škálovatelné způsobem. Tento článek nabízí přehled *zabezpečení* koncepty, které jsou součástí zřizování zařízení. Tento článek je relevantní pro všechny osoby, které se účastní Příprava zařízení pro nasazení.

## <a name="attestation-mechanism"></a>Ověření mechanismu

Ověření mechanismu je metoda použitá pro potvrzení identity zařízení. Ověření mechanismu se taky hodí do seznamu registrace, který informuje zřizování služby, kterou metodu ověření pro použití s daným zařízením.

> [!NOTE]
> IoT Hub používá "schéma ověřování" pro podobný koncept v této službě.

Služba zřizování zařízení podporuje dva způsoby ověření:
* **Certifikáty X.509** podle standardní tok ověřování certifikátu X.509.
* **Tokeny SAS** podle nonce výzvu pomocí čipu TPM standard pro klíče. Nevyžaduje fyzické TPM na zařízení, ale služba očekává, že ověřit identitu pomocí ověřovacího klíče na [specifikace TPM](https://trustedcomputinggroup.org/work-groups/trusted-platform-module/).

## <a name="hardware-security-module"></a>Modul hardwarového zabezpečení

Modul hardwarového zabezpečení nebo modulu hardwarového zabezpečení, se používá pro zabezpečené úložiště hardwarové zařízení tajemství a je nejbezpečnější formu tajný úložiště. X.509 – certifikáty a tokeny SAS může být uložen v modulu hardwarového zabezpečení. Moduly hardwarového zabezpečení lze použít s obou mechanismů ověření zřizování podporuje.

> [!TIP]
> Důrazně doporučujeme pomocí modulu hardwarového zabezpečení zařízení na zařízení bezpečně uložit tajných klíčů.

Tajné klíče zařízení může být taky uložené v softwaru (paměť), ale je méně bezpečné formu úložiště než modulu hardwarového zabezpečení.

## <a name="trusted-platform-module-tpm"></a>Trusted Platform Module (TPM)

Čip TPM mohou odkazovat na standard týkající se bezpečného ukládání klíče, které slouží k ověření platformu nebo mohou odkazovat na rozhraní vstupně-výstupních operací používají k interakci s moduly implementace standardní. Čipy TPM může existovat jako diskrétní hardware, integrované hardware, založené na firmwaru, nebo na základě softwaru. Další informace o [čipy TPM a TPM ověření](/windows-server/identity/ad-ds/manage/component-updates/tpm-key-attestation). Služba zřizování zařízení podporuje jenom čip TPM 2.0.

## <a name="endorsement-key"></a>Ověřovací klíč

Ověřovací klíč je asymetrický klíč nachází v čipu TPM, který byl aplikován na výrobní čas a je jedinečný pro každý čip TPM. Ověřovací klíč nelze změnit ani odebrat. Soukromá část ověřovacího klíče nikdy vydání mimo čipu TPM, přičemž veřejnou část ověřovacího klíče se používá k rozpoznání pravosti čipu TPM. Další informace o [ověřovací klíč](https://technet.microsoft.com/library/cc770443(v=ws.11).aspx).

## <a name="storage-root-key"></a>Klíč kořenového úložiště

Kořenový klíč úložiště je uložená v čipu TPM a slouží k ochraně klíčů TPM, které jsou vytvořené pomocí aplikací, aby tyto klíče nejde použít bez čipu TPM. Kořenový klíč úložiště se vygeneruje, když převzít vlastnictví čipu TPM; Když zrušíte zaškrtnutí čipu TPM, takže nového uživatele můžete převzít vlastnictví, je generována nového kořenového klíče úložiště. Další informace o [úložiště kořenový klíč](https://technet.microsoft.com/library/cc753560(v=ws.11).aspx).

## <a name="root-certificate"></a>Kořenový certifikát

Kořenový certifikát je typ certifikátu X.509 představující certifikační autority a je podepsaný svým držitelem. Je konce řetězu certifikátů.

## <a name="intermediate-certificate"></a>Zprostředkující certifikát

Zprostředkující certifikát je certifikát X.509, který byl podepsán kořenový certifikát (nebo jiný certifikát kořenový certifikát ve svém řetězu) a který se používá k podepisování certifikátů typu list.

## <a name="leaf-certificate"></a>Certifikát listu

Certifikát listu, nebo certifikát koncové entity, se používá k identifikaci držitele certifikátu a kořenový certifikát má ve svém řetězu certifikátů. Listu certifikát se používá k podepisování jiných certifikátů.
