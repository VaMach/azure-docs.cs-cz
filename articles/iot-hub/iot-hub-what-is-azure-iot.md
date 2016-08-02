<properties
 pageTitle="Řešení Azure pro Internet věcí | Microsoft Azure"
 description="Přehled technologie IoT na platformě Azure, včetně ukázkové architektury řešení a informací o tom, jak souvisí se službou Azure IoT Hub, sadami SDK a předkonfigurovanými řešeními"
 services="iot-hub"
 documentationCenter=""
 authors="dominicbetts"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="na"
 ms.topic="get-started-article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="04/29/2016"
 ms.author="dobett"/>

[AZURE.INCLUDE [iot-azure-and-iot](../../includes/iot-azure-and-iot.md)]

## Další kroky

Azure IoT Hub je služba Azure, která umožňuje zabezpečenou a spolehlivou obousměrnou komunikaci mezi back-endem vaší aplikaci a miliony zařízení. Back-endu vaší aplikace to umožňuje ze zařízení přijímat škálovaná telemetrická data, směrovat tato data do zpracovatele událostí datového proudu a také posílat příkazy typu cloud-zařízení do konkrétních zařízení. Pomocí služby IoT Hub můžete implementovat vlastní back-end řešení. Kromě toho služba IoT Hub obsahuje registr identit zařízení, který slouží ke zřizování zařízení, jejich zabezpečených přihlašovacích údajů a práv k připojení do služby. Další informace naleznete v tématu:

- [Co je služba IoT Hub?][lnk-iot-hub]
- [Začínáme se službou IoT Hub][lnk-getstarted]
- [Přehled správy zařízení ve službě Azure IoT Hub][lnk-device-management]

K implementaci klientských aplikací v celé řadě hardwarových platforem a operačních systémů zařízení můžete použít sady SDK zařízení IoT. Sady SDK zařízení IoT zahrnují knihovny, které usnadňují odesílání telemetrických dat do služby IoT Hub a příjem příkazů typu cloud-zařízení. Při používání sad SDK si ke komunikaci se službou IoT Hub můžete vybrat z řady síťových protokolů. Další informace naleznete v tématu [informace sadách SDK pro zařízení][lnk-device-sdks].

Možná by vás také mohla zajímat sada [Azure IoT Suite][lnk-iot-suite], což je kolekce předkonfigurovaných řešení. Sada IoT vám umožní rychle začít vytvářet a škálovat projekty IoT tak, abyste vyřešili běžné scénáře IoT – například vzdálené monitorování, správa assetu či prediktivní údržba.

[lnk-getstarted]: iot-hub-csharp-csharp-getstarted.md
[lnk-device-sdks]: https://github.com/Azure/azure-iot-sdks/blob/master/readme.md
[lnk-iot-hub]: iot-hub-what-is-iot-hub.md
[lnk-iot-suite]: https://azure.microsoft.com/documentation/suites/iot-suite/
[lnk-iotdev]: https://azure.microsoft.com/develop/iot/
[lnk-device-management]: iot-hub-device-management-overview.md


<!--HONumber=Jun16_HO2-->


