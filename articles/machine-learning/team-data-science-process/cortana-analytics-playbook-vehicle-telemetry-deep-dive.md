---
title: "Podrobné informace do předpovědi vehicle stavu a řídí zvyklosti - Azure | Microsoft Docs"
description: "Pomocí možnosti Cortana Intelligence získat přehledy v reálném čase a prediktivní na vehicle stavu a řídí zvyklosti."
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: d8866fa6-aba6-40e5-b3b3-33057393c1a8
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: bradsev
ms.openlocfilehash: 4050fdc2056df395bbcc37e3783f61eebd90f80a
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="vehicle-telemetry-analytics-solution-playbook-deep-dive-into-the-solution"></a>Scénář řešení analýzy telemetrie vozidla: podrobné informace o řešení
To **nabídky** odkazy na části této playbook: 

[!INCLUDE [cap-vehicle-telemetry-playbook-selector](../../../includes/cap-vehicle-telemetry-playbook-selector.md)]

Tato část podrobněji zanalyzuje data do každé z fází znázorněný v architektuře řešení s pokyny a ukazatele pro přizpůsobení. 

## <a name="data-sources"></a>Zdroje dat
Toto řešení využívá dva různé datové zdroje:

* **simulated vehicle signály a diagnostiky datovou sadu** a 
* **vehicle katalogu**

Vehicle telematika jsme je součástí tohoto řešení. Vysílá diagnostické informace a signály odpovídající stavu nástroj a podporovat jeho vzor k danému bodu v čase. Klikněte na tlačítko [Vehicle telematika simulátoru](http://go.microsoft.com/fwlink/?LinkId=717075) ke stažení **Vehicle telematika simulátoru Visual Studio řešení** přizpůsobení podle svých požadavků. Katalogu vehicle obsahuje odkaz na datovou sadu s VIN pro mapování modelu.

![Vehicle telematika simulátoru](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig1-vehicle-telematics-simulator.png)

*Obrázek 1 – Vehicle telematika simulátoru*

Toto je datová sada formátu JSON, který obsahuje následující schéma.

| Sloupec | Popis | Hodnoty |
| --- | --- | --- |
| VIN |Náhodně generované identifikační číslo |To se získávají z hlavní seznam 10 000 náhodně generované vehicle identifikačními čísly. |
| Mimo teploty |Mimo teploty, kde je nástroj Řízení |Náhodně generované číslo od 0 – 100 |
| Modul teploty |Modul teplota nástroj |Náhodně generované číslo od 0-500 |
| Rychlost |Modul rychlost, jakou se řídí nástroj |Náhodně generované číslo od 0 – 100 |
| Paliva |Úroveň paliva nástroj |Náhodně generované číslo od 0 až 100 (určuje procento úrovně paliva) |
| EngineOil |Úroveň modulu těžba ropy nástroj |Náhodně generované číslo od 0 až 100 (určuje procento úrovně těžba ropy modul) |
| Můžete zadat přetížení |Můžete zadat tlak nástroj |Náhodně generované číslo od 0 – 50 (určuje procento úrovně zatížení můžete zadat) |
| Počítadlo kilometrů |Čtení počítadlo kilometrů nástroj |Náhodně generované číslo od 0 200000 |
| Accelerator_pedal_position |Nástroj Pedálové pozici akcelerátoru |Náhodně generované číslo od 0 až 100 (určuje procento úrovně akcelerátoru) |
| Parking_brake_status |Určuje, zda nástroj zaparkováno nebo ne |True nebo False |
| Headlamp_status |Určuje, kde je světlometu na nebo ne |True nebo False |
| Brake_pedal_status |Určuje, zda je nebo není stisknuta brzdového pedálu |True nebo False |
| Transmission_gear_position |Pozice ozubené kolečko přenosu nástroj |Stavy: první, druhá, třetí a čtvrté, páté, šestého, sedmá, osmého |
| Ignition_status |Určuje, zda je spuštěná nebo zastavená nástroj |True nebo False |
| Windshield_wiper_status |Určuje, zda stírání čelního skla zapnutý, nebo ne |True nebo False |
| ABS |Určuje, zda je nebo není zařazen ABS |True nebo False |
| časové razítko |Časové razítko vytvoření datového bodu |Datum |
| Město |Umístění nástroj |4 města v tomto řešení: Bellevue, Redmond, Sammamish, Praha |

Vehicle modelu referenční datová sada obsahuje VIN mapování modelu. 

| VIN | Model |
| --- | --- |
| FHL3O1SA4IEHB4WU1 |Limuzíny |
| 8J0U8XCPRGW4Z3NQE |Hybridní |
| WORG68Z2PLTNZDBI7 |Rodiny Sedan |
| JTHMYHQTEPP4WBMRN |Limuzíny |
| W9FTHG27LZN1YWO0Y |Hybridní |
| MHTP9N792PHK08WJM |Rodiny Sedan |
| EI4QXI2AXVQQING4I |Limuzíny |
| 5KKR2VB4WHQH97PF8 |Hybridní |
| W9NSZ423XZHAONYXB |Rodiny Sedan |
| 26WJSGHX4MA5ROHNL |Převoditelné |
| GHLUB6ONKMOSI7E77 |Kombi |
| 9C2RHVRVLMEJDBXLP |Compact Car |
| BRNHVMZOUJ6EOCP32 |Malé SUV |
| VCYVW0WUZNBTM594J |Auto sportu |
| HNVCE6YFZSA5M82NY |Střední SUV |
| 4R30FOR7NUOBL05GJ |Kombi |
| WYNIIY42VKV6OQS1J |Velké SUV |
| 8Y5QKG27QET1RBK7I |Velké SUV |
| DF6OX2WSRA6511BVG |Kupé |
| Z2EOZWZBXAEW3E60T |Limuzíny |
| M4TV6IEALD5QDS3IR |Hybridní |
| VHRA1Y2TGTA84F00H |Rodiny Sedan |
| R0JAUHT1L1R3BIKI0 |Limuzíny |
| 9230C202Z60XX84AU |Hybridní |
| T8DNDN5UDCWL7M72H |Rodiny Sedan |
| 4WPYRUZII5YV7YA42 |Limuzíny |
| D1ZVY26UV2BFGHZNO |Hybridní |
| XUF99EW9OIQOMV7Q7 |Rodiny Sedan |
| 8OMCL3LGI7XNCC21U |Převoditelné |
| ……. | |

### <a name="references"></a>Odkazy
[Řešení vehicle telematika simulátoru sady Visual Studio](http://go.microsoft.com/fwlink/?LinkId=717075) 

[Centra událostí Azure](https://azure.microsoft.com/services/event-hubs/)

[Azure Data Factory](https://azure.microsoft.com/documentation/learning-paths/data-factory/)

## <a name="ingestion"></a>Přijímání
Kombinace Azure Event Hubs, Stream Analytics a objektu pro vytváření dat jsou využity k ingestování signály vehicle, diagnostických událostí a v reálném čase a analýza služby batch. Všechny tyto součásti vytvoříte a nakonfigurujete jako součást nasazení řešení. 

### <a name="real-time-analysis"></a>Analýzy v reálném čase
Události vygenerované pomocí simulátoru telematika Vehicle jsou publikovány do centra událostí pomocí sady SDK centra událostí. Úloha Stream Analytics ingestuje tyto události z centra událostí a zpracovává data v reálném čase analyzovat vehicle stavu. 

![Řídicí panel centra událostí](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig4-vehicle-telematics-event-hub-dashboard.png) 

*Obrázek 4 – řídicí panel centra událostí*

![Stream Analytics úlohy zpracování dat](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig5-vehicle-telematics-stream-analytics-job-processing-data.png) 

*Obrázek 5 – úloha Stream Analytics zpracování dat*

Úloha Stream Analytics;

* ingestuje dat z centra událostí 
* provede připojení k u referenčních dat pro mapování vehicle VIN odpovídající modelu 
* dál je do Azure blob storage analytics bohaté batch. 

Následující dotaz služby Stream Analytics se používá k uchování dat do úložiště objektů blob Azure. 

![Dotaz úlohy Stream Analytics přijímání dat](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig6-vehicle-telematics-stream-analytics-job-query-for-data-ingestion.png) 

*Obrázek 6 - Stream Analytics úlohy dotazu pro přijímání dat*

### <a name="batch-analysis"></a>Dávková analýza
Také jsme generují další svazek simulované vehicle signály a diagnostiky datové sady pro širší batch analýzu. To je potřeba zajistit dobrý reprezentativní datový svazek pro dávkové zpracování. Pro tento účel používáme kanál s názvem "PrepareSampleDataPipeline" v pracovním postupu Azure Data Factory ke generování za jeden rok simulované vehicle signály a diagnostiky datové sady. Klikněte na tlačítko [vlastní aktivity služby Data Factory](http://go.microsoft.com/fwlink/?LinkId=717077) ke stažení pro vytváření dat vlastní DotNet aktivita řešení sady Visual Studio pro přizpůsobení podle požadavků. 

![Příprava ukázková data pro dávkové zpracování pracovního postupu](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig7-vehicle-telematics-prepare-sample-data-for-batch-processing.png) 

*Obrázek 7: Příprava ukázková data pro zpracování pracovní postup služby batch*

Kanál se skládá z vlastní .net ADF aktivity, zobrazit tady:

![PrepareSampleDataPipeline aktivity](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig8-vehicle-telematics-prepare-sample-data-pipeline.png) 

*Obrázek 8 - PrepareSampleDataPipeline*

Jakmile kanálu provede úspěšně a datovou sadu "RawCarEventsTable" je označen jako "Připravený", jeden rok vhodné simulované vehicle signálů a diagnostických dat vytváří. Zobrazí následující složku a soubor vytvořený v účtu úložiště v kontejneru "connectedcar":

![Výstup PrepareSampleDataPipeline](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig9-vehicle-telematics-prepare-sample-data-pipeline-output.png) 

*Obrázek 9 - PrepareSampleDataPipeline výstup*

### <a name="references"></a>Odkazy
[Azure SDK centra událostí pro přijímání datového proudu](../../event-hubs/event-hubs-csharp-ephcs-getstarted.md)

[Možnosti přesun dat Azure Data Factory](../../data-factory/v1/data-factory-data-movement-activities.md)
[aktivita služby Azure Data Factory DotNet.](../../data-factory/v1/data-factory-use-custom-activities.md)

[Řešení sady visual studio Azure Data Factory DotNet aktivity pro přípravu ukázková data](http://go.microsoft.com/fwlink/?LinkId=717077) 

## <a name="partition-the-dataset"></a>Oddíl datové sady
Nezpracovaná částečně strukturovaných vehicle signály a diagnostiky datové sady jsou rozděleny do oddílů v rámci přípravy dat do formátu měsíc roku. Toto rozdělení do oddílů podporuje efektivnější dotazování a škálovatelné dlouhodobé úložiště povolíte odolnost převzetí z účtu jeden objekt blob na další jako první účet zaplní. 

>[!NOTE] 
>Tento krok v řešení se vztahuje pouze na dávkové zpracování.

Vstupní a výstupní data správy dat:

* **Výstupní data** (s názvem bez přípony *PartitionedCarEventsTable*) se musí na dlouhou dobu jako základní / "rawest" formu data v zákazníka "Data Lake". 
* **Vstupní data** do tohoto kanálu by obvykle ignorována, protože výstupních dat má úplné věrnosti do vstupní – stačí uložené (oddíly) lépe pro pozdější použití.

![Pracovní postup události car oddílu](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig10-vehicle-telematics-partition-car-events-workflow.png)

*Obrázek 10: pracovní postup události Car oddílu*

Nezpracovaná data je rozdělen na oddíly pomocí aktivitu Hive HDInsight v "PartitionCarEventsPipeline". Ukázková data vygenerované v roce v kroku 1 je rozdělena na oddíly pomocí měsíc roku. Oddíly, které se používají ke generování vehicle signály a diagnostických dat pro každý měsíc (celkový počet 12 oddíly) v roce. 

![PartitionCarEventsPipeline aktivity](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig11-vehicle-telematics-partition-car-events-pipeline.png)

*Obrázek 11 – PartitionCarEventsPipeline*

***Skript PartitionConnectedCarEvents Hive***

Následující skript Hive, s názvem "partitioncarevents.hql", se používá pro vytváření oddílů a je umístěn ve složce "\demo\src\connectedcar\scripts" stažený zip. 
    
    SET hive.exec.dynamic.partition=true;
    SET hive.exec.dynamic.partition.mode = nonstrict;
    set hive.cli.print.header=true;

    DROP TABLE IF EXISTS RawCarEvents; 
    CREATE EXTERNAL TABLE RawCarEvents 
    (
                vin                                string,
                model                            string,
                timestamp                        string,
                outsidetemperature                string,
                enginetemperature                string,
                speed                            string,
                fuel                            string,
                engineoil                        string,
                tirepressure                    string,
                odometer                        string,
                city                            string,
                accelerator_pedal_position        string,
                parking_brake_status            string,
                headlamp_status                    string,
                brake_pedal_status                string,
                transmission_gear_position        string,
                ignition_status                    string,
                windshield_wiper_status            string,
                abs                              string,
                gendate                            string

    ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:RAWINPUT}'; 

    DROP TABLE IF EXISTS PartitionedCarEvents; 
    CREATE EXTERNAL TABLE PartitionedCarEvents 
    (
                vin                                string,
                model                            string,
                timestamp                        string,
                outsidetemperature                string,
                enginetemperature                string,
                speed                            string,
                fuel                            string,
                engineoil                        string,
                tirepressure                    string,
                odometer                        string,
                city                            string,
                accelerator_pedal_position        string,
                parking_brake_status            string,
                headlamp_status                    string,
                brake_pedal_status                string,
                transmission_gear_position        string,
                ignition_status                    string,
                windshield_wiper_status            string,
                abs                              string,
                gendate                            string
    ) partitioned by (YearNo int, MonthNo int) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:PARTITIONEDOUTPUT}';

    DROP TABLE IF EXISTS Stage_RawCarEvents; 
    CREATE TABLE IF NOT EXISTS Stage_RawCarEvents 
    (
                vin                                string,
                model                            string,
                timestamp                        string,
                outsidetemperature                string,
                enginetemperature                string,
                speed                            string,
                fuel                            string,
                engineoil                        string,
                tirepressure                    string,
                odometer                        string,
                city                            string,
                accelerator_pedal_position        string,
                parking_brake_status            string,
                headlamp_status                    string,
                brake_pedal_status                string,
                transmission_gear_position        string,
                ignition_status                    string,
                windshield_wiper_status            string,
                abs                              string,
                gendate                            string,
                YearNo                             int,
                MonthNo                         int) 
    ROW FORMAT delimited fields terminated by ',' LINES TERMINATED BY '10';

    INSERT OVERWRITE TABLE Stage_RawCarEvents
    SELECT
        vin,            
        model,
        timestamp,
        outsidetemperature,
        enginetemperature,
        speed,
        fuel,
        engineoil,
        tirepressure,
        odometer,
        city,
        accelerator_pedal_position,
        parking_brake_status,
        headlamp_status,
        brake_pedal_status,
        transmission_gear_position,
        ignition_status,
        windshield_wiper_status,
        abs,
        gendate,
        Year(gendate),
        Month(gendate)

    FROM RawCarEvents WHERE Year(gendate) = ${hiveconf:Year} AND Month(gendate) = ${hiveconf:Month}; 

    INSERT OVERWRITE TABLE PartitionedCarEvents PARTITION(YearNo, MonthNo) 
    SELECT
        vin,            
        model,
        timestamp,
        outsidetemperature,
        enginetemperature,
        speed,
        fuel,
        engineoil,
        tirepressure,
        odometer,
        city,
        accelerator_pedal_position,
        parking_brake_status,
        headlamp_status,
        brake_pedal_status,
        transmission_gear_position,
        ignition_status,
        windshield_wiper_status,
        abs,
        gendate,
        YearNo,
        MonthNo
    FROM Stage_RawCarEvents WHERE YearNo = ${hiveconf:Year} AND MonthNo = ${hiveconf:Month};

Jakmile kanálu proveden úspěšně, zobrazí se následující oddíly generované ve vašem účtu úložiště v kontejneru "connectedcar".

![Oddílů výstup](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig12-vehicle-telematics-partitioned-output.png)

*Obrázek 12 - oddílů výstup*

Data je nyní optimalizovaná, správu a připraveno pro další zpracování a získáte přehled o bohaté batch. 

## <a name="data-analysis"></a>Analýza dat
V této části najdete postup kombinace Azure Stream Analytics, Azure Machine Learning, Azure Data Factory a Azure HDInsight pro bohaté pokročilé analýzy na vehicle stavu a řídí zvyklosti. Existují tři pododdíly tady:

1. **Strojového učení**: Tato část obsahuje informace o experimentu detekce anomálií, který jsme použili v tomto řešení k předvídání vozidel nutnosti obsluhy údržby a nutnosti navrácení kvůli problémům s zabezpečení.
2. **Analýzy v reálném čase**: Tato část obsahuje informace týkající se analýzy v reálném čase pomocí Stream Analytics Query Language a zprovozňování experimentu strojové učení v reálném čase pomocí vlastní aplikaci.
3. **Batch analysis**: Tato část obsahuje informace týkající se transformace a zpracování dávky dat pomocí Azure HDInsight nebo Azure Machine Learning operationalized službou Azure Data Factory.

### <a name="machine-learning"></a>Machine Learning
Zde Naším cílem je k předvídání vozidel, které vyžadují údržby nebo odvolání na základě statistik určité stavu. Provedeme následující předpoklady

* Pokud platí jedna z následujících tří podmínek, vyžadují vozidel **obsluhy údržby**:
  
  * Můžete zadat naléhavost je nízká
  * Modul těžba ropy úroveň je nízká
  * Modul teploty je vysoká.
* Pokud jeden z následujících podmínek jsou splněny, může mít vozidel **problém zabezpečení** a vyžadovat **odvolání**:
  
  * Modul teploty je vysoká, ale mimo teploty je nízká
  * Modul teploty je nízký, ale mimo teploty je vysoká.

V závislosti na požadavcích předchozí, jsme vytvořili dva samostatné modely k detekci anomálií, jeden pro zjišťování vehicle údržby a jeden pro zjišťování pro vyvolání vehicle. V obou těchto modelů integrované algoritmus hlavní součásti Analysis (PCA) slouží pro zjišťování anomálií. 

**Model pro odhalování údržby**

Pokud jeden z tři indikátory - zatížení můžete zadat, těžba ropy modul nebo modul teploty - splňuje jeho odpovídající stav, model pro odhalování údržby sestavy anomálií. Výsledkem je musíme pouze vzít v úvahu tyto tři proměnné při vytváření modelu. V našem experimentu v Azure Machine Learning, nejprve používáme **výběr sloupců v datové sadě** modulu k extrakci těchto tří proměnných. Modul pro zjišťování anomálií založený na PCA vedle používáme k sestavení modelu detekce anomálií. 

Vytvořeným technika v machine learning, který lze použít k výběru funkcí, klasifikace a detekce anomálií je hlavní součásti analýza (PCA). PCA převede sadu případ obsahující pravděpodobně korelační proměnné, do sady hodnot nazývané jako hlavní komponenty. Klíče představu o na základě PCA modelování je projektu dat na nižší dimenzí místa, aby funkce a anomálií, lze snadno identifikovat.

Pro každý nový vstup model pro odhalování detekce anomálií nejprve vypočítá jeho projekce na eigenvectors a pak vypočítá chyba normalizovaný obnovu. Tato chyba normalizovaný je skóre anomálií. Čím chyby, více neobvyklé instance je. 

V údržby zjišťování problému každý záznam lze považovat za bod v prostorovém 3 prostoru definovaný zatížení můžete zadat, modul těžba ropy a teploty modul souřadnice. Pokud chcete zaznamenat tyto anomálií, jsme můžete promítnout do 2 dimenzí prostoru pomocí PCA původní data v prostorovém 3 prostoru. Proto jsme nastavit parametr počet komponenty pro použití v PCA být 2. Tento parametr hraje důležitou roli při uplatňování detekce anomálií založený na PCA. Po plánování dat pomocí PCA abychom mohli snadno identifikovat tyto anomálií.

**Model detekce anomálií odvolání** v modelu s detekce anomálií odvolání používáme výběr sloupců v datové sadě a anomálií založený na PCA moduly rozpoznávání podobným způsobem. Konkrétně jsme nejprve extrahovat pomocí tří proměnných - mimo teploty a rychlost teploty modul - **výběr sloupců v datové sadě** modulu. Můžeme také zahrnovat proměnnou rychlost vzhledem k tomu, že teplota modul obvykle je vztažen k rychlosti. Další používáme modulu detekce anomálií založený na PCA do projektu data z dimenzí 3 prostoru na 2 dimenzí místa. Jsou splněna kritéria pro vyvolání a proto nástroj při vysoce negativně korelační modul teploty a mimo teploty vyžaduje odvolání. Pomocí algoritmu detekce anomálií založený na PCA, jsme po provedení PCA zachytit anomálií. 

Při tréninku buď modelu, musíme normální data, která nevyžaduje údržby nebo odvolání jako vstupní data pro trénování modelu detekce anomálií založený na PCA použít. V rámci vyhodnocování experimentu používáme modelu detekce anomálií vyškolení ke zjištění, zda nástroj vyžaduje údržby nebo odvolání. 

### <a name="real-time-analysis"></a>Analýzy v reálném čase
Následující dotaz SQL Stream Analytics se používá k získání průměr všech parametrů důležité vehicle jako rychlosti, úroveň paliva, modul teploty, počítadlo kilometrů čtení, můžete zadat naléhavost, modul těžba ropy úroveň a dalších. Průměry se používají k zjišťovat anomálie, vydání výstrahy a určení celkového stavu podmínky vozidel provozovat v určité oblasti a chybě korelace demografické údaje. 

![Stream Analytics query pro zpracování v reálném čase](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig13-vehicle-telematics-stream-analytics-query-for-real-time-processing.png)

*Obrázek 13: dotaz služby Stream Analytics ke zpracování v reálném čase*

Všechny průměry jsou vypočítávány přes TumblingWindow 3 sekundu. TubmlingWindow se používá v tomto případě vzhledem k tomu, že je nutné, aby se a souvislý časové intervaly. 

Další informace o všech možnostech "Oddílová" v Azure Stream Analytics, klikněte na tlačítko [Oddílová (Azure Stream Analytics)](https://msdn.microsoft.com/library/azure/dn835019.aspx).

**V reálném čase předpovědi**

Aplikace je součástí řešení, aby zprovoznit model machine learning v reálném čase. Tuto aplikaci s názvem "RealTimeDashboardApp" je vytvořen a nakonfigurován jako součást nasazení řešení. Aplikace provede následující akce:

1. Naslouchá na instanci Centra událostí, kde Stream Analytics je publikování události ve tvaru nepřetržitě. ![Stream Analytics dotazu pro publikování dat](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig14-vehicle-telematics-stream-analytics-query-for-publishing.png) *obrázek 14: Stream Analytics dotazu pro publikování dat do výstupu instance centra událostí* 
2. Pro každou událost, která obdrží tuto aplikaci: 
   
   * Zpracuje data pomocí koncového bodu Machine Learning požadavků a odpovědí vyhodnocování (záznamy RR). Záznamy o prostředku koncového bodu je automaticky publikován jako součást nasazení.
   * Výstup RRS je publikovaný na datovou sadu Power BI pomocí nabízených rozhraní API.

Tento vzor platí i pro scénáře, ve kterých chcete integrovat obchodní (LoB) aplikace s tokem analýzu v reálném čase, pro scénáře, jako je výstrahy, oznámení a zasílání zpráv.

Klikněte na tlačítko [RealtimeDashboardApp stažení](http://go.microsoft.com/fwlink/?LinkId=717078) ke stažení sady Visual Studio RealtimeDashboardApp řešení pro úpravy. 

**Spuštění aplikace řídicího panelu v reálném čase**
1. Extrahování a uložit místně ![RealtimeDashboardApp složky](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig16-vehicle-telematics-realtimedashboardapp-folder.png) *obrázek 16 – RealtimeDashboardApp složky*  
2. Spuštění aplikace RealtimeDashboardApp.exe
3. Zadejte platné přihlašovací údaje Power BI, přihlášení a klikněte na tlačítko Přijmout ![Přihlášení řídicího panelu v reálném čase aplikace k Power BI.](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig17a-vehicle-telematics-realtimedashboardapp-sign-in-to-powerbi.png) ![Aplikace v reálném čase řídicího panelu dokončit přihlášení k Power BI.](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig17b-vehicle-telematics-realtimedashboardapp-sign-in-to-powerbi.png) 

*Obrázek 17 – RealtimeDashboardApp: Přihlaste se do Power BI*

>[!NOTE] 
>Pokud chcete vyprázdnit datové sady Power BI, spouštění RealtimeDashboardApp s parametrem "flushdata": 

    RealtimeDashboardApp.exe -flushdata


### <a name="batch-analysis"></a>Dávková analýza
Cílem je zobrazit, jak motory Contoso využívá funkce výpočtů Azure a budou využívat velké objemy dat a získáte přehled o bohaté na řízení vzorek, chování využití a stavu vehicle. Díky tomu je možné:

* Zlepšovat prostředí zákazníků a nastavit jej jako levnější tím, že poskytuje přehled na řídí zvyklosti a efektivní řízení chování paliva
* Seznamte se aktivně zákazníků a jejich řízení patters řídit obchodní rozhodnutí a poskytovat nejlepší v třída produkty a služby

V tomto řešení jsme cílení následující metriky:

1. **Agresivní řídí chování**: identifikuje trend modely, umístění, podporovat jeho podmínky, a času v roce a získáte přehled o agresivní řízení vzory. Contoso motory pro můžete použít tyto přehledy marketingových kampaní, řídí přizpůsobené nové funkce a pojišťovnictví na základě využití.
2. **Paliva efektivní řízení chování**: identifikuje trend modely, umístění, podporovat jeho podmínky, a času v roce a získáte přehled o paliva efektivní řízení vzory. Contoso motory pro můžete použít tyto přehledy marketingových kampaní, řídí nové funkce a proaktivní zprávy ovladače pro náklady platné a prostředí zvyklosti popisný řízení. 
3. **Odvolat modely**: identifikuje modely nutnosti navrácení podle zprovozňování detekce anomálií strojového učení experimentu

Podívejme se na podrobné informace o jednotlivých tyto metriky

**Agresivní řízení vzor**

Signály oddílů vehicle a diagnostických dat jsou zpracovány v kanál s názvem "AggresiveDrivingPatternPipeline" použití Hive k určení modely, umístění, vehicle, jízdních podmínek a další parametry, pro jehož agresivní řízení vzor.

![Agresivní řízení pracovního postupu vzor](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig18-vehicle-telematics-aggressive-driving-pattern.png) 
*obrázek 18 – agresivní řízení pracovního postupu vzor*


***Agresivní řízení dotaz Hive vzor***

S názvem "aggresivedriving.hql" použít pro analýzu agresivní řízení vzor podmínku skriptu Hive je umístěný ve složce "\demo\src\connectedcar\scripts" souboru zip staženého. 

    DROP TABLE IF EXISTS PartitionedCarEvents; 
    CREATE EXTERNAL TABLE PartitionedCarEvents
    (
                vin                                string,
                model                            string,
                timestamp                        string,
                outsidetemperature                string,
                enginetemperature                string,
                speed                            string,
                fuel                            string,
                engineoil                        string,
                tirepressure                    string,
                odometer                        string,
                city                            string,
                accelerator_pedal_position        string,
                parking_brake_status            string,
                headlamp_status                    string,
                brake_pedal_status                string,
                transmission_gear_position        string,
                ignition_status                    string,
                windshield_wiper_status            string,
                abs                              string,
                gendate                            string

    ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:PARTITIONEDINPUT}';

    DROP TABLE IF EXISTS CarEventsAggresive; 
    CREATE EXTERNAL TABLE CarEventsAggresive
    (
                   vin                         string, 
                model                        string,
                timestamp                    string,
                city                        string,
                speed                          string,
                transmission_gear_position    string,
                brake_pedal_status            string,
                Year                        string,
                Month                        string

    ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:AGGRESIVEOUTPUT}';



    INSERT OVERWRITE TABLE CarEventsAggresive
    select
    vin,
    model,
    timestamp,
    city,
    speed,
    transmission_gear_position,
    brake_pedal_status,
    "${hiveconf:Year}" as Year,
    "${hiveconf:Month}" as Month
    from PartitionedCarEvents
    where transmission_gear_position IN ('fourth', 'fifth', 'sixth', 'seventh', 'eight') AND brake_pedal_status = '1' AND speed >= '50'


Kombinace ozubené kolečko pozici přenosu na vehicle, brzdy Pedálové stavu a rychlost používá ke zjišťování reckless/agresivní řízení chování v závislosti na brzdění vzor vysokou rychlostí. 

Jakmile kanálu proveden úspěšně, zobrazí se následující oddíly generované ve vašem účtu úložiště v kontejneru "connectedcar".

![Výstup AggressiveDrivingPatternPipeline](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig19-vehicle-telematics-aggressive-driving-pattern-output.png) 

*Obrázek 19 – AggressiveDrivingPatternPipeline výstup*

**Efektivní řízení vzor paliva**

Signály oddílů vehicle a diagnostických dat jsou zpracovány v kanál s názvem "FuelEfficientDrivingPatternPipeline". Hive slouží k určení modely, umístění, vehicle, jízdních podmínek a další vlastnosti, které vykazují paliva efektivní řízení vzorem.

![Zvýšení řízení vzor](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig19-vehicle-telematics-fuel-efficient-driving-pattern.png) 

*Obrázek 20 – zvýšení řízení pracovního postupu vzor*

***Paliva efektivní řízení vzor dotaz Hive***

S názvem "fuelefficientdriving.hql" použít pro analýzu agresivní řízení vzor podmínku skriptu Hive je umístěný ve složce "\demo\src\connectedcar\scripts" souboru zip staženého. 

    DROP TABLE IF EXISTS PartitionedCarEvents; 
    CREATE EXTERNAL TABLE PartitionedCarEvents
    (
                vin                                string,
                model                            string,
                timestamp                        string,
                outsidetemperature                string,
                enginetemperature                string,
                speed                            string,
                fuel                            string,
                engineoil                        string,
                tirepressure                    string,
                odometer                        string,
                city                            string,
                accelerator_pedal_position        string,
                parking_brake_status            string,
                headlamp_status                    string,
                brake_pedal_status                string,
                transmission_gear_position        string,
                ignition_status                    string,
                windshield_wiper_status            string,
                abs                              string,
                gendate                            string

    ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:PARTITIONEDINPUT}';

    DROP TABLE IF EXISTS FuelEfficientDriving; 
    CREATE EXTERNAL TABLE FuelEfficientDriving
    (
                   vin                         string, 
                model                        string,
                   city                        string,
                speed                          string,
                transmission_gear_position    string,                
                brake_pedal_status            string,            
                accelerator_pedal_position    string,                             
                Year                        string,
                Month                        string

    ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:FUELEFFICIENTOUTPUT}';



    INSERT OVERWRITE TABLE FuelEfficientDriving
    select
    vin,
    model,
    city,
    speed,
    transmission_gear_position,
    brake_pedal_status,
    accelerator_pedal_position,
    "${hiveconf:Year}" as Year,
    "${hiveconf:Month}" as Month
    from PartitionedCarEvents
    where transmission_gear_position IN ('fourth', 'fifth', 'sixth', 'seventh', 'eight') AND parking_brake_status = '0' AND brake_pedal_status = '0' AND speed <= '60' AND accelerator_pedal_position >= '50'


Používá kombinaci vehicle na přenos ozubené kolečko pozice, brzdy Pedálové stav, rychlost a akcelerátoru Pedálové pozice ke zjištění paliva efektivní řízení chování v závislosti na akcelerace, brzdění a rychlost vzory. 

Jakmile kanálu proveden úspěšně, zobrazí se následující oddíly generované ve vašem účtu úložiště v kontejneru "connectedcar".

![Výstup FuelEfficientDrivingPatternPipeline](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig20-vehicle-telematics-fuel-efficient-driving-pattern-output.png) 

*Obrázek 21 – FuelEfficientDrivingPatternPipeline výstup*

**Odvolat předpovědi**

Strojového učení experimentu je zřízený a publikovat jako webovou službu, jako součást nasazení řešení. V tomto pracovním postupu, zaregistrován jako služba data factory propojené a operationalized pomocí data factory aktivita dávkového vyhodnocování se využívají záznamy dávkového vyhodnocování koncového bodu.

![Koncový bod Machine Learning](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig21-vehicle-telematics-machine-learning-endpoint.png) 

*Obrázek 22 – strojového učení koncový bod registrován jako propojené služby v datové továrně*

Registrovanou propojenou službu se používá v DetectAnomalyPipeline skóre pro data s využitím modelu detekce anomálií. 

![Počítač Learning aktivita dávkového vyhodnocování v datové továrně](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig22-vehicle-telematics-aml-batch-scoring.png) 

*Obrázek 23 – Azure Machine Learning dávkového vyhodnocování aktivity v objektu pro vytváření dat* 

Je několik kroků provést u tohoto kanálu pro přípravu dat tak, aby může být operationalized s dávkového vyhodnocování webové služby. 

![DetectAnomalyPipeline pro predikci vozidel nutnosti navrácení](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig23-vehicle-telematics-pipeline-predicting-recalls.png) 

*Obrázek 24 – DetectAnomalyPipeline pro predikci vozidel nutnosti navrácení* 

***Dotaz Hive detekce anomálií***

Po dokončení vyhodnocování aktivitu HDInsight se používá ke zpracování a agregovat data, která jsou zařazené do kategorií jako anomálií modelem se skóre pravděpodobnosti 0,60 nebo vyšší.

    DROP TABLE IF EXISTS CarEventsAnomaly; 
    CREATE EXTERNAL TABLE CarEventsAnomaly 
    (
                vin                            string,
                model                        string,
                gendate                        string,
                outsidetemperature            string,
                enginetemperature            string,
                speed                        string,
                fuel                        string,
                engineoil                    string,
                tirepressure                string,
                odometer                    string,
                city                        string,
                accelerator_pedal_position    string,
                parking_brake_status        string,
                headlamp_status                string,
                brake_pedal_status            string,
                transmission_gear_position    string,
                ignition_status                string,
                windshield_wiper_status        string,
                abs                          string,
                maintenanceLabel            string,
                maintenanceProbability        string,
                RecallLabel                    string,
                RecallProbability            string

    ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:ANOMALYOUTPUT}';

    DROP TABLE IF EXISTS RecallModel; 
    CREATE EXTERNAL TABLE RecallModel 
    (

                vin                            string,
                model                        string,
                city                        string,
                outsidetemperature            string,
                enginetemperature            string,
                speed                        string,
                Year                        string,
                Month                        string                

    ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:RECALLMODELOUTPUT}';

    INSERT OVERWRITE TABLE RecallModel
    select
    vin,
    model,
    city,
    outsidetemperature,
    enginetemperature,
    speed,
    "${hiveconf:Year}" as Year,
    "${hiveconf:Month}" as Month
    from CarEventsAnomaly
    where RecallLabel = '1' AND RecallProbability >= '0.60'


Jakmile kanálu proveden úspěšně, zobrazí se následující oddíly generované ve vašem účtu úložiště v kontejneru "connectedcar".

![Výstup DetectAnomalyPipeline](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig24-vehicle-telematics-detect-anamoly-pipeline-output.png) 

*Obrázek 25 – DetectAnomalyPipeline výstup*

## <a name="publish"></a>Publikování

### <a name="real-time-analysis"></a>Analýzy v reálném čase
Některý z těchto dotazů v úloze Stream Analytics publikuje události do výstupu instance centra událostí. 

![Úlohy Stream Analytics publikuje do výstupu instance centra událostí](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig25-vehicle-telematics-stream-analytics-job-publishes-output-event-hub.png)

*Obrázek 26 – Stream Analytics úlohy publikuje do výstupu instance centra událostí*

![Dotaz analýzy datového proudu k publikování výstupu v instanci Centra událostí](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig26-vehicle-telematics-stream-analytics-query-publish-output-event-hub.png)

*Obrázek 27 – Stream Analytics dotaz pro publikování ve výstupu centra událostí instance*

Tento datový proud událostí, které se spotřebovávají RealTimeDashboardApp součástí řešení. Tato aplikace využívá webové služby Machine Learning požadavků a odpovědí pro vyhodnocování v reálném čase a publikuje Výsledná data do Power BI datové sady pro používání. 

### <a name="batch-analysis"></a>Dávková analýza
Výsledky batch a v reálném čase zpracování se publikují do tabulky Azure SQL Database pro používání. Azure SQL Server, databáze a tabulky se vytvoří automaticky v rámci instalační skript. 

![Dávkové zpracování kopírovat výsledky do pracovního postupu datového tržiště](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig27-vehicle-telematics-batch-processing-results-copy-to-data-mart.png)

*Obrázek 28 – dávkového zpracování kopírovat výsledky do pracovního postupu datového tržiště*

![Úlohy Stream Analytics publikuje do datového tržiště](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig28-vehicle-telematics-stream-analytics-job-publishes-to-data-mart.png)

*Obrázek 29 – Stream Analytics úlohy publikuje do datového tržiště*

![Datové Tržiště nastavení v úloze Stream Analytics](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig29-vehicle-telematics-data-mart-setting-in-stream-analytics-job.png)

*Obrázek 30 – datové Tržiště nastavení v úloze Stream Analytics*

## <a name="consume"></a>Konzumace
Power BI poskytuje toto řešení bohaté řídicí panel pro data v reálném čase a vizualizací prediktivní analýzy. 

Kliknutím sem získáte podrobné pokyny k nastavení služby Power BI sestavy a řídicí panel. Poslední řídicí panel vypadá takto:

![Řídicí panel Power BI](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig30-vehicle-telematics-powerbi-dashboard.png)

*Obrázek 31 - řídicí panel Power BI*

## <a name="summary"></a>Souhrn
Tento dokument obsahuje podrobné procházení řešení Analytics Vehicle Telemetrie. To umožňující prezentovat vzor architektura lambda v reálném čase a dávky analytics s předpovědi a akce. Tento vzor platí pro širokou škálu případy použití, které vyžadují aktivní cesta (v reálném čase) a analýzy neaktivní trase (batch). 

