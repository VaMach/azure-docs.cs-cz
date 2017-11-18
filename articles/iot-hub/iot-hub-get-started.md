---
title: "Azure IoT Hub - začít pracovat s připojením zařízení IoT ke cloudu | Microsoft Docs"
description: "Zjistěte, jak se připojit ke službě Azure IoT Hub panely IoT a Startovní sady. Zařízení může odesílat telemetrická data do služby IoT Hub a IoT Hub můžete sledovat a spravovat vaše zařízení."
services: iot-hub
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
keywords: Azure iot hub kurzu
ms.assetid: 24376318-5344-4a81-a1e6-0003ed587d53
ms.service: iot-hub
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/22/2017
ms.author: dobett
ms.openlocfilehash: 764c6bd3c2466ceb93e33d03bece1c9036c1b1d9
ms.sourcegitcommit: 933af6219266cc685d0c9009f533ca1be03aa5e9
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/18/2017
---
# <a name="azure-iot-hub-get-started-tutorials"></a>Azure IoT Hub Začínáme kurzy

Azure IoT Hub a sady SDK zařízení Azure IoT můžete použít k vytvoření řešení Internetu věcí (IoT):

* Azure IoT Hub je plně spravovaná služba v cloudu, které bezpečně připojit, monitoruje a spravuje zařízení IoT. Implementace zařízení IoT pomocí sady SDK zařízení IoT v Azure.
* Používejte bránu IoT v složitější scénáře IoT. Například, kdy potřebujete vezměte v úvahu faktory, jako jsou zařízení se starší verzí, náklady na šířku pásma, zásady zabezpečení a ochrana osobních údajů nebo zpracování dat okraj. V těchto scénářích použijete k implementaci bránu, která se připojuje zařízení do služby IoT hub Azure IoT okraj.

## <a name="what-the-tutorials-cover"></a>Co kurzy zahrnují

Tyto kurzy vám představí Azure IoT Hub a sady SDK pro zařízení. Kurzů k zahrnují běžné scénáře IoT k předvedení funkcí služby IoT Hub. Kurzů k také znázorňují, jak kombinovat s jinými službami Azure a nástroje pro vytváření výkonnější IoT řešení IoT Hub. V kurzech můžete použít simulované nebo skutečné zařízení IoT. Kromě toho můžete další informace o použití brány postup pro povolení zařízením pro připojení do služby IoT hub.

## <a name="set-up-your-device"></a>Nastavení zařízení

Připojte zařízení IoT nebo brány Azure IoT Hub. Můžete vybrat fyzické nebo simulované zařízení začít:

| Zařízení IoT                       | Programovací jazyk |
|----------------------------------|----------------------|
| Raspberry Pi                     | [Python][Pi_Py], [Node.js][Pi_Nd], [C][Pi_C]  |
| IoT DevKit                       | [Arduino v VSCode][DevKit]     |
| Intel Edison                     | [Node.js][Ed_Nd], [C][Ed_C]    |
| Prolnutí Adafruit HUZZAH ESP8266  | [Arduino][Hu_Ard]              |
| Sparkfun ESP8266 věc vývojářů       | [Arduino][Th_Ard]              |
| Prolnutí Adafruit M0              | [Arduino][M0_Ard]              |
| Simulované zařízení na počítačích           | [Rozhraní .NET][Sim_NET], [Java][Sim_Jav], [Node.js][Sim_Nd], [Python][Sim_Pyth] |
| Simulátor online zařízení         | [Malinová platformy (Node.js)][Ol_Sim] |

[!INCLUDE [iot-hub-get-started-extended](../../includes/iot-hub-get-started-extended.md)]

[Pi_Nd]: iot-hub-raspberry-pi-kit-node-get-started.md
[Pi_C]: iot-hub-raspberry-pi-kit-c-get-started.md
[Pi_Py]: iot-hub-raspberry-pi-kit-python-get-started.md
[DevKit]: iot-hub-arduino-iot-devkit-az3166-get-started.md
[Ed_Nd]: iot-hub-intel-edison-kit-node-get-started.md
[Ed_C]: iot-hub-intel-edison-kit-c-get-started.md
[Hu_Ard]: iot-hub-arduino-huzzah-esp8266-get-started.md
[Th_Ard]: iot-hub-sparkfun-esp8266-thing-dev-get-started.md
[M0_Ard]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-get-started.md
[Sim_NET]: iot-hub-csharp-csharp-getstarted.md
[Sim_Jav]: iot-hub-java-java-getstarted.md
[Sim_Nd]: iot-hub-node-node-getstarted.md
[Sim_Pyth]: iot-hub-python-getstarted.md
[NUC_Lnx]: iot-hub-gateway-kit-c-lesson1-set-up-nuc.md
[Sim_Lnx]: iot-hub-linux-iot-edge-get-started.md
[Sim_Win]: iot-hub-windows-iot-edge-get-started.md
[Ol_Sim]: iot-hub-raspberry-pi-web-simulator-get-started.md
