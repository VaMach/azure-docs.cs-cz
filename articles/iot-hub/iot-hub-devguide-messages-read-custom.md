---
title: "Pochopení vlastní koncové body Azure IoT Hub | Microsoft Docs"
description: "Příručka vývojáře - pravidel směrování pro směrování zpráv typu zařízení cloud do vlastní koncové body."
services: iot-hub
documentationcenter: .net
author: dominicbetts
manager: timlt
editor: 
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/19/2017
ms.author: dobett
ms.openlocfilehash: a499783fc02e1371562edd41b827758e19fbd823
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="use-message-routes-and-custom-endpoints-for-device-to-cloud-messages"></a>Použít vlastní koncové body a směrování zpráv pro zprávy typu zařízení cloud

IoT Hub umožňuje směrovat [zpráv typu zařízení cloud] [ lnk-device-to-cloud] na koncové body služby směřujících centra IoT podle vlastnosti zprávy. Pravidla směrování poskytují flexibilitu pro odesílání zpráv, kde musí přejít na zpracování zpráv bez nutnosti dalších služeb nebo napsat další kód. Každé pravidlo směrování, která můžete konfigurovat má následující vlastnosti:

| Vlastnost      | Popis |
| ------------- | ----------- |
| **Název**      | Jedinečný název, který identifikuje pravidlo. |
| **Zdroj**    | Původ datový proud do být reagovali na ni. Například zařízení telemetrie. |
| **Podmínka** | Výraz dotazu pro směrování pravidlo, které je spouštění hlavičky a text zprávy a používá k určení, zda je shoda pro koncový bod. Další informace o vytváření podmínku trasy, najdete v článku [odkaz - dotazovacího jazyka pro úlohy a dvojčata zařízení][lnk-devguide-query-language]. |
| **Koncový bod**  | Název koncového bodu, kde IoT Hub odešle zprávy, které splňují podmínku. Koncové body musí být ve stejné oblasti jako službu IoT hub, jinak vám může být účtovat mezi oblastmi zápisy. |

Do jedné zprávy může odpovídat na více pravidel směrování, ve kterých případ IoT Hub doručení zprávy do koncového bodu spojené s každou odpovídající pravidlo podmínku. Centrum IoT také automaticky deduplicates doručení zpráv, takže pokud zpráva odpovídá více pravidel, které mají stejný cíl, je pouze zapsán do tohoto cíle jednou.

Centrum IoT má výchozí [vestavěným koncovým bodem][lnk-built-in]. Můžete vytvořit vlastní koncové body pro směrování zpráv do pomocí jiných služeb v rámci vašeho předplatného propojení k rozbočovači. IoT Hub aktuálně podporuje kontejnery Azure Storage, Event Hubs, fronty Service Bus a témat sběrnice Service Bus jako vlastní koncové body.

> [!NOTE]
> IoT Hub podporuje pouze zápis dat do Azure Storage kontejnerů jako objekty BLOB.

> [!WARNING]
> Fronty sběrnice a témata s **relací** nebo **duplicitní detekce** povoleno nejsou podporovány jako vlastní koncové body.

Další informace o vytváření vlastní koncové body v centru IoT najdete v tématu [koncové body centra IoT][lnk-devguide-endpoints].

Další informace o čtení ze vlastní koncové body najdete v tématu:

* Čtení z [kontejnery Azure Storage][lnk-getstarted-storage].
* Čtení z [služby Event Hubs][lnk-getstarted-eh].
* Čtení z [fronty Service Bus][lnk-getstarted-queue].
* Čtení z [témat sběrnice Service Bus][lnk-getstarted-topic].

### <a name="next-steps"></a>Další kroky

Další informace o koncové body centra IoT najdete v tématu [koncové body centra IoT][lnk-devguide-endpoints].

Další informace o dotazovací jazyk používáte k definování pravidel směrování, najdete v části [IoT Hub dotazovacího jazyka pro dvojčata zařízení, úlohy a směrování zpráv][lnk-devguide-query-language].

[Zpráv typu zařízení cloud proces IoT Hub pomocí trasy] [ lnk-d2c-tutorial] kurzu se dozvíte, jak používat pravidla směrování a vlastní koncové body.

[lnk-built-in]: iot-hub-devguide-messages-read-builtin.md
[lnk-device-to-cloud]: iot-hub-devguide-messages-d2c.md
[lnk-devguide-query-language]: iot-hub-devguide-query-language.md
[lnk-devguide-endpoints]: iot-hub-devguide-endpoints.md
[lnk-d2c-tutorial]: iot-hub-csharp-csharp-process-d2c.md
[lnk-getstarted-eh]: ../event-hubs/event-hubs-csharp-ephcs-getstarted.md
[lnk-getstarted-queue]: ../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md
[lnk-getstarted-topic]: ../service-bus-messaging/service-bus-dotnet-how-to-use-topics-subscriptions.md
[lnk-getstarted-storage]: ../storage/blobs/storage-blobs-introduction.md
