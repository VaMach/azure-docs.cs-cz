---
title: "Porovnání Azure IoT Hub do služby Azure Event Hubs | Microsoft Docs"
description: "Porovnání služby IoT Hub a Event Hubs Azure zvýraznění určité rozdíly a případy použití. Porovnání obsahuje podporované protokoly, správu zařízení, monitorování, a nahrávání souborů."
services: iot-hub
documentationcenter: 
author: fsautomata
manager: timlt
editor: 
ms.assetid: aeddea62-8302-48e2-9aad-c5a0e5f5abe9
ms.service: iot-hub
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/24/2017
ms.author: elioda
ms.openlocfilehash: b515e05d16dda83c7d865113d5d3578c44be084f
ms.sourcegitcommit: 933af6219266cc685d0c9009f533ca1be03aa5e9
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/18/2017
---
# <a name="comparison-of-azure-iot-hub-and-azure-event-hubs"></a>Porovnání Azure IoT Hub a Azure Event Hubs
Jedním z případů se využívá pro IoT Hub je ke shromažďování telemetrických dat ze zařízení. Z tohoto důvodu IoT Hub je často ve srovnání s [Azure Event Hubs][Azure Event Hubs]. Jako IoT Hub Event Hubs je služba, která umožňuje příchozího událostí a telemetrie do cloudu v masivním měřítku, s nízkou latencí a vysokou spolehlivostí zpracování událostí.

Ale služeb mají mnoho rozdíly, které jsou podrobně popsané v následující tabulce:

| Oblast | IoT Hub | Event Hubs |
| --- | --- | --- |
| Vzory komunikace | Umožňuje [komunikace zařízení cloud] [ lnk-d2c-guidance] (zasílání zpráv, souborů nahrávání a hlášené vlastnosti) a [cloud zařízení komunikace] [ lnk-c2d-guidance] (přímé metody, požadované vlastnosti, zasílání zpráv). |Umožňuje pouze příchozí události (obvykle považovány za pro scénáře zařízení cloud). |
| Informace o stavu zařízení | [Dvojčata zařízení] [ lnk-twins] můžete ukládat a dotaz na informace o stavu zařízení. | Mohou být uloženy žádné informace o stavu zařízení. |
| Podpora protokolu zařízení |Podporuje MQTT, MQTT přes Websocket, AMQP, AMQP prostřednictvím technologie WebSockets a HTTPS. Kromě toho IoT Hub funguje s [brány protokolu Azure IoT][lnk-azure-protocol-gateway], provedení přizpůsobitelné protokolu brány na podporu vlastní protokoly. |Podporuje AMQP, AMQP prostřednictvím technologie WebSockets a HTTPS. |
| Zabezpečení |Poskytuje identitu na zařízení a řízení přístupu odvolatelné. Najdete v článku [zabezpečení část průvodce vývojáře IoT Hub]. |Poskytuje celou centra událostí [sdílené zásady přístupu][Event Hubs - security], s omezenou odvolání podporují pomocí [zásad vydavatele][Event Hubs publisher policies]. Řešení IoT jsou často vyžaduje k implementaci vlastních řešení pro podporu přihlašovacích údajů na zařízení a ochranu proti falšování míry. |
| Monitorování operací |Umožňuje řešení IoT k odběru bohatou sadu událostí správy a připojení identity zařízení například chybám při ověřování jednotlivých zařízení, omezení a výjimky chybný formát. Tyto události umožňují rychle identifikovat potíže s připojením k na úrovni jednotlivých zařízení. |Zpřístupní jenom agregační metriky. |
| Měřítko |Je optimalizován pro podporu miliony současně připojených zařízení. |Meters připojení dle [kvóty Azure Event Hubs][Azure Event Hubs quotas]. Na druhé straně Event Hubs umožňuje zadat oddíl pro každou zprávu odeslat. |
| Sady SDK zařízení |Poskytuje [sady SDK pro zařízení] [ Azure IoT SDKs] pro širokou škálu platformy a jazyky, kromě direct MQTT, AMQP a rozhraní API pro protokol HTTPS. |Je podporován v rozhraní .NET, Java a C, kromě k odeslání rozhraní AMQP a HTTPS. |
| Nahrávání souborů |Umožňuje řešení IoT k nahrání souborů ze zařízení do cloudu. Obsahuje koncový bod souborové oznámení pro pracovní postup integrace a operace monitorování kategorie pro ladění podpory. | Není podporováno. |
| Směrování zpráv do víc koncových bodů | Až 10 vlastní koncové body jsou podporovány. Pravidla určují, jak jsou zprávy směrovány do vlastní koncové body. Další informace najdete v tématu [odesílat a přijímat zprávy službou IoT Hub][lnk-devguide-messaging]. | Vyžaduje další kód zapisovat a hostované pro odeslání zpráv. |

V souhrnu i v případě, že pouze případ použití je příchozí telemetrická data zařízení cloud IoT Hub poskytuje službu, která je určená pro připojení zařízení IoT. Nadále rozbalte prvky pro tyto scénáře pomocí funkce specifické pro IoT. Služba Event Hubs je určená pro příjem příchozích dat událostí v masivním měřítku, i v kontextu mimo datové centrum a intra-datacenter scénáře.

Není používat ve stejném řešení IoT Hub a Event Hubs. Komunikace zařízení cloud zpracovává IoT Hub a Event Hubs zpracovává události v pozdější fázi příchozího do stroji pro zpracování v reálném čase.

### <a name="next-steps"></a>Další kroky
Další informace o plánování nasazení služby IoT Hub, najdete v části [škálování, HA a zotavení po Havárii][lnk-scaling].

Pokud chcete prozkoumat další možnosti IoT Hub, najdete v části:

* [Příručka vývojáře pro službu IoT Hub][lnk-devguide]
* [Nasazení AI do hraniční zařízení s Azure IoT Edge][lnk-iotedge]

[lnk-twins]: iot-hub-devguide-device-twins.md
[lnk-c2d-guidance]: iot-hub-devguide-c2d-guidance.md
[lnk-d2c-guidance]: iot-hub-devguide-d2c-guidance.md

[Azure Event Hubs]: ../event-hubs/event-hubs-what-is-event-hubs.md
[zabezpečení část průvodce vývojáře IoT Hub]: iot-hub-devguide-security.md
[Event Hubs - security]: ../event-hubs/event-hubs-authentication-and-security-model-overview.md
[Event Hubs publisher policies]: ../event-hubs/event-hubs-features.md#event-publishers
[Azure Event Hubs quotas]: ../event-hubs/event-hubs-quotas.md
[Azure IoT SDKs]: https://github.com/Azure/azure-iot-sdks
[lnk-azure-protocol-gateway]: iot-hub-protocol-gateway.md

[lnk-scaling]: iot-hub-scaling.md
[lnk-devguide]: iot-hub-devguide.md
[lnk-iotedge]: ../iot-edge/tutorial-simulate-device-linux.md
[lnk-devguide-messaging]: iot-hub-devguide-messaging.md
