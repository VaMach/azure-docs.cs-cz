---
title: "Úvod do služby Stream Analytics | Dokumentace Microsoftu"
description: "Přečtěte si o službě Stream Analytics, která umožňuje v reálném čase analyzovat data streamovaná z platformy Internet věcí (IOT)."
keywords: "analytics jako služba, spravované služby, zpracování datového proudu, streamování analytics, co je datový proud analytics"
services: stream-analytics
documentationcenter: 
author: samacha
manager: jhubbard
editor: cgronlun
ms.assetid: 613c9b01-d103-46e0-b0ca-0839fee94ca8
ms.service: stream-analytics
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 08/08/2017
ms.author: samacha
ms.openlocfilehash: 6bb70ba7588b8c6e9261fca097403e7fccb041eb
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="what-is-stream-analytics"></a>Co je služba Stream Analytics?

Azure Stream Analytics je plně spravovaný modul na zpracování událostí, který umožňuje nastavit a provádět analytické výpočty streamovaných dat v reálném čase. Data můžou pocházet ze zařízení, senzorů, webů, informačních kanálů sociálních médií, aplikací, systémů infrastruktury a dalších zdrojů. 

## <a name="what-can-i-do-with-stream-analytics"></a>Co se dá se službou Stream Analytics dělat?

Pomocí Stream Analytics můžete zkoumat velké objemy dat odesílaných ze zařízení nebo procesů, extrahovat informace z datových proudů a vyhledávat vzory, trendy a vztahy. Podle povahy zkoumaných dat pak můžete provádět různé úlohy aplikací. Můžete například vyvolávat výstrahy, spouštět automatizované pracovní postupy, odesílat informace do nástrojů na vytváření sestav, jako je Power BI, nebo ukládat data pro pozdější analýzu. 

Příklady:

* Personalizované analýzy obchodování na burze a výstrahy v reálném čase nabízené finančními institucemi
* Odhalování podvodů v reálném čase prověřováním dat transakcí 
* Služby ochrany dat a identity
* Analýza dat generovaných senzory a poháněcími zařízeními ve fyzických objektech (Internet věcí nebo IoT)
* Analýza navštívených webových stránek
* Aplikace řízení vztahů se zákazníky (CRM), například vydání výstrahy, když se v určitém časovém rámci sníží kvalita zákaznických služeb

## <a name="how-does-stream-analytics-work"></a>Jak funguje Stream Analytics?

Tento diagram znázorňuje kanál Stream Analytics a ukazuje, jak jsou data ingestována, analyzována a následně odesílána k prezentování nebo provedení akce. 

![Kanál Stream Analytics](./media/stream-analytics-introduction/stream_analytics_intro_pipeline.png)

Stream Analytics začíná zdrojem streamovaných dat. Dat je možné ingestovat v Azure ze zařízení pomocí centra událostí Azure nebo centra IoT. Data je také možné získávat z úložišť dat, například z Blob Storage. 

Pokud chcete prozkoumat datový proud, vytvořte *úlohu* Stream Analytics, která bude určovat, odkud data pocházejí. Úloha také definuje *transformaci*&mdash; – to, jak se mají vyhledávat data, vzory nebo vztahy. Stream Analytics podporuje pro tuto úlohu dotazovací jazyk typu SQL, který umožňuje filtrovat, řadit, agregovat a spojovat streamovaná data za určité časové období.

Úloha pak také definuje výstup, kam se mají transformovaná data odesílat. Tak můžete řídit, jaké akce se mají provádět v návaznosti na informace, které jste analyzovali. V návaznosti na analýzu můžete například:

* Poslat příkaz, který změní nastavení zařízení 
* Poslat data do fronty monitorované procesem, který provede akci závislou na zjištěných informacích 
* Poslat data na řídicí panel Power BI za účelem generování sestav
* Poslat data do úložiště, jako je Data Lake Store, databáze SQL Serveru nebo úložiště objektů blob nebo tabulek Azure

Probíhající úlohu můžete monitorovat a upravovat počet událostí, které zpracovává za sekundu. Úlohy můžou také vytvářet diagnostické protokoly k řešení potíží.

## <a name="key-capabilities-and-benefits"></a>Klíčové funkce a výhody

Služba Stream Analytics byla navržena jako snadno použitelný, flexibilní a ekonomický nástroj, škálovatelný na libovolnou velikost úlohy.

### <a name="connectivity-to-many-inputs-and-outputs"></a>Připojení k mnoha vstupům a výstupům

Služba Stream Analytics se za účelem ingestování datových proudů připojuje přímo ke službám [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/) a [Azure IoT Hub](https://azure.microsoft.com/services/iot-hub/) a za účelem ingestování historických dat přímo ke [službě Azure Blob Storage](https://docs.microsoft.com/azure/storage/storage-introduction#blob-storage-accounts). Když získáte data z center událostí, můžete Stream Analytics zkombinovat s dalšími zdroji dat a moduly na zpracování událostí.

Vstup úlohy může také zahrnovat referenční data (statická nebo pomalu se měnící data). K těmto referenčním datům můžete připojit streamovaná data, abyste mohli provádět operace vyhledávání stejně jako u databázových dotazů.

Výstup úlohy Stream Analytics můžete směrovat mnoha směry. Můžete ho zapsat do úložiště, například do objektů blob nebo tabulek ve službě Azure Storage, Azure SQL Database, Azure Data Lake Store nebo Azure Cosmos DB. Odtud můžou být data předávána k analýze dávky prostřednictvím Azure HDInsight. Výstupy můžete odesílat do jiných služeb ke zpracování jiným procesem, například do center událostí, front nebo témat Sběrnice Azure. Můžete je také posílat do Power BI, pokud je chcete vizualizovat.

### <a name="ease-of-use"></a>Snadné používání

K definování transformací slouží jednoduchý deklarativní [dotazovací jazyk Stream Analytics](https://msdn.microsoft.com/library/azure/dn834998.aspx), který umožňuje vytvářet sofistikované analýzy bez znalosti programování. Vstupem pro dotazovací jazyk jsou streamovaná data. Data pak můžete filtrovat, řadit a spojovat (v datovém proudu nebo s referenčními daty), můžete agregovat hodnoty, provádět výpočty a používat geoprostorové funkce. Dotazy můžete na portálu upravovat pomocí IntelliSense a kontroly syntaxe a můžete je testovat pomocí ukázkových dat, která se dají extrahovat z živého datového proudu.

### <a name="extensible-query-language"></a>Rozšiřitelný dotazovací jazyk

Možnosti dotazovacího jazyka můžete rozšířit definováním a vyvoláním dalších funkcí. Můžete využít výhod řešení Azure Machine Learning a definovat v něm volání funkcí. Pokud chcete provádět složitější výpočty v rámci dotazu Stream Analytics, můžete také integrovat uživatelem definované funkce jazyka JavaScript.

### <a name="scalability"></a>Škálovatelnost

Stream Analytics dokáže zpracovat až 1 GB příchozích dat za sekundu. Integrace se službami [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/) a [Azure IoT Hub](https://azure.microsoft.com/services/iot-hub/) umožňuje úlohám ingestovat miliony událostí za sekundu, například událostí odesílaných z připojených zařízení, webů a souborů protokolů. Centra událostí nabízejí funkci oddílů, pomocí které můžete výpočty dělit na logické kroky – ty pak můžete dělit ještě podrobněji, pokud chcete zvýšit škálovatelnost.

### <a name="low-cost"></a>Nízké náklady

Stream Analytics je cloudová služba, takže umožňuje začít s minimálními náklady. Platíte na základě jednotek datových proudů a tedy míry svého používání ve smyslu objemu zpracovaných dat. Míra využití je založena na objemu zpracovaných událostí a výpočetním výkonu poskytnutém v rámci clusteru ke zpracování úlohy služby Stream Analytics.

### <a name="reliability-quick-recovery-and-repeatability"></a>Spolehlivost, rychlé obnovení a opakovatelnost

Stream Analytics je spravovaná služba v cloudu, což pomáhá předcházet ztrátě dat a zajišťovat kontinuitu podnikových procesů. V případě selhání použije služba integrované funkce pro obnovení. Díky schopnosti interního zachování stavu služba poskytuje možnost archivace událostí a opakovaného zpracování dat v budoucnosti, které vždy vrátí stejné výsledky. To umožňuje vracet se v čase a při provádění analýz původních příčin a citlivostních analýz opakovaně zkoumat výpočty a jejich výsledky.

## <a name="next-steps"></a>Další kroky

* Začněte [experimentovat se vstupy a dotazy ze zařízení IoT](stream-analytics-get-started-with-azure-stream-analytics-to-process-data-from-iot-devices.md).
* Sestavte [ucelené řešení Stream Analytics](stream-analytics-real-time-fraud-detection.md), které bude zkoumat telefonní metadata a hledat podvodná volání.
* Seznamte se s dotazovacím jazykem typu SQL pro Stream Analytics a s jedinečnými koncepty, jako jsou [oddílové funkce](stream-analytics-window-functions.md).
* Zjistěte, jak [škálovat úlohy Stream Analytics](stream-analytics-scale-jobs.md). 
* Zjistěte, jak [integrovat Stream Analytics a Azure Machine Learning](stream-analytics-machine-learning-integration-tutorial.md).
* Najděte odpovědi na své dotazy ke službě Stream Analytics ve [fóru Azure Stream Analytics](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).

