---
title: "Přehled Azure IoT Edge | Microsoft Docs"
description: "Popisuje klíčové architektury koncepty v Azure IoT Edge, jako jsou brány, moduly a zprostředkovatelé."
services: iot-hub
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.service: iot-hub
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/04/2017
ms.author: dobett
ms.openlocfilehash: 11f6375c319c8945b0278003ee08215715f7ac42
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="azure-iot-edge-architectural-concepts"></a>Koncepty architektury služby Azure IoT Edge

Před zkontrolujte všechny ukázkový kód nebo vytvořit vlastní pole bránu pomocí IoT Edge, byste měli porozumět klíčové koncepty, které jsou základem architektuře IoT okraj.

## <a name="iot-edge-modules"></a>Moduly IoT Edge

Vytvořit bránu s Azure IoT hranou vytvořením a ty se *IoT Edge moduly*. Moduly používají pro výměnu dat mezi sebou *zprávy*. Modul obdrží zprávu a provede s ní určitou akci, například ji může transformovat na novou zprávu a pak ji publikovat pro ostatní moduly ke zpracování. Některé moduly pouze vytvářejí nové zprávy a nezpracovávají zprávy příchozí. Řetěz modulů vytvoří kanál zpracování dat, ve kterém každý modul provádí transformaci dat v daném místě kanálu.

![Řetěz modulů v bráně vytvořené s použitím služby Azure IoT Edge][1]

Okraj IoT obsahuje následující součásti:

* Předem napsané moduly, které provádějí běžné funkce brány.
* Rozhraní, které vývojář může použít k vytvoření vlastních modulů.
* Infrastrukturu nutnou pro nasazení a spuštění sady modulů.

Sada SDK poskytuje abstraktní vrstvu, která vám umožní sestavovat bran pro spouštění v různých operačních systémů a platformy.

![Vrstva abstrakce služby Azure IoT Edge][2]

## <a name="messages"></a>Zprávy

Ačkoli je představa modulů posílajících zprávy pohodlným způsobem, jak popsat koncepci funkce brány, neodráží přesně samotnou její činnost. Moduly IoT Edge pomocí zprostředkovatele vzájemně komunikovat. Moduly publikování zpráv zprostředkovatele (pomocí zasílání zpráv vzory, třeba sběrnice nebo publikování a přihlášení k odběru) a nechte zprostředkovatele směrovat zprávy na moduly, které k němu připojená.

K publikování zpráv do zprostředkovatele používá modul funkci **Broker_Publish**. Zprostředkovatel předává zprávy ostatním modulům zavoláním funkce zpětného volání. Zpráva se skládá ze sady vlastností klíč/hodnota a obsah se předává jako blok paměti.

![Role zprostředkovatele ve službě Azure IoT Edge][3]

## <a name="message-routing-and-filtering"></a>Směrování a filtrování zpráv

Přímé zpráv, které mají správnou moduly IoT hraniční dvěma způsoby:

* Sada odkazů můžete předat zprostředkovatele, aby věděl, zprostředkovatel může zdrojový a podřízený pro každý modul.
* Modul můžete filtrovat podle vlastnosti zprávy.

Modul by měl postupovat pouze podle zpráv, které mu jsou určené. Odkazy a efektivně filtrování zpráv vytvoření kanálu zprávy.

## <a name="next-steps"></a>Další kroky

Tyto koncepty použijí v ukázku, můžete spustit najdete v sekci [prozkoumat Azure IoT Edge architektura][lnk-hello-world].

<!-- Images -->
[1]: media/iot-hub-iot-edge-overview/modules.png
[2]: media/iot-hub-iot-edge-overview/modules_2.png
[3]: media/iot-hub-iot-edge-overview/messages_1.png

<!-- Links -->
[lnk-hello-world]: ./iot-hub-linux-iot-edge-get-started.md
