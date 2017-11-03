---
title: "Azure IoT Hub komunikační protokoly a porty | Microsoft Docs"
description: "Příručka vývojáře - popisuje podporované komunikační protokoly komunikace zařízení cloud a z cloudu do zařízení a čísla portů, která musí být otevřený."
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
ms.date: 08/31/2017
ms.author: dobett
ms.openlocfilehash: 37602bf78f7a43fb8255ddc0aad21f24095cb43c
ms.sourcegitcommit: 51ea178c8205726e8772f8c6f53637b0d43259c6
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# Reference – volba komunikační protokol

IoT Hub umožňuje zařízení používat následující protokoly pro komunikaci straně zařízení:

* [MQTT][lnk-mqtt]
* MQTT přes objekty WebSockets
* [AMQP][lnk-amqp]
* AMQP přes objekty WebSockets
* HTTPS

Informace o tom, jak tyto protokoly podporují konkrétní funkce služby IoT Hub naleznete v tématu [pokyny komunikace zařízení cloud] [ lnk-d2c-guidance] a [Cloud zařízení komunikace pokyny] [lnk-c2d-guidance].

Následující tabulka poskytuje podrobný doporučení pro zvoleného protokolu:

| Protocol (Protokol) | Pokud byste měli zvolit tento protokol |
| --- | --- |
| MQTT <br> MQTT přes protokol WebSocket |Použít na všech zařízeních, které nevyžadují připojení více zařízení (každý má svoje vlastní přihlašovací údaje podle zařízení) pomocí stejné připojení protokol TLS. |
| AMQP <br> AMQP přes protokol WebSocket |Použít na pole a cloudové brány využívat výhod připojení multiplexní mezi zařízeními. |
| HTTPS |Používejte pro zařízení, které nemohou podporovat jiné protokoly. |

Když zvolíte váš protokol pro komunikaci straně zařízení, zvažte následující body:

* **Vzor cloud zařízení**. HTTPS nemá účinný způsob, jak implementovat nabízené serveru. Jako takový při použití protokolu HTTPS, zařízení dotazovat služby IoT Hub pro zprávy typu cloud zařízení. Tento přístup je neefektivní pro zařízení a služby IoT Hub. V části aktuální pokyny pro protokol HTTPS musí každé zařízení dotazování na zprávy každých 25 minut nebo déle. MQTT a AMQP podporu nabízených serveru při přijímání zprávy typu cloud zařízení. Umožňují okamžitou nabízela zpráv ze služby IoT Hub zařízení. Pokud vám záleží hlavně doručení latence, MQTT nebo AMQP protokoly nejlepší používat. Pro připojené zřídka zařízení funguje i HTTPS.
* **Pole brány**. Při použití MQTT a HTTPS, nemůžete se připojit více zařízení (každý má svoje vlastní přihlašovací údaje podle zařízení) pomocí stejné připojení protokol TLS. Pro [pole scénáře brány] [ lnk-azure-gateway-guidance] , které vyžadují jeden TLS připojení mezi brána pole a IoT Hub pro každé připojených zařízení a tyto protokoly jsou zhoršené.
* **Nedostatek prostředků zařízení**. Knihovny MQTT a HTTPS mít menší nároky než knihovny AMQP. Pokud zařízení má omezené prostředky (například méně než 1 MB paměti RAM), může být tyto protokoly jako takový pouze implementace protokolu, která je k dispozici.
* **Sítě traversal**. Standardní protokol AMQP používá port 5671 a MQTT naslouchá na portu 8883. Použití těchto portů by mohla způsobovat problémy v sítích, které jsou uzavřeny protokolů – protokol HTTPS. Pomocí technologie WebSockets nebo HTTPS v tomto scénáři použijte MQTT přes Websocket, AMQP.
* **Velikost datové části**. MQTT a AMQP jsou binární protokoly, které mít za následek kompaktnější datových částí než protokol HTTPS.

> [!WARNING]
> Při použití protokolu HTTPS, musí každé zařízení dotazování pro zprávy typu cloud zařízení, každých 25 minut nebo déle. Během vývoje, je přijatelné pro cyklické dotazování častěji, než každých 25 minut.

## Čísla portů

Zařízení může komunikovat s centrem IoT v Azure pomocí různých protokolů. Obvykle volba protokol vycházejí z konkrétní požadavky na řešení. Následující tabulka uvádí Odchozí porty, které musí být otevřené pro zařízení a mohli používat konkrétní protokolu:

| Protocol (Protokol) | Port |
| --- | --- |
| MQTT |8883 |
| MQTT přes objekty WebSockets |443 |
| AMQP |5671 |
| AMQP přes objekty WebSockets |443 |
| HTTPS |443 |

Po vytvoření služby IoT hub v oblasti Azure IoT hub uchovává stejnou IP adresu po dobu jeho existence tohoto centra IoT. Ale pokud Microsoft přesune na jednotky škálování různých udržovat kvalitu služby IoT hub, bude mu přiřazen novou IP adresu.


## Další kroky

Další informace o tom, jak IoT Hub implementuje protokol MQTT najdete v tématu [komunikace službou IoT hub pomocí protokolu MQTT][lnk-mqtt-support].

[lnk-d2c-guidance]: iot-hub-devguide-d2c-guidance.md
[lnk-c2d-guidance]: iot-hub-devguide-c2d-guidance.md
[lnk-mqtt-support]: iot-hub-mqtt-support.md
[lnk-amqp]: http://docs.oasis-open.org/amqp/core/v1.0/os/amqp-core-complete-v1.0-os.pdf
[lnk-mqtt]: http://docs.oasis-open.org/mqtt/mqtt/v3.1.1/mqtt-v3.1.1.pdf
[lnk-azure-gateway-guidance]: iot-hub-devguide-endpoints.md#field-gateways
