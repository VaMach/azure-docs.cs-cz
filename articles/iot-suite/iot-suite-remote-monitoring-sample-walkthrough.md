<properties
 pageTitle="Návod pro předkonfigurované řešení vzdáleného monitorování | Microsoft Azure"
 description="Popis předkonfigurovaného řešení Azure IoT pro vzdálené monitorování a jeho architektura."
 services=""
 suite="iot-suite"
 documentationCenter=""
 authors="stevehob"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-suite"
 ms.devlang="na"
 ms.topic="get-started-article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="03/02/2016"
 ms.author="stevehob"/>

# Návod pro předkonfigurované řešení vzdáleného monitorování

## Úvod

Předkonfigurované řešení pro vzdálené monitorování v sadě IoT Suite je ucelené základní řešení pro podnikový scénář, ve kterém se pracuje s více počítači ve vzdálených umístěních. Řešení kombinuje klíčové služby sady Azure IoT Suite, s jejichž pomocí zajišťuje obecnou implementaci podnikového scénáře a je výchozím bodem pro zákazníky, kteří plánují implementaci tohoto typu řešení IoT tak, aby splnilo konkrétní požadavky v jejich podniku.

## Logická architektura

Následující diagram popisuje logické součásti tohoto předkonfigurovaného řešení:

![](media/iot-suite-remote-monitoring-sample-walkthrough/remote-monitoring-architecture.png)


### Simulovaná zařízení

V předkonfigurovaných řešeních představuje simulované zařízení chladící zařízení (například klimatizační jednotku v budově nebo vzduchovod). Každé simulované zařízení posílá do služby IoT Hub následující telemetrické zprávy:


| Zpráva  | Popis |
|----------|-------------|
| Spuštění  | Zařízení po spuštění odešle zprávu s **informacemi o zařízení**, obsahující vybrané údaje o zařízení, například jeho id, metadata, seznam podporovaných příkazů a aktuální konfiguraci zařízení. |


Simulovaná zařízení odesílají v podobě metadat následující vlastnosti:

| Vlastnost               |  Účel |
|------------------------|--------- |
| ID zařízení              | ID, které je zadáno nebo přiřazeno při vytvoření zařízení v řešení |
| Výrobce           | Výrobce zařízení |
| Číslo modelu           | Číslo modelu zařízení |
| Sériové číslo          | Sériové číslo zařízení |
| Firmware               | Aktuální verzi firmwaru v zařízení |
| Platforma               | Architektura platformy zařízení |
| Procesor              | Procesor použitý v zařízení |
| Nainstalovaná paměť          | Velikost paměti RAM nainstalované v zařízení |
| Povolení služby IoT Hub      | Vlastnost popisující stav povolení přístupu ke službě IoT Hub v zařízení |
| Čas vytvoření           | Čas vytvoření zařízení v řešení |
| Čas poslední aktualizace           | Čas poslední aktualizace vlastností zařízení |
| Zeměpisná šířka               | Zeměpisná šířka umístění zařízení |
| Zeměpisná délka              | Zeměpisná délka umístění zařízení |

Simulátor doplňuje pro tyto vlastnosti v simulovaném zařízení vzorové hodnoty.  Pokaždé, když simulátor inicializuje simulované zařízení, odešle toto zařízení do služby IoT Hub předem definovaná metadata. Všimněte si, že tato akce přepíše všechny aktualizace metadat provedené v portálu pro zařízení.


Simulovaná zařízení mohou zpracovávat následující příkazy, odeslané ze služby IoT Hub:

| Příkaz                | Popis                                         |
|------------------------|-----------------------------------------------------|
| PingDevice             | Odešle do zařízení _ping_ pro účely kontroly, zda je dané zařízení aktivní.   |
| StartTelemetry         | Spustí odesílání telemetrických dat ze zařízení                 |
| StopTelemetry          | Zastaví odesílání telemetrických dat zařízením             |
| ChangeSetPointTemp     | Změní hodnotu bodu, na jehož základě se vytvářejí náhodná data |
| DiagnosticTelemetry    | Aktivuje v simulátoru zařízení odesílání další telemetrické hodnoty (externalTemp) |
| ChangeDeviceState      | Změní rozšířené stavové vlastnosti zařízení a odešle z něj zprávu s informacemi o zařízení |


Potvrzení příkazu zařízení je zajišťováno prostřednictvím služby IoT Hub.


### Úlohy služby Azure Stream Analytics


**Úloha 1: Informace o zařízení** filtruje z příchozího datového proudu zprávy s informacemi o zařízení a odesílá je do koncového bodu Centra událostí. Zařízení odesílá zprávy s informacemi o zařízení při spuštění a jako odezvu na příkaz **SendDeviceInfo**. Tato úloha používá následující definici dotazu:

```
SELECT * FROM DeviceDataStream Partition By PartitionId WHERE  ObjectType = 'DeviceInfo'
```

**Úloha 2: Pravidla** vyhodnotí příchozí telemetrická data o teplotě a vlhkosti v porovnání s mezními hodnotami na daném zařízení. Mezní hodnoty se nastavují v editoru pravidel, který je součástí řešení. Každý pár zařízení/hodnota se ukládá pomocí časového razítka v objektu blob, který se do služby Stream Analytics načítá jako **referenční data**. Úloha porovná všechny neprázdné hodnoty s nastavenými mezními hodnotami v zařízení. Při překročení podmínky ' >' bude výstupem úlohy událost **varování**, která signalizuje, že byla překročena mezní hodnota a její součástí budou informace o zařízení, zjištěné hodnotě a časovém razítku. Tato úloha používá následující definici dotazu:

```
WITH AlarmsData AS 
(
SELECT
     Stream.DeviceID,
     'Temperature' as ReadingType,
     Stream.Temperature as Reading,
     Ref.Temperature as Threshold,
     Ref.TemperatureRuleOutput as RuleOutput,
     Stream.EventEnqueuedUtcTime AS [Time]
FROM IoTTelemetryStream Stream
JOIN DeviceRulesBlob Ref ON Stream.DeviceID = Ref.DeviceID
WHERE
     Ref.Temperature IS NOT null AND Stream.Temperature > Ref.Temperature

UNION ALL

SELECT
     Stream.DeviceID,
     'Humidity' as ReadingType,
     Stream.Humidity as Reading,
     Ref.Humidity as Threshold,
     Ref.HumidityRuleOutput as RuleOutput,
     Stream.EventEnqueuedUtcTime AS [Time]
FROM IoTTelemetryStream Stream
JOIN DeviceRulesBlob Ref ON Stream.DeviceID = Ref.DeviceID
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

**Úloha 3: Telemetrická** má v příchozím datovém proudu telemetrických dat dvě funkce. První je odeslání všech telemetrických zpráv ze zařízení do trvalého úložiště objektů blob. Druhou je výpočet průměrné, minimální a maximální hodnoty vlhkosti v pětiminutovém posuvném okně. Tato data jsou taktéž odeslána úložiště objektů blob. Tato úloha používá následující definici dotazu:

```
WITH 
    [StreamData]
AS (
    SELECT
        *
    FROM 
      [IoTHubStream] 
    WHERE
        [ObjectType] IS NULL -- Filter out device info and command responses
) 

SELECT
    *
INTO
    [Telemetry]
FROM
    [StreamData]

SELECT
    DeviceId,
    AVG (Humidity) AS [AverageHumidity], 
    MIN(Humidity) AS [MinimumHumidity], 
    MAX(Humidity) AS [MaxHumidity], 
    5.0 AS TimeframeMinutes 
INTO
    [TelemetrySummary]
FROM
    [StreamData]
WHERE
    [Humidity] IS NOT NULL
GROUP BY
    DeviceId, 
    SlidingWindow (mi, 5)
```

### Procesor událostí

**Procesor událostí** zpracovává zprávy s informacemi o zařízení a odezvy na příkazy. Používá:

- Zprávy s informacemi o zařízení pro účely aktualizace registru zařízení (uloženého v databázi DocumentDB) aktuálními informacemi o zařízení.
- Zprávy s odezvami na příkazy pro účely aktualizace historie příkazů zařízení (uloženou v databázi DocumentDB).

## Začínáme

Tato část vás provede součástmi řešení, popíše zamýšlený případ použití a poskytne vám příklady.

### Řídicí panel pro vzdálené monitorování
Tato stránka ve webové aplikaci používá ovládací prvky PowerBI v jazyce JavaScript (viz [Úložiště vizuálních prvků PowerBI](https://www.github.com/Microsoft/PowerBI-visuals)) k vizualizaci výstupních dat úloh služby Stream Analytics v úložišti objektů blob.


### Portál pro správu zařízení

Tato webová aplikace umožňuje:

- Zřídit nové zařízení, čímž nastavíte jedinečné ID zařízení a vygenerujete pro něj klíč pro ověřování.
- Spravovat vlastnosti zařízení, přičemž mezi tyto aktivity patří zobrazení stávajících vlastností a aktualizace novými vlastnostmi.
- Odesílat příkazy do zařízení.
- Zobrazit historii příkazů pro zařízení.

### Sledování chování cloudového řešení
Zřízené prostředky můžete zobrazit na webu [portál Azure](https://portal.azure.com) ve skupině prostředků se zadaným názvem řešení.

![](media/iot-suite-remote-monitoring-sample-walkthrough/azureportal_01.png)

Při prvním spuštění vzorového řešení jsou jeho součástí čtyři předkonfigurovaná simulovaná zařízení:

![](media/iot-suite-remote-monitoring-sample-walkthrough/solutionportal_01.png)

Pomocí portálu pro správu zařízení můžete přidat nové simulované zařízení:

![](media/iot-suite-remote-monitoring-sample-walkthrough/solutionportal_02.png)

Standardně je nové zařízení na portálu pro správu zařízení ve stavu **Čekající**:

![](media/iot-suite-remote-monitoring-sample-walkthrough/solutionportal_03.png)

Po dokončení nasazení zařízení simulovaného zařízení se zobrazovaný stav zařízení na portálu pro správu zařízení změní na **Spuštěno**, jak je znázorněno na následujícím snímku obrazovky. Úloha **DeviceInfo** služby Stream Analytics odesílá informace o stavu zařízení ze zařízení do portálu pro správu zařízení.

![](media/iot-suite-remote-monitoring-sample-walkthrough/solutionportal_04.png)

Pomocí portálu řešení můžete do zařízení odesílat příkazy, jako je například **ChangeSetPointTemp**:

![](media/iot-suite-remote-monitoring-sample-walkthrough/solutionportal_05.png)

Když zařízení ohlásí, že příkaz úspěšně provedlo, stav se změní na **Úspěch**.

![](media/iot-suite-remote-monitoring-sample-walkthrough/solutionportal_06.png)

Pomocí portálu řešení můžete hledat zařízení s konkrétními vlastnostmi, jako je například číslo modelu:

![](media/iot-suite-remote-monitoring-sample-walkthrough/solutionportal_07.png)

Zařízení můžete zakázat, zakázané zařízení lze následně odebrat:

![](media/iot-suite-remote-monitoring-sample-walkthrough/solutionportal_08.png)


## Další kroky

Následující příspěvky blogu TechNet poskytují další podrobnosti o předkonfigurovaném řešení pro vzdálené monitorování:

- [Sada IoT Suite - Pod pokličkou - Vzdálené monitorování](http://social.technet.microsoft.com/wiki/contents/articles/32941.iot-suite-under-the-hood-remote-monitoring.aspx)
- [Sada IoT Suite - Vzdálené monitorování - Přidávání skutečných a simulovaných zařízení](http://social.technet.microsoft.com/wiki/contents/articles/32975.iot-suite-remote-monitoring-adding-live-and-simulated-devices.aspx)


<!--HONumber=Jun16_HO2-->


