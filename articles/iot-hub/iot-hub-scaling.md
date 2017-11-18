---
title: "Škálování Azure IoT Hub | Microsoft Docs"
description: "Postup škálování služby IoT hub pro podporu vašeho propustnost předpokládaného zpráv. Obsahuje souhrn podporovaných propustnosti pro každou vrstvu a možnosti pro horizontálního dělení."
services: iot-hub
documentationcenter: 
author: fsautomata
manager: timlt
editor: 
ms.assetid: e7bd4968-db46-46cf-865d-9c944f683832
ms.service: iot-hub
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/13/2017
ms.author: elioda
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 00043293eb57768f0117e912bb67f02d088934f3
ms.sourcegitcommit: 933af6219266cc685d0c9009f533ca1be03aa5e9
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/18/2017
---
# <a name="scale-your-iot-hub-solution"></a>Škálování řešení IoT hub
Azure IoT Hub může podporovat až miliony současně připojených zařízení. Další informace najdete v tématu [IoT Hub ceny][lnk-pricing]. Jednotlivé jednotky služby IoT Hub umožňuje počet denní zprávy.

Chcete-li správně škálování řešení, zvažte konkrétní používání služby IoT Hub. Zejména zvažte propustnosti vyžaduje ve špičce operací v těchto kategoriích:

* Zprávy typu zařízení-cloud
* Zprávy typu cloud zařízení
* Operace registru identit

Kromě těchto informací propustnost, najdete v části [IoT Hub kvóty a omezení] [ IoT Hub quotas and throttles] a navrhněte řešení odpovídajícím způsobem.

## <a name="device-to-cloud-and-cloud-to-device-message-throughput"></a>Propustnost zpráv typu zařízení cloud a z cloudu do zařízení
Nejlepší způsob, jak velikost do řešení IoT Hub je vyhodnotit provozu na základě za jednotku.

Zprávy typu zařízení cloud postupujte podle těchto pokynů propustnost.

| Úroveň | Propustnost | Udržovanou rychlost |
| --- | --- | --- |
| S1 |Až 1111 KB za minutu za jednotku<br/>(1,5 GB/den/unit) |Průměr 278 zpráv za minutu za jednotku<br/>(400 000 zprávy dny a hodiny za jednotku) |
| S2 |Až 16 MB za minutu za jednotku<br/>(22.8 GB/den/unit) |Průměr 4,167 zpráv za minutu za jednotku<br/>(6 milionu zprávy dny a hodiny za jednotku) |
| S3 |Až 814 MB za minutu za jednotku<br/>(1144.4 GB/den/unit) |Průměr 208,333 zpráv za minutu za jednotku<br/>(300 milionů zprávy dny a hodiny za jednotku) |

## <a name="identity-registry-operation-throughput"></a>Propustnost operaci registru identit
Operace s registrem identit služby IoT Hub není by mělo být spuštění operace, jako se většinou vztahují k zřizování zařízení.

Pro konkrétní shluků čísla výkonu, najdete v části [IoT Hub kvóty a omezení][IoT Hub quotas and throttles].

## <a name="sharding"></a>Horizontálního dělení
Během jedné IoT hub lze škálovat na miliony zařízení, někdy řešení vyžaduje konkrétní výkonové charakteristiky, které nemůže zaručit jeden IoT hub. V takovém případě se doporučuje, aby oddílu zařízení do více centra IoT. Více centra IoT funkce smooth shluky provoz a získat požadovaná propustnost nebo sazby operace, které jsou požadovány.

## <a name="next-steps"></a>Další kroky
Pokud chcete prozkoumat další možnosti IoT Hub, najdete v části:

* [Příručka vývojáře pro službu IoT Hub][lnk-devguide]
* [Nasazení AI do hraniční zařízení s Azure IoT Edge][lnk-iotedge]

[lnk-pricing]: https://azure.microsoft.com/pricing/details/iot-hub
[IoT Hub quotas and throttles]: iot-hub-devguide-quotas-throttling.md

[lnk-devguide]: iot-hub-devguide.md
[lnk-iotedge]: ../iot-edge/tutorial-simulate-device-linux.md
