---
title: "Brány protokolu Azure IoT | Microsoft Docs"
description: "Jak používat brány protokolu Azure IoT rozšířit IoT Hub, možnosti a podpora protokolu povolit zařízení pro připojení do vašeho centra pomocí protokolů nejsou podporovány službou IoT Hub nativně."
services: iot-hub
documentationcenter: 
author: kdotchkoff
manager: timlt
editor: 
ms.assetid: 555e59ae-3136-4533-8ba8-f3a3b6acf648
ms.service: iot-hub
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/11/2017
ms.author: kdotchko
ms.openlocfilehash: b2ad2c6f5eeec2f803e2d2f5f98831b551efa8b1
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# Podpora dalších protokoly pro IoT Hub
Azure IoT Hub nativně podporuje komunikaci přes protokoly MQTT, AMQP a HTTP. V některých případech zařízení nebo pole brány nemusí být možné používat jednu z těchto standardních protokolů a bude vyžadovat protokol přizpůsobení. V takových případech můžete použít vlastní bránu. Vlastní bránu můžete povolit protokol přizpůsobení pro koncové body centra IoT tak přemostění provoz do a ze služby IoT Hub. Můžete použít [brány protokolu Azure IoT](https://github.com/Azure/azure-iot-protocol-gateway/blob/master/README.md) jako vlastní bránu povolit protokol přizpůsobení pro IoT Hub.

## Brány protokolu Azure IoT
Brány protokolu Azure IoT je rozhraní pro protokol přizpůsobení, která je určená pro špičkové obousměrnou komunikaci zařízení službou IoT Hub. Brána protokolu je průchozí komponenty, která přijímá připojení zařízení přes určitý protokol. Přenosy do služby IoT Hub je přemosťuje prostřednictvím protokolu AMQP 1.0. Je k dispozici jako software open source projekt zajistit flexibilitu pro přidání podpory pro různé protokoly a verze protokolu brány protokolu Azure IoT.

Vysoce škálovatelné způsobem můžete nasadit brány protokolu v Azure pomocí Azure Service Fabric, rolí pracovního procesu Azure Cloud Services nebo virtuální počítače s Windows. Kromě toho brány protokolu se dá nasadit v prostředích místní, například pole brány.

Brány protokolu Azure IoT zahrnuje adaptér MQTT protokol, který umožňuje přizpůsobit chování protokolu MQTT v případě potřeby. Vzhledem k tomu, že služba IoT Hub zajišťuje integrovanou podporu pro protokol MQTT v3.1.1, měli byste pouze zvážit použití adaptér protokolu MQTT Pokud vlastní nastavení protokolu nebo specifické požadavky pro další funkce, je potřeba.

Adaptér MQTT také ukazuje programovací model pro vytváření adaptéry protokolu pro jiné protokoly. Kromě toho programovací model brány protokolu Azure IoT můžete zařadit vlastní komponenty pro speciální zpracování například vlastní ověřování, zpráva transformace, kompresi nebo dekompresi nebo šifrování a dešifrování přenosů mezi zařízeními a IoT Hub.

Pružnosti brány protokolu a MQTT implementace jsou uvedeny v projektu open-source softwaru. To umožňuje podle potřeby přizpůsobit implementace.

## Další kroky
Další informace o brány protokolu Azure IoT a jak používat a nasaďte ho jako součást řešení IoT najdete v tématu:

* [Úložiště brány protokolu Azure IoT na Githubu](https://github.com/Azure/azure-iot-protocol-gateway/blob/master/README.md)
* [Příručka vývojáře brány protokolu sady Azure IoT](https://github.com/Azure/azure-iot-protocol-gateway/blob/master/docs/DeveloperGuide.md)

Další informace o plánování nasazení služby IoT Hub naleznete v tématu:

* [Porovnání s služby Event Hubs][lnk-compare]
* [Změna velikosti, HA a zotavení po Havárii][lnk-scaling]
* [Příručka vývojáře pro službu IoT Hub][lnk-devguide]

[lnk-compare]: iot-hub-compare-event-hubs.md
[lnk-scaling]: iot-hub-scaling.md
[lnk-devguide]: iot-hub-devguide.md
