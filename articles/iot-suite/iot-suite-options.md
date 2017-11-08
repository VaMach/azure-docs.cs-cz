---
title: "Možnosti Microsoft Azure IoT | Dokumentace Microsoftu"
description: "Zvolte, jak implementovat řešení Azure IoT pomocí služeb IoT Suite, IoT Central nebo IoT Hub."
services: 
suite: iot-suite
author: dominicbetts
manager: timlt
ms.assetid: 2d38d08a-4133-4e5c-8b28-f93cadb5df05
ms.service: iot-suite
ms.topic: get-started-article
ms.date: 09/21/2017
ms.author: dobett
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: bd98d42ab391d471d2302066dc2baf2c64f56f55
ms.sourcegitcommit: dfd49613fce4ce917e844d205c85359ff093bb9c
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/31/2017
---
# <a name="compare-azure-iot-options"></a>Porovnání možností Azure IoT

Článek [Azure a Internet věcí](iot-suite-what-is-azure-iot.md) popisuje typickou architekturu řešení IoT s následujícími vrstvami:

* Správa a připojení zařízení
* Zpracování a analýza dat
* Prezentace a obchodní integrace

Azure IoT pro implementaci této architektury nabízí několik možností, které jsou vhodné pro různé sady zákaznických požadavků:

* [Azure IoT Suite](index.md) je kolekce [předkonfigurovaných řešení](iot-suite-what-are-preconfigured-solutions.md) na podnikové úrovni, která je založená na platformě Azure jako službě (PaaS) a umožňuje zrychlit vývoj vlastních řešení IoT.

* [Microsoft IoT Central](https://www.microsoft.com/internet-of-things/iot-central-saas-solutions) je řešení SaaS, které využívá přístup založený na modelu a umožňuje vytvářet řešení IoT na podnikové úrovni, aniž by to vyžadovalo zkušenosti s vývojem cloudových řešení.

## <a name="azure-iot-hub"></a>Azure IoT Hub

Azure IoT Hub je základem platformy Azure jako služby a využívá ho IoT Central i IoT Suite. IoT Hub umožňuje spolehlivou a zabezpečenou obousměrnou komunikaci mezi miliony zařízení IoT a cloudovým řešením. IoT Hub pomáhá plnit náročné úkoly implementace problémy, jako jsou:

* Správa a připojení zařízení velkého rozsahu
* Ingestování telemetrie velkého rozsahu
* Příkazy a ovládání zařízení
* Vynucení zabezpečení zařízení

## <a name="compare-iot-suite-and-iot-central"></a>Porovnání IoT Suite a IoT Central

Volba produktu Azure IoT je důležitou součástí plánování řešení IoT. IoT Hub je individuální služba Azure, která sama o sobě neposkytuje kompletní řešení IoT. IoT Hub můžete použít jako výchozí bod pro jakékoli řešení IoT a k jeho využití nepotřebujete Azure IoT Suite ani Microsoft IoT Central. IoT Suite i IoT Central využívají IoT Hub společně s dalšími službami Azure. Následující tabulka shrnuje hlavní rozdíly mezi IoT Suite a IoT Central a pomůže vám při výběru vhodné varianty pro vaše požadavky:

|                        | IoT Suite | IoT Central |
| ---------------------- | --------- | ----------- |
| Primární využití | Ke zrychlení vývoje vlastního řešení IoT, které vyžaduje maximální flexibilitu | Ke zkrácení doby uvedení na trh pro jednoduchá řešení IoT, která nevyžadují rozsáhlá přizpůsobení služeb |
| Přístup k podkladovým službám PaaS          | Máte přístup k podkladovým službám Azure a můžete je spravovat nebo podle potřeby vyměnit. | SaaS. Plně spravovaná řešení, podkladové služby nejsou zveřejněné. |
| Flexibilita            | Vysoká. Kód pro mikroslužby je typu open source a můžete ho upravit libovolným způsobem podle svých potřeb. Kromě toho můžete přizpůsobit infrastrukturu nasazení.| Střední. Můžete využít předdefinované uživatelské prostředí založené na prohlížeči a přizpůsobit model řešení a aspekty uživatelského rozhraní. Infrastrukturu nejde přizpůsobit, protože jednotlivé komponenty se nezveřejňují.|
| Úroveň dovedností                 | Středně vysoká. K přizpůsobení back-endu řešení potřebujete znalosti Javy nebo .NET. K přizpůsobení vizualizace potřebujete znalosti JavaScriptu. | Nízká. K přizpůsobení řešení potřebujete znalosti modelování. Znalosti kódování nejsou potřeba. |
| Úvodní prostředí | Předkonfigurovaná řešení implementují běžné scénáře IoT. Je možné je nasadit v řádu minut. | Šablony poskytují předdefinované modely. Je možné je nasadit v řádu minut. |
| Ceny                | Služby můžete doladit tak, abyste měli náklady pod kontrolou. | Jednoduchá a předvídatelná struktura cena. |

Rozhodnutí, který produkt využít k sestavení vašeho řešení IoT, určují hlavně:

* Vaše obchodní požadavky
* Typ řešení, které chcete sestavit
* Dovednosti vaší organizace pro sestavování a správu řešení z dlouhodobého hlediska

## <a name="next-steps"></a>Další kroky

Na základě zvoleného produktu a přístupu jsou navrhujeme tyto další kroky:

* **IoT Suite:** [Co jsou předkonfigurovaná řešení Azure IoT Suite?](iot-suite-what-are-preconfigured-solutions.md)
* **IoT Central:** [Microsoft IoT Central](https://www.microsoft.com/internet-of-things/iot-central-saas-solutions).
* **IoT Hub:** [Přehled služby Azure IoT Hub](../iot-hub/iot-hub-what-is-iot-hub.md).
