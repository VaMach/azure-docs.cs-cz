---
title: "Seznámení s řešením prediktivní údržby – Azure | Dokumentace Microsoftu"
description: "Návod pro předkonfigurované řešení prediktivní údržby Azure IoT."
services: 
suite: iot-suite
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 3c48a716-b805-4c99-8177-414cc4bec3de
ms.service: iot-suite
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/25/2017
ms.author: dobett
ms.openlocfilehash: 4a430fb250b9145166a3a212d416a4f1c754473f
ms.sourcegitcommit: dfd49613fce4ce917e844d205c85359ff093bb9c
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/31/2017
---
# <a name="predictive-maintenance-preconfigured-solution-walkthrough"></a>Návod pro předkonfigurované řešení prediktivní údržby

Předkonfigurované řešení prediktivní údržby je uceleným řešením pro podnikový scénář, které se pokouší předvídat bod, ve kterém pravděpodobně nastane chyba. Toto předkonfigurované řešení můžete aktivně využívat pro různé činnosti, jako je třeba optimalizace údržby. Řešení kombinuje klíčové služby sady Azure IoT Suite, jako je služba IoT Hub, Stream Analytics a pracovní prostor [Azure Machine Learning][lnk-machine-learning]. Tento pracovní prostor obsahuje model založený na veřejné ukázkové sadě dat, který předpovídá zbývající dobu životnosti (RUL) leteckého motoru. Řešení nabízí úplnou implementaci daného obchodního scénáře IoT jako výchozího bodu pro plánování a implementaci řešení, které vyhovuje vašim konkrétním obchodním požadavkům.

## <a name="logical-architecture"></a>Logická architektura

Následující diagram popisuje logické součásti tohoto předkonfigurovaného řešení:

![][img-architecture]

Modré položky jsou služby Azure zřízené v oblasti, kam jste nasadili předkonfigurované řešení. Seznam oblastí, do kterých můžete nasadit předkonfigurované řešení, najdete na [stránce zřizování][lnk-azureiotsuite].

Zelená položka je simulované zařízení, které představuje letecký motor. Další informace o těchto simulovaných zařízeních najdete v následující části.

Šedé položky představují součásti, které implementují funkce *správy zařízení*. Aktuální verze předkonfigurovaného řešení prediktivní údržby tyto prostředky neposkytuje. Další informace o správě zařízení najdete v tématu [Předkonfigurované řešení pro vzdálené monitorování][lnk-remote-monitoring].

## <a name="simulated-devices"></a>Simulovaná zařízení

V předkonfigurovaných řešeních simulované zařízení představuje letecký motor. Řešení obsahuje dva motory, které jsou součástí jednoho letadla. Každý motor vysílá čtyři typy telemetrických dat: ze snímačů Sensor 9, Sensor 11, Sensor 14 a Sensor 15, které poskytují data potřebná k tomu, aby mohl model Machine Learning vypočítat zbývající dobu životnosti pro tento motor. Každé simulované zařízení posílá do služby IoT Hub následující telemetrické zprávy:

*Počet cyklů*. Cyklus představuje dokončený let trvající 2 až 10 hodin. Během letu se telemetrická data zaznamenávají každou půlhodinu.

*Telemetrická data*. V řešení jsou čtyři snímače, které představují vlastnosti motoru. Snímače jsou obecně označeny jako Sensor 9, Sensor 11, Sensor 14 a Sensor 15. Tyto čtyři snímače představují dostatek telemetrických dat pro získání užitečných výsledků z modelu zbývající doby životnosti. Model použitý v předkonfigurovaném řešení je vytvořený z veřejné sady dat, obsahující data snímačů reálného motoru. Další informace o způsobu vytvoření modelu z původní sady dat najdete v tématu [Šablona prediktivní údržby v galerii Cortana Intelligence][lnk-cortana-analytics].

Simulovaná zařízení mohou v řešení zpracovávat následující příkazy odeslané ze služby IoT Hub:

| Příkaz | Popis |
| --- | --- |
| StartTelemetry |Řídí stav simulace.<br/>Spustí odesílání telemetrických dat ze zařízení |
| StopTelemetry |Řídí stav simulace.<br/>Zastaví odesílání telemetrických dat ze zařízení |

Služba IoT Hub zajišťuje potvrzení příkazu zařízení.

## <a name="azure-stream-analytics-job"></a>Úlohy služby Azure Stream Analytics

**Úloha: Telemetrie** funguje v příchozím datovém proudu telemetrických dat ze zařízení pomocí dvou příkazů:

* První vybere veškerá telemetrická data ze zařízení a odešle je do úložiště objektů blob. Odtud se data vizualizují ve webové aplikaci.
* Druhý vypočítá průměrné hodnoty snímačů v rámci dvouminutového posuvného okna a odešle je prostřednictvím centra událostí do **procesoru událostí**.

## <a name="event-processor"></a>Procesor událostí
**Hostitel procesoru událostí** běží ve webové úloze Azure. **Procesor událostí** přebírá průměrné hodnoty snímačů za dokončený cyklus. Potom tyto hodnoty předá do rozhraní API, které zpřístupní trénovaný model pro výpočet zbývající doby životnosti motoru. Rozhraní API je zveřejněné pracovním prostorem Machine Learning zřízeným jako součást řešení.

## <a name="machine-learning"></a>Machine Learning
Součást Machine Learning používá model odvozený z dat shromážděných z reálných leteckých motorů. Do pracovního prostoru Machine Learning se můžete dostat z dlaždice na stránce zřízeného řešení na webu [azureiotsuite.com][lnk-azureiotsuite]. Tato dlaždice je k dispozici v případě, že je řešení ve stavu **Připraveno**.


## <a name="next-steps"></a>Další kroky
Když jste se seznámili s klíčovými součástmi předkonfigurovaného řešení prediktivní údržby, můžete si jej přizpůsobit. Viz [Doprovodné materiály k přizpůsobení předkonfigurovaných řešení][lnk-customize].

Můžete si taky prostudovat některé další funkce a možnosti předkonfigurovaných řešení sady IoT Suite:

* [Nejčastější dotazy k sadě IoT Suite][lnk-faq]
* [Zabezpečení IoT od počátku][lnk-security-groundup]

[img-architecture]: media/iot-suite-predictive-walkthrough/architecture.png

[lnk-remote-monitoring]: iot-suite-remote-monitoring-sample-walkthrough.md
[lnk-cortana-analytics]: http://gallery.cortanaintelligence.com/Collection/Predictive-Maintenance-Template-3
[lnk-azureiotsuite]: https://www.azureiotsuite.com/
[lnk-customize]: iot-suite-guidance-on-customizing-preconfigured-solutions.md
[lnk-faq]: iot-suite-faq.md
[lnk-security-groundup]: securing-iot-ground-up.md
[lnk-machine-learning]: https://azure.microsoft.com/services/machine-learning/