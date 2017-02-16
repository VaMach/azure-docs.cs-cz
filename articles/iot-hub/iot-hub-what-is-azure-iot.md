---
title: "Řešení Azure pro Internet věcí | Dokumentace Microsoftu"
description: "Přehled ukázkové architektury řešení IoT a informace o tom, jak souvisí se zařízeními, službou Azure IoT Hub, sadami SDK pro zařízení Azure IoT, sadami SDK pro služby Azure IoT a dalšími službami Azure."
services: iot-hub
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: a859e379-dca7-42fa-bdf6-1125c86ad140
ms.service: iot-hub
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/04/2017
ms.author: dobett
translationtype: Human Translation
ms.sourcegitcommit: e223d0613cd48994315451da87e6b7066585bdb6
ms.openlocfilehash: 36e4afb54485c12b39349124e241a2d74617702c


---
[!INCLUDE [iot-azure-and-iot](../../includes/iot-azure-and-iot.md)]

## <a name="next-steps"></a>Další kroky
Azure IoT Hub je služba Azure, která umožňuje zabezpečenou a spolehlivou obousměrnou komunikaci mezi back-endem řešení a miliony zařízení. Umožňuje back-endu řešení:

* Přijímat škálovaná telemetrická data z vašich zařízení.
* Směrovat data z vašich zařízení do zpracovatele událostí datového proudu.
* Přijímat nahrávání souborů ze zařízení.
* Odesílat zprávy typu cloud-zařízení na konkrétní zařízení.

Pomocí služby IoT Hub můžete implementovat vlastní back-end řešení. Kromě toho služba IoT Hub obsahuje registr identit, který slouží ke zřizování zařízení, jejich zabezpečených přihlašovacích údajů a práv pro připojení ke službě IoT Hub. Další informace o službě IoT Hub najdete v části [Co je IoT Hub?][lnk-iot-hub].

V článku [Přehled správy zařízení ve službě IoT Hub][lnk-device-management] zjistíte, jak Azure IoT Hub umožňuje pomocí správy zařízení založené na standardech provádět vzdálenou správu, konfiguraci a aktualizaci vašich zařízení.

K implementaci klientských aplikací v celé řadě hardwarových platforem a operačních systémů zařízení můžete použít sady SDK pro zařízení Azure IoT. Sady SDK pro zařízení zahrnují knihovny, které usnadňují odesílání telemetrických dat do služby IoT Hub a příjem zpráv typu cloud-zařízení. Při používání sad SDK pro zařízení si ke komunikaci se službou IoT Hub můžete vybrat z řady síťových protokolů. Další informace najdete v tématu [informace sadách SDK pro zařízení][lnk-device-sdks].

Pokud chcete začít s psaním kódu a spouštěním ukázek, přečtěte si kurz [Začínáme se službou IoT Hub][lnk-getstarted].

Možná by vás také mohla zajímat sada [Azure IoT Suite][lnk-iot-suite], což je kolekce předkonfigurovaných řešení. Sada IoT vám umožní rychle začít vytvářet a škálovat projekty IoT tak, abyste vyřešili běžné scénáře IoT – například vzdálené monitorování, správa assetu či prediktivní údržba.

[lnk-getstarted]: iot-hub-csharp-csharp-getstarted.md
[lnk-device-sdks]: https://github.com/Azure/azure-iot-sdks
[lnk-iot-hub]: iot-hub-what-is-iot-hub.md
[lnk-iot-suite]: https://azure.microsoft.com/documentation/suites/iot-suite/
[lnk-iotdev]: https://azure.microsoft.com/develop/iot/
[lnk-device-management]: iot-hub-device-management-overview.md



<!--HONumber=Dec16_HO1-->


