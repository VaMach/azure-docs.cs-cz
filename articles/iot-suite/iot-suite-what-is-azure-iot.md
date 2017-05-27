---
title: "Řešení Azure pro Internet věcí (IoT Suite) | Dokumentace Microsoftu"
description: "Přehled technologie IoT na platformě Azure, včetně ukázkové architektury řešení a informací o tom, jak souvisí se sadou Azure IoT Suite a předkonfigurovanými řešeními"
services: 
suite: iot-suite
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 437d2655-896f-4a9e-a4a8-b864790d3ef8
ms.service: iot-suite
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/25/2017
ms.author: dobett
ms.translationtype: Human Translation
ms.sourcegitcommit: b0c27ca561567ff002bbb864846b7a3ea95d7fa3
ms.openlocfilehash: 1c8703d64ff45e589a7ce93f1f2176681e1bf331
ms.contentlocale: cs-cz
ms.lasthandoff: 04/25/2017


---
[!INCLUDE [iot-azure-and-iot](../../includes/iot-azure-and-iot.md)]

## <a name="azure-iot-suite"></a>Azure IoT Suite
Microsoft Azure IoT Suite je podnikové řešení umožňující rychlý start prostřednictvím sady rozšiřitelných předkonfigurovaných řešení. Tato řešení se vztahují k běžným scénářům IoT, jako je [vzdálené monitorování][lnk-preconfigured-solutions], [prediktivní údržba][lnk-predictive-maintenance] a [propojená továrna][lnk-connected-factory]. Tato řešení jsou implementacemi architektury řešení IoT, která je vysvětlená v tomto článku.

Předkonfigurovaná řešení jsou kompletní, funkční, ucelená řešení, která zahrnují:

- Simulovaná zařízení, která vám pomohou začít.
- Předkonfigurované služby Azure, jako například [Azure IoT Hub][Azure IoT Hub], [Azure Event Hubs][Azure Event Hubs], [Azure Stream Analytics][Azure Stream Analytics], [Azure Machine Learning][Azure Machine Learning] a [Azure Storage][Azure storage].
- Konzoly pro správu konkrétních řešení.

Předkonfigurovaná řešení obsahují ověřený a ke zprovoznění připravený kód, který si můžete přizpůsobit a rozšířit tak, abyste mohli implementovat vlastní scénáře IoT.

Mohla by vás také zajímat služba [Azure IoT Hub][Azure IoT Hub], kterou využívá celá řada předkonfigurovaných řešení. [Azure IoT Hub][Azure IoT Hub] nabízí bezpečnou a spolehlivou obousměrnou komunikaci mezi zařízeními a cloudy, které jsou použity v architekturách předkonfigurovaných řešení.

## <a name="next-steps"></a>Další kroky
Prostudováním těchto materiálů zjistíte další informace o sadě IoT Suite a předkonfigurovaných řešeních:

* [Co je Azure IoT Suite?][lnk-whatissuite]
* [Co jsou předkonfigurovaná řešení Azure IoT Suite?][lnk-whatarepreconfigured]

[lnk-whatissuite]: iot-suite-overview.md
[lnk-whatarepreconfigured]: iot-suite-what-are-preconfigured-solutions.md

[lnk-preconfigured-solutions]: iot-suite-getstarted-preconfigured-solutions.md
[Azure IoT Hub]: https://azure.microsoft.com/documentation/services/iot-hub/
[Azure Event Hubs]: https://azure.microsoft.com/documentation/services/event-hubs/
[Azure Stream Analytics]: https://azure.microsoft.com/documentation/services/stream-analytics/
[Azure Machine Learning]: https://azure.microsoft.com/documentation/services/machine-learning/
[Azure storage]: https://azure.microsoft.com/documentation/services/storage/
[lnk-predictive-maintenance]: iot-suite-predictive-overview.md
[lnk-connected-factory]: iot-suite-connected-factory-overview.md
