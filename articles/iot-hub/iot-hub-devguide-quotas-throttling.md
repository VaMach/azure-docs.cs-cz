---
title: "Rady pro pochopení Azure IoT Hub kvóty a omezení | Microsoft Docs"
description: "Příručka vývojáře – popis kvóty, které platí pro IoT Hub a očekávané chování omezení."
services: iot-hub
documentationcenter: .net
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 425e1b08-8789-4377-85f7-c13131fae4ce
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/18/2017
ms.author: dobett
ms.openlocfilehash: 8ffe25f1950f8535983c2c344b5c4331b7157869
ms.sourcegitcommit: 51ea178c8205726e8772f8c6f53637b0d43259c6
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="reference---iot-hub-quotas-and-throttling"></a>Referenční dokumentace - IoT Hub kvóty a omezení

## <a name="quotas-and-throttling"></a>Kvóty a omezování
Každé předplatné Azure může mít maximálně 10 centra IoT a nejvýše 1 volné rozbočovače.

Každé centrum IoT je opatřen počet jednotek v konkrétní SKU (Další informace najdete v tématu [ceník služby Azure IoT Hub][lnk-pricing]). SKU a počet jednotek určit maximální denní kvóta zpráv, které můžete odeslat.

Verze SKU také určuje omezení limity, které vynucuje IoT Hub na všechny operace.

## <a name="operation-throttles"></a>Omezení operace
Omezení operace jsou omezení míry, které se použijí v minutu rozsahy a jsou určeny, aby se zabránilo zneužití. Centrum IoT pokusí vyhnout vrací chyby, pokud je to možné, ale začne vrácení výjimky, pokud omezení porušení příliš dlouho.

Následující tabulka obsahuje vynucené omezení. Hodnoty se vztahují k rozbočovači jednotlivých.

| Omezení | Volné a rozbočovače S1 | S2 rozbočovače | Centra S3 | 
| -------- | ------- | ------- | ------- |
| Operace s registrem identit (vytvořit, získat, seznam, aktualizace, odstranění) | 1.67/sec/Unit (min/100/unit) | 1.67/sec/Unit (min/100/unit) | 83.33/sec/Unit (5000/minimální/unit) |
| Připojení zařízení | Vyšší 100 za sekundu nebo 12/sec/jednotka <br/> Například dvě jednotky S1 jsou 2\*12 = 24/sec, ale, že nejméně 100 za sekundu napříč vaší jednotky. U devět S1 jednotek, máte 108 za sekundu (9\*12) mezi vaší jednotky. | 120/sec/jednotka | 6000/sec/jednotka |
| Odesílání typu zařízení-cloud | Vyšší 100 za sekundu nebo 12/sec/jednotka <br/> Například dvě jednotky S1 jsou 2\*12 = 24/sec, ale, že nejméně 100 za sekundu napříč vaší jednotky. U devět S1 jednotek, máte 108 za sekundu (9\*12) mezi vaší jednotky. | 120/sec/jednotka | 6000/sec/jednotka |
| Odesílání typu cloud-zařízení | 1.67/sec/Unit (min/100/unit) | 1.67/sec/Unit (min/100/unit) | 83.33/sec/Unit (5000/minimální/unit) |
| Příjem typu cloud-zařízení <br/> (jenom když zařízení používá protokol HTTPS)| 16.67/sec/Unit (1000/minimální/unit) | 16.67/sec/Unit (1000/minimální/unit) | 833.33/sec/Unit (50000/minimální/unit) |
| Nahrávání souborů | 1.67 souboru nahrávání oznámení/sec/jednotka (min/100/unit) | 1.67 souboru nahrávání oznámení/sec/jednotka (min/100/unit) | 83.33 souboru odesílání oznámení/sec/jednotky (5000/minimální/unit) |
| Přímé metody | 20/sec/jednotka | 60/sec/jednotka | 3000/sec/jednotka | 
| Čtení dvojčat zařízení | 10 za sekundu | Vyšší 10 za sekundu nebo 1/sec/jednotka | 50/sec/jednotka |
| Aktualizace dvojčat zařízení | 10 za sekundu | Vyšší 10 za sekundu nebo 1/sec/jednotka | 50/sec/jednotka |
| Operace úloh <br/> (vytvoření, aktualizace, výpis, odstranění) | 1.67/sec/Unit (min/100/unit) | 1.67/sec/Unit (min/100/unit) | 83.33/sec/Unit (5000/minimální/unit) |
| Propustnost operací úloh jednotlivých zařízení | 10 za sekundu | Vyšší 10 za sekundu nebo 1/sec/jednotka | 50/sec/jednotka |

Je důležité o vysvětlení, že *připojení zařízení* omezení řídí rychlost, jakou by bylo možné navázat nová připojení zařízení pomocí služby IoT hub. *Připojení zařízení* omezení neřídí maximální počet současně připojených zařízení. Omezení závisí na počtu jednotek, které jsou zřízené pro službu IoT hub.

Pokud si zakoupíte jedné jednotky S1, například zobrazí omezení připojení 100 za sekundu. Proto se pokud chcete připojit 100 000 zařízení, trvá nejméně 1 000 sekund (přibližně 16 minut). Ale může mít libovolný počet současně připojených zařízení, máte zařízení zaregistrované v registru identit.

Podrobné informace o IoT Hub omezení chování, naleznete v příspěvku blogu [IoT Hub, omezení a][lnk-throttle-blog].

> [!NOTE]
> V každém okamžiku je možné zvýšit kvóty nebo omezení limity zvýšit počet zřízené jednotky v služby IoT hub.
> 
> [!IMPORTANT]
> Operace s registrem identit jsou určeny k použití v správu zařízení a zřizování scénáře. Čtení nebo aktualizaci velký počet identit zařízení je podporována prostřednictvím [import a export úloh][lnk-importexport].
> 
> 

## <a name="other-limits"></a>Další omezení

IoT Hub vynucuje jiné provozní omezení:

| Operace | Omezení |
| --------- | ----- |
| Nahrávání souborů identifikátory URI | 10000 identifikátory URI SAS může být si pro účet úložiště najednou. <br/> Najednou může existovat 10 identifikátorů URI SAS/zařízení. |
| Úlohy | Historie úlohy se uchovávají až do 30 dnů <br/> Maximální souběžných úloh je 1 (pro zdarma) a S1, 5 (pro S2), 10 (pro S3). |
| Další koncové body | Placené SKU centra asi 10 další koncové body. Volné centra skladová položka může mít jeden další koncový bod. |
| Pravidla směrování zpráv | Placené SKU rozbočovače mají 100 pravidla směrování. Volné centra skladová položka může mít pět pravidla směrování. |
| Zasílání zpráv typu zařízení cloud | Maximální velikost 256 KB |
| Zasílání zpráv typu cloud zařízení | Maximální velikost 64 KB |
| Zasílání zpráv typu cloud zařízení | Maximální počet čekajících zpráv pro doručení je 50 |

> [!NOTE]
> Maximální počet zařízení, které se můžete připojit k jedné IoT hub je v současné době 500 000. Pokud chcete tento limit zvýšit, obraťte se na [Microsoft Support](https://azure.microsoft.com/support/options/).

## <a name="latency"></a>Latence
Centrum IoT se snaží poskytovat s nízkou latencí pro všechny operace. Ale kvůli stavu sítě a dalších faktorů nepředvídatelným nemůže zaručit maximální latence. Při návrhu řešení, proveďte následující kroky:

* Vyvarujte se všechny předpoklady o maximální latence všechny operace služby IoT Hub.
* Zřídit služby IoT hub v oblasti Azure, které je nejblíže k zařízení.
* Zvažte použití Azure IoT Edge k provádění operací citlivý na latenci na zařízení nebo na bránu blízko zařízení.

Víc jednotek služby IoT Hub ovlivnit omezení, jak je popsáno výše, ale neposkytuje žádné další latence výhody nebo záruky.
Pokud se zobrazí neočekávané zvýšení latence operace, obraťte se na [Microsoft Support](https://azure.microsoft.com/support/options/).

## <a name="next-steps"></a>Další kroky
Zahrnout další referenční témata v této příručce pro vývojáře IoT Hub:

* [Koncové body centra IoT][lnk-devguide-endpoints]
* [IoT Hub dotazovacího jazyka pro dvojčata zařízení, úlohy a směrování zpráv][lnk-devguide-query]
* [Podpora MQTT centra IoT][lnk-devguide-mqtt]

[lnk-pricing]: https://azure.microsoft.com/pricing/details/iot-hub
[lnk-throttle-blog]: https://azure.microsoft.com/blog/iot-hub-throttling-and-you/
[lnk-importexport]: iot-hub-devguide-identity-registry.md#import-and-export-device-identities

[lnk-devguide-endpoints]: iot-hub-devguide-endpoints.md
[lnk-devguide-query]: iot-hub-devguide-query-language.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md
