---
title: "Pochopení zasílání zpráv Azure IoT Hub | Microsoft Docs"
description: "Příručka vývojáře - zařízení cloud a cloud zařízení zasílání zpráv službou IoT Hub. Obsahuje informace o formáty zpráv a protokoly podporované komunikace."
services: iot-hub
documentationcenter: .net
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 3fc5f1a3-3711-4611-9897-d4db079b4250
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/14/2017
ms.author: dobett
ms.openlocfilehash: 4a7f545f1af2904f47808fe8b1d7d3aab3afff84
ms.sourcegitcommit: 933af6219266cc685d0c9009f533ca1be03aa5e9
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/18/2017
---
# <a name="device-to-cloud-and-cloud-to-device-messaging-with-iot-hub"></a>Zařízení cloud a cloud zařízení zasílání zpráv s centrem IoT

Pomocí zasílání zpráv pro komunikaci se vaše zařízení pomocí služby IoT Hub:

* Odesílání [zařízení cloud] [ lnk-d2c] zpráv ze zařízení do řešení back-end.
* Odesílání [cloud zařízení] [ lnk-c2d] zprávy z řešení back-end pro vaše zařízení.

Základní vlastnosti funkce zasílání zpráv služby IoT Hub jsou spolehlivost a odolnost zpráv. Tyto vlastnosti umožňují odolnost proti přerušované připojení na straně zařízení a načíst špičky v případě zpracování na straně cloudu. IoT Hub implementuje *alespoň jednou* doručení zaručuje pro zasílání zpráv typu zařízení cloud a cloud zařízení.

Úvod k funkcím služby IoT Hub, najdete v článcích [Azure a Internet věcí] [ lnk-azure-iot] a [Přehled služby Azure IoT Hub][lnk-iot-hub-overview].

## <a name="when-to-use-iot-hub-messaging"></a>Kdy použít Centrum IoT zasílání zpráv

Jednosměrné oznámení do aplikace pro zařízení pomocí zpráv typu zařízení cloud pro odesílání výstrahy a časové řady telemetrie z vaší aplikace zařízení a zprávy typu cloud zařízení.

* Odkazovat na [pokyny komunikace zařízení cloud] [ lnk-d2c-guidance] Pokud máte pochybnosti mezi pomocí zpráv typu zařízení cloud, hlášen vlastnostech nebo nahrávání souborů.
* Odkazovat na [Cloud zařízení komunikace pokyny] [ lnk-c2d-guidance] Pokud nejistých mezi pomocí zpráv typu cloud zařízení, požadované vlastnosti nebo metody direct.

## <a name="next-steps"></a>Další kroky

* Další informace o IoT Hub [zasílání zpráv typu zařízení cloud][lnk-d2c].
* Další informace o IoT Hub [zasílání zpráv typu cloud zařízení][lnk-c2d].

[lnk-azure-iot]: iot-hub-what-is-azure-iot.md
[lnk-iot-hub-overview]: iot-hub-what-is-iot-hub.md
[lnk-d2c]: iot-hub-devguide-messages-d2c.md
[lnk-c2d]: iot-hub-devguide-messages-c2d.md
[lnk-c2d-guidance]: iot-hub-devguide-c2d-guidance.md
[lnk-d2c-guidance]: iot-hub-devguide-d2c-guidance.md