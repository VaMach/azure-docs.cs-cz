---
title: "Návod pro předkonfigurované řešení vzdáleného monitorování | Dokumentace Microsoftu"
description: "Popis předkonfigurovaného řešení Azure IoT pro vzdálené monitorování a jeho architektura."
services: 
suite: iot-suite
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 31fe13af-0482-47be-b4c8-e98e36625855
ms.service: iot-suite
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/17/2016
ms.author: dobett
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 6338750446b33269614c404ecaad8f8192bf1ab2


---
# <a name="remote-monitoring-preconfigured-solution-walkthrough"></a>Návod pro předkonfigurované řešení vzdáleného monitorování
## <a name="introduction"></a>Úvod
[Předkonfigurované řešení][lnk-preconfigured-solutions] vzdáleného monitorování sady IoT Suite je implementace monitorovacího řešení od začátku do konce pro více zařízení spuštěných ve vzdálených umístěních. Řešení kombinuje využívání klíčových služeb Azure a poskytují obecnou implementaci obchodního scénáře. Můžete ho použít jako výchozí bod pro vlastní implementaci. Řešení je možné [přizpůsobit][lnk-customize] podle konkrétních obchodních požadavků vaší firmy.

Tento článek vás provede některými z klíčových prvků řešení vzdáleného monitorování, aby vám pomohl pochopit, jak toto řešení funguje. Díky tomu budete moct:

* Odstraňovat potíže v řešení.
* Naplánujte, jak řešení přizpůsobit podle konkrétních požadavků. 
* Navrhněte vlastní řešení IoT, které používá služby Azure.

## <a name="logical-architecture"></a>Logická architektura
Následující diagram popisuje logické součásti tohoto předkonfigurovaného řešení:

![Logická architektura](media/iot-suite-remote-monitoring-sample-walkthrough/remote-monitoring-architecture.png)

## <a name="simulated-devices"></a>Simulovaná zařízení
V předkonfigurovaných řešeních představuje simulované zařízení chladící zařízení (například klimatizační jednotku v budově nebo vzduchovod). Při nasazení předkonfigurovaných řešení dojde také automaticky ke zřízení čtyř simulovaných zařízení, která běží ve [webových úlohách Azure][lnk-webjobs]. Simulovaná zařízení umožňují snadno zkoumat chování řešení, aniž by bylo nutné nasazovat fyzická zařízení. Pokud chcete nasadit skutečné fyzické zařízení, přečtěte si článek [Připojení zařízení k předkonfigurovanému řešení vzdáleného monitorování][lnk-connect-rm].

Každé simulované zařízení může odesílat do služby IoT Hub následující typy zpráv:

| Zpráva | Popis |
| --- | --- |
| Spuštění |Při spuštění zařízení odešle zprávu **device-info** s informace o sobě na back-end. Tato data zahrnují ID zařízení, metadata zařízení, seznam příkazů, které zařízení podporuje, a aktuální konfiguraci zařízení. |
| Přítomnost |Zařízení pravidelně odesílá zprávu o **presence**, která oznamuje, jestli zařízení rozpoznává přítomnost senzoru. |
| Telemetrická data |Zařízení pravidelně odesílá zprávu **telemetry**, která hlásí simulované hodnoty teploty a vlhkosti získané ze simulovaných senzorů zařízení. |

Simulovaná zařízení odesílají ve zprávě **device-info** následující vlastnosti:

| Vlastnost | Účel |
| --- | --- |
| ID zařízení |ID, které je zadáno nebo přiřazeno při vytvoření zařízení v řešení |
| Výrobce |Výrobce zařízení |
| Číslo modelu |Číslo modelu zařízení |
| Sériové číslo |Sériové číslo zařízení |
| Firmware |Aktuální verzi firmwaru v zařízení |
| Platforma |Architektura platformy zařízení |
| Procesor |Procesor použitý v zařízení |
| Nainstalovaná paměť |Velikost paměti RAM nainstalované v zařízení |
| Povolení služby IoT Hub |Vlastnost popisující stav povolení přístupu ke službě IoT Hub v zařízení |
| Čas vytvoření |Čas vytvoření zařízení v řešení |
| Čas poslední aktualizace |Čas poslední aktualizace vlastností zařízení |
| Zeměpisná šířka |Zeměpisná šířka umístění zařízení |
| Zeměpisná délka |Zeměpisná délka umístění zařízení |

Simulátor doplňuje pro tyto vlastnosti v simulovaném zařízení vzorové hodnoty.  Pokaždé, když simulátor inicializuje simulované zařízení, odešle toto zařízení do služby IoT Hub předem definovaná metadata. Všimněte si, jak tato akce přepíše všechny aktualizace metadat provedené v portálu pro zařízení.

Simulovaná zařízení mohou zpracovávat následující příkazy, odeslané z řídicího panelu řešení pomocí služby IoT Hub:

| Příkaz | Popis |
| --- | --- |
| PingDevice |Odešle do zařízení *ping* pro účely kontroly, jestli je dané zařízení aktivní. |
| StartTelemetry |Spustí odesílání telemetrických dat ze zařízení |
| StopTelemetry |Zastaví odesílání telemetrických dat zařízením |
| ChangeSetPointTemp |Změní hodnotu bodu, na jehož základě se vytvářejí náhodná data |
| DiagnosticTelemetry |Aktivuje v simulátoru zařízení odesílání další telemetrické hodnoty (externalTemp) |
| ChangeDeviceState |Změní rozšířené stavové vlastnosti zařízení a odešle z něj zprávu s informacemi o zařízení |

Potvrzení příkazu zařízení back endu řešení je zajišťováno prostřednictvím služby IoT Hub.

## <a name="iot-hub"></a>IoT Hub
Služba [IoT Hub][lnk-iothub] přijímá data odesílaná ze zařízení do cloudu a zpřístupňuje je úlohám Azure Stream Analytics (ASA). IoT Hub rovněž odesílá příkazy do všech zařízení jménem portálu zařízení. Každá úloha datového proudu ASA používá ke čtení streamu zpráv ze zařízení samostatnou skupinu příjemců IoT Hub.

## <a name="azure-stream-analytics"></a>Azure Stream Analytics
Ve vzdáleném řešení monitorování služba [Azure Stream Analytics][lnk-asa] (ASA) odesílá zprávy zařízení přijaté službou IoT Hub do jiných komponent back endu ke zpracování nebo uložení. Různé úlohy ASA provádějí konkrétní funkce na základě obsahu zpráv.

**Úloha 1: Informace o zařízení** filtruje z příchozího datového proudu zprávy s informacemi o zařízení a odesílá je do koncového bodu Centra událostí. Zařízení odesílá zprávy s informacemi o zařízení při spuštění a jako odezvu na příkaz **SendDeviceInfo**. Tato úloha používá následující definici dotazu k identifikaci zpráv **device-info**:

```
SELECT * FROM DeviceDataStream Partition By PartitionId WHERE  ObjectType = 'DeviceInfo'
```

Tato úloha odesílá svůj výstup do služby Event Hub k dalšímu zpracování.

**Úloha 2: Pravidla** vyhodnotí příchozí telemetrická data o teplotě a vlhkosti v porovnání s mezními hodnotami na daném zařízení. Mezní hodnoty se nastavují v editoru pravidel, který je dostupný na řídicím panelu řešení. Každý pár zařízení/hodnota se ukládá pomocí časového razítka v objektu blob, který služba Stream Analytics načítá jako **referenční data**. Úloha porovná všechny neprázdné hodnoty s nastavenými mezními hodnotami v zařízení. Při překročení podmínky ' >' bude výstupem úlohy událost **alarmu**, která signalizuje, že je překročena mezní hodnota, a její součástí budou informace o zařízení, zjištěné hodnotě a časovém razítku. Tato úloha používá následující definice dotazu k identifikaci telemetrických zpráv, které mohou aktivovat alarm:

```
WITH AlarmsData AS 
(
SELECT
     Stream.IoTHub.ConnectionDeviceId AS DeviceId,
     'Temperature' as ReadingType,
     Stream.Temperature as Reading,
     Ref.Temperature as Threshold,
     Ref.TemperatureRuleOutput as RuleOutput,
     Stream.EventEnqueuedUtcTime AS [Time]
FROM IoTTelemetryStream Stream
JOIN DeviceRulesBlob Ref ON Stream.IoTHub.ConnectionDeviceId = Ref.DeviceID
WHERE
     Ref.Temperature IS NOT null AND Stream.Temperature > Ref.Temperature

UNION ALL

SELECT
     Stream.IoTHub.ConnectionDeviceId AS DeviceId,
     'Humidity' as ReadingType,
     Stream.Humidity as Reading,
     Ref.Humidity as Threshold,
     Ref.HumidityRuleOutput as RuleOutput,
     Stream.EventEnqueuedUtcTime AS [Time]
FROM IoTTelemetryStream Stream
JOIN DeviceRulesBlob Ref ON Stream.IoTHub.ConnectionDeviceId = Ref.DeviceID
WHERE
     Ref.Humidity IS NOT null AND Stream.Humidity > Ref.Humidity
)

SELECT *
INTO DeviceRulesMonitoring
FROM AlarmsData

SELECT *
INTO DeviceRulesHub
FROM AlarmsData
```

Úloha odešle svůj výstup do služby Event Hub k dalšímu zpracování a uloží podrobnosti jednotlivých výstrah do úložiště objektů blob, odkud může informace o výstrahách přečíst řídicí panel řešení.

**Úloha 3: Telemetrická** má v příchozím datovém proudu telemetrických dat dvě funkce. Za prvé odeslání všech telemetrických zpráv ze zařízení do trvalého úložiště objektů blob k dlouhodobému uložení. Za druhé výpočet průměrné, minimální a maximální hodnoty vlhkosti v pětiminutovém posuvném okně a odeslání těchto dat do úložiště objektu blob. Řídicí panel řešení načítá telemetrická data z úložiště objektů blob a vytváří grafy. Tato úloha používá následující definici dotazu:

```
WITH 
    [StreamData]
AS (
    SELECT
        *
    FROM [IoTHubStream]
    WHERE
        [ObjectType] IS NULL -- Filter out device info and command responses
) 

SELECT
    IoTHub.ConnectionDeviceId AS DeviceId,
    Temperature,
    Humidity,
    ExternalTemperature,
    EventProcessedUtcTime,
    PartitionId,
    EventEnqueuedUtcTime,
    * 
INTO
    [Telemetry]
FROM
    [StreamData]

SELECT
    IoTHub.ConnectionDeviceId AS DeviceId,
    AVG (Humidity) AS [AverageHumidity],
    MIN(Humidity) AS [MinimumHumidity],
    MAX(Humidity) AS [MaxHumidity],
    5.0 AS TimeframeMinutes 
INTO
    [TelemetrySummary]
FROM [StreamData]
WHERE
    [Humidity] IS NOT NULL
GROUP BY
    IoTHub.ConnectionDeviceId,
    SlidingWindow (mi, 5)
```

## <a name="event-hubs"></a>Event Hubs
Úlohy služby ASA **informace o zařízení** a **pravidla** odesílají výstupní data do služby Event Hubs, aby bylo zajištěno spolehlivé předání do **Procesoru událostí** spuštěném ve webové úloze.

## <a name="azure-storage"></a>Úložiště Azure
Toto řešení využívá službu Azure Blob Storage k trvalému uchování všech nezpracovaných a souhrnných telemetrických dat ze zařízení v řešení. Řídicí panel načítá telemetrická data z úložiště objektů blob a vytváří grafy. Za účelem zobrazení výstrah řídicí panel načítá data z úložiště objektů, které zaznamenává, když telemetrické hodnoty překračují nakonfigurované prahové hodnoty. Řešení také využívá úložiště objektů blob pro záznam prahových hodnot, které nastavíte v řídicím panelu.

## <a name="webjobs"></a>Webové úlohy
Webové úlohy v řešení kromě hostování simulátorů zařízení také hostují **Procesor událostí** spuštěný ve webové úloze Azure, který zpracovává zprávy s informacemi o zařízeních a odezvy na příkazy. Používá:

* Zprávy s informacemi o zařízení pro účely aktualizace registru zařízení (uloženého v databázi DocumentDB) aktuálními informacemi o zařízení.
* Zprávy s odezvami na příkazy pro účely aktualizace historie příkazů zařízení (uloženou v databázi DocumentDB).

## <a name="documentdb"></a>DocumentDB
Řešení používá k ukládání informací o zařízeních připojených k řešení databázi DocumentDB. Tyto informace zahrnují metadata zařízení a historie příkazů odesílaných do zařízení z řídicího panelu.

## <a name="web-apps"></a>Webové aplikace
### <a name="remote-monitoring-dashboard"></a>Řídicí panel pro vzdálené monitorování
Tato stránka ve webové aplikaci používá ovládací prvky PowerBI v jazyce JavaScript (viz [Úložiště vizuálních prvků PowerBI](https://www.github.com/Microsoft/PowerBI-visuals)) k vizualizaci telemetrických dat ze zařízení. Řešení využívá telemetrickou úlohu ASA k zápisu telemetrických dat do úložiště objektů blob.

### <a name="device-administration-portal"></a>Portál pro správu zařízení
Tato webová aplikace umožňuje:

* Zřízení nového zařízení. Tato akce nastaví jedinečné ID zařízení a vygeneruje klíč pro ověřování. Zapisuje informace o zařízení do registru identity služby IoT Hub a databáze DocumentDB specifické pro řešení.
* Správa vlastností zařízení. Tato akce zahrnuje zobrazení existujících vlastností a aktualizaci novými vlastnostmi.
* Odesílat příkazy do zařízení.
* Zobrazit historii příkazů pro zařízení.
* Povolení a zákaz zařízení.

## <a name="next-steps"></a>Další kroky
Následující příspěvky na blogu TechNet poskytují další podrobnosti o předkonfigurovaném řešení pro vzdálené monitorování:

* [Sada IoT Suite - Pod pokličkou - Vzdálené monitorování](http://social.technet.microsoft.com/wiki/contents/articles/32941.iot-suite-under-the-hood-remote-monitoring.aspx)
* [Sada IoT Suite - Vzdálené monitorování - Přidávání skutečných a simulovaných zařízení](http://social.technet.microsoft.com/wiki/contents/articles/32975.iot-suite-remote-monitoring-adding-live-and-simulated-devices.aspx)

Další informace o sadě IoT Suite najdete v následujících článcích:

* [Připojení zařízení k předkonfigurovanému řešení vzdáleného monitorování][lnk-connect-rm]
* [Oprávnění na webu azureiotsuite.com][lnk-permissions]

[lnk-preconfigured-solutions]: iot-suite-what-are-preconfigured-solutions.md
[lnk-customize]: iot-suite-guidance-on-customizing-preconfigured-solutions.md
[lnk-iothub]: https://azure.microsoft.com/documentation/services/iot-hub/
[lnk-asa]: https://azure.microsoft.com/documentation/services/stream-analytics/
[lnk-webjobs]: https://azure.microsoft.com/documentation/articles/websites-webjobs-resources/
[lnk-connect-rm]: iot-suite-connecting-devices.md
[lnk-permissions]: iot-suite-permissions.md



<!--HONumber=Nov16_HO2-->


