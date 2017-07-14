---
title: "Úvod do služby Stream Analytics | Dokumentace Microsoftu"
description: "Přečtěte si o službě Stream Analytics, která umožňuje v reálném čase analyzovat data streamovaná z platformy Internet věcí (IOT)."
keywords: "analytics jako služba, spravované služby, zpracování datového proudu, streamování analytics, co je datový proud analytics"
services: stream-analytics
documentationcenter: 
author: jeffstokes72
manager: jhubbard
editor: cgronlun
ms.assetid: 613c9b01-d103-46e0-b0ca-0839fee94ca8
ms.service: stream-analytics
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 06/16/2017
ms.author: jeffstok
ms.translationtype: Human Translation
ms.sourcegitcommit: 6dbb88577733d5ec0dc17acf7243b2ba7b829b38
ms.openlocfilehash: 421bdfb3132bc8c9f193bcca8d55c9cf9eba1c3b
ms.contentlocale: cs-cz
ms.lasthandoff: 07/04/2017


---

# Co je služba Stream Analytics?
<a id="what-is-stream-analytics" class="xliff"></a>

Azure Stream Analytics je plně spravovaný modul na zpracování událostí, který umožňuje nastavit a provádět analytické výpočty streamovaných dat v reálném čase. Data můžou pocházet ze zařízení, senzorů, webů, informačních kanálů sociálních médií, aplikací, systémů infrastruktury a dalších zdrojů. 

## K čemu můžu službu Stream Analytics používat?
<a id="what-can-i-use-stream-analytics-for" class="xliff"></a>

Pomocí Stream Analytics můžete zkoumat velké objemy dat odesílaných ze zařízení nebo procesů, extrahovat informace z datových proudů a vyhledávat vzory, trendy a vztahy. Podle povahy zkoumaných dat pak můžete provádět různé úlohy aplikací. Můžete například vyvolávat výstrahy, spouštět automatizované pracovní postupy, odesílat informace do nástrojů na vytváření sestav, jako je Power BI, nebo ukládat data pro pozdější analýzu. 

Příklady scénářů Stream Analytics:

* Personalizované analýzy obchodování na burze a výstrahy v reálném čase nabízené finančními institucemi
* Odhalování podvodů v reálném čase prověřováním dat transakcí 
* Služby ochrany dat a identity
* Analýza dat generovaných senzory a poháněcími zařízeními ve fyzických objektech (Internet věcí nebo IoT)
* Analýza navštívených webových stránek
* Aplikace řízení vztahů se zákazníky (CRM), například vydání výstrahy, když se v určitém časovém rámci sníží kvalita zákaznických služeb

## Jak funguje Stream Analytics?
<a id="how-does-stream-analytics-work" class="xliff"></a>

Následující diagram znázorňuje kanál Stream Analytics a ukazuje, jak jsou data ingestována, jak jsou analyzována a jak jsou potom odesílána k prezentování nebo jiným účelům. 

![Kanál Stream Analytics](./media/stream-analytics-introduction/stream_analytics_intro_pipeline.png)

Stream Analytics začíná zdrojem streamovaných dat. Dat je možné ingestovat v Azure ze zařízení pomocí centra událostí Azure nebo centra IoT. Data je také možné získávat z úložišť dat, například z Blob Storage. 

Pokud chcete prozkoumat datový proud, vytvořte *úlohu* Stream Analytics, která bude určovat, odkud data pocházejí. Úloha také definuje *transformaci*&mdash; – to, jak se mají vyhledávat data, vzory nebo vztahy. Stream Analytics podporuje pro tuto úlohu dotazovací jazyk typu SQL, který umožňuje filtrovat, řadit, agregovat a spojovat data streamovaná za určité časové období.

Úloha pak také definuje výstup, kam se mají transformovaná data odesílat. Tak můžete řídit, jaké akce se mají provádět v návaznosti na informace, které jste analyzovali. V návaznosti na analýzu můžete například:

* Poslat příkaz, který změní nastavení zařízení 
* Poslat data do fronty monitorované procesem, který provede akci závislou na zjištěných informacích 
* Poslat data na řídicí panel Power BI za účelem generování sestav
* Poslat data do úložiště, jako je Data Lake Store, databáze SQL Serveru nebo úložiště objektů blob nebo tabulek Azure

Můžete monitorovat probíhající úlohu a upravovat počet událostí, které zpracovává za sekundu. Úlohy můžou také vytvářet diagnostické protokoly k řešení potíží.

## Klíčové funkce a výhody
<a id="key-capabilities-and-benefits" class="xliff"></a>

Služba Stream Analytics byla navržena jako snadno použitelný, flexibilní a ekonomický nástroj, škálovatelný na libovolnou velikost úlohy.

### Připojení k mnoha vstupům a výstupům
<a id="connectivity-to-many-inputs-and-outputs" class="xliff"></a>

Služba Stream Analytics se za účelem zpracování příjmu streamů připojuje přímo ke službám [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/) a [Azure IoT Hubs](https://azure.microsoft.com/services/iot-hub/) a za účelem ingestování historických dat přímo do [služby úložiště Azure Blob](https://docs.microsoft.com/azure/storage/storage-introduction#blob-storage-accounts). Když získáte data z center událostí, můžete Stream Analytics zkombinovat s dalšími zdroji dat a moduly na zpracování událostí.

Vstup úlohy může také zahrnovat referenční data (statická nebo pomalu se měnící data). K těmto referenčním datům můžete připojit streamovaná data, abyste mohli provádět operace vyhledávání stejně jako u databázových dotazů.

Výstupy úloh Stream Analytics můžete směrovat do nejrůznějších služeb. Můžete je zapsat do úložiště, například do objektů blob nebo tabulek Azure Storage, Azure SQL DB, Azure Data Lake Store nebo Azure Cosmos DB. Odtud můžou být data předávána k analýze dávky prostřednictvím Azure HDInsight. Výstupy můžete odesílat do jiných služeb ke zpracování jiným procesem, například do center událostí, front nebo témat Sběrnice Azure. Můžete je také posílat do Power BI, pokud je chcete vizualizovat.

### Snadné používání
<a id="ease-of-use" class="xliff"></a>

K definování transformací slouží jednoduchý deklarativní [dotazovací jazyk Stream Analytics](https://msdn.microsoft.com/library/azure/dn834998.aspx), který umožňuje vytvářet sofistikované analýzy bez znalosti programování. Vstupem pro dotazovací jazyk jsou streamovaná data. Data pak můžete filtrovat, řadit a spojovat (v datovém proudu nebo s referenčními daty), můžete agregovat hodnoty, provádět výpočty a používat geoprostorové funkce. Dotazy můžete na portálu upravovat pomocí IntelliSense a kontroly syntaxe a můžete je testovat pomocí ukázkových dat, která se dají extrahovat z živého datového proudu.

### Rozšiřitelný dotazovací jazyk
<a id="extensible-query-language" class="xliff"></a>

Možnosti dotazovacího jazyka můžete rozšířit definováním a vyvoláním dalších funkcí. Můžete využít výhod řešení Azure Machine Learning a definovat v něm volání funkcí. Pokud chcete provádět složitější výpočty v rámci dotazu Stream Analytics, můžete také integrovat uživatelem definované funkce jazyka JavaScript.

### Škálovatelnost
<a id="scalability" class="xliff"></a>

Stream Analytics dokáže zpracovat až 1 GB příchozích dat za sekundu. Integrace se službou [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/) a [Azure IoT Hubs](https://azure.microsoft.com/services/iot-hub/) umožňuje úlohám ingestovat miliony událostí za sekundu, například událostí odesílaných z připojených zařízení, webů a souborů protokolů. Centra událostí nabízejí funkci oddílů, pomocí které můžete výpočty dělit na logické kroky – ty pak můžete dělit ještě podrobněji, pokud chcete zvýšit škálovatelnost.

### Nízké náklady
<a id="low-cost" class="xliff"></a>

Stream Analytics je cloudová služba, takže umožňuje začít s minimálními náklady. Platíte na základě jednotek datových proudů a tedy míry svého používání ve smyslu objemu zpracovaných dat. Míra využití je založena na objemu zpracovaných událostí a výpočetním výkonu poskytnutém v rámci clusteru ke zpracování úlohy služby Stream Analytics.

### Spolehlivost, rychlé obnovení a opakovatelnost
<a id="reliability-quick-recovery-and-repeatability" class="xliff"></a>

Stream Analytics je spravovaná služba v cloudu, což pomáhá předcházet ztrátě dat a zajišťovat kontinuitu podnikových procesů. V případě selhání použije služba integrované funkce pro obnovení. Díky schopnosti interního zachování stavu služba poskytuje možnost archivace událostí a opakovaného zpracování dat v budoucnosti, které vždy vrátí stejné výsledky. To umožňuje vracet se v čase a při provádění analýz původních příčin a citlivostních analýz opakovaně zkoumat výpočty a jejich výsledky.

## Další kroky
<a id="next-steps" class="xliff"></a>

* Začněte [experimentovat se vstupy a dotazy ze zařízení IoT](stream-analytics-get-started-with-azure-stream-analytics-to-process-data-from-iot-devices.md).
* Sestavte [ucelené řešení Stream Analytics](stream-analytics-real-time-fraud-detection.md), které bude zkoumat telefonní metadata a hledat podvodná volání.
* Seznamte se s dotazovacím jazykem typu SQL pro Stream Analytics a s jedinečnými koncepty, jako jsou [oddílové funkce](stream-analytics-window-functions.md).
* Přečtěte si, jak [škálovat úlohy Stream Analytics](stream-analytics-scale-jobs.md). 
* Přečtěte si, jak [integrovat Stream Analytics a Azure Machine Learning](stream-analytics-machine-learning-integration-tutorial.md).
* Najděte si odpovědi na své otázky ke Stream Analytics ve [fóru Azure Stream Analytics](https://social.msdn.microsoft.com/Forums/home?forum=AzureStreamAnalytics).


