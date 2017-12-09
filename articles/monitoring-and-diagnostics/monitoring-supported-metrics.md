---
title: "Azure metriky monitorování - podporované metriky na typ prostředku | Microsoft Docs"
description: "Seznam metriky, které jsou dostupné pro jednotlivé typy prostředků s Azure monitorování."
author: anirudhcavale
manager: ashwink
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 63d4ac65-1688-40d1-85c8-7cd408285b0f
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 9/25/2017
ms.author: ancav
ms.openlocfilehash: d66c6760cd2414e377d9c0cf55835a21b4bc5051
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/08/2017
---
# <a name="supported-metrics-with-azure-monitor"></a>Podporované metriky s monitorováním Azure
Monitorování Azure poskytuje několik způsobů, jak pracovat s metriky, včetně grafů, je na portálu, k nim přistupovat pomocí rozhraní REST API nebo je dotazování pomocí prostředí PowerShell nebo rozhraní příkazového řádku. Níže je úplný seznam všech metriky aktuálně k dispozici s Azure monitorování metriky kanálu.

> [!NOTE]
> Další metriky může být k dispozici v portálu nebo pomocí starší verze rozhraní API. Tento seznam obsahuje pouze metriky, které jsou k dispozici konsolidované monitorování Azure metriky zřetězením příkazů. Vyhledat a přístup k metriky s dimenzí použijte prosím [2017-05-01-preview verze rozhraní api.](https://docs.microsoft.com/rest/api/monitor/metricdefinitions)
>
>

## <a name="microsoftanalysisservicesservers"></a>Microsoft.AnalysisServices/servers

|Metrika|Metriky zobrazovaný název|Jednotka|Typ agregace|Popis|Dimenze|
|---|---|---|---|---|---|
|qpu_metric|QPU|Počet|Průměr|QPU. Rozsah 0-100 S1, 0 – 200 S2 a 0 – 400 pro S4|Žádné dimenze|
|memory_metric|Memory (Paměť)|Bajty|Průměr|Paměť. V rozsahu 0-25 GB pro S1, 0 – 50 GB pro S2 a 0 – 100 GB pro S4|Žádné dimenze|
|TotalConnectionRequests|Žádosti o připojení (celkem)|Počet|Průměr|Požadavky na celkový počet připojení. Jedná se o doručení.|Žádné dimenze|
|SuccessfullConnectionsPerSec|Úspěšné připojení za sekundu|CountPerSecond|Průměr|Počet dokončených úspěšné připojení.|Žádné dimenze|
|TotalConnectionFailures|Chyby připojení (celkem)|Počet|Průměr|Celkový počet neúspěšných pokusů o připojení.|Žádné dimenze|
|CurrentUserSessions|Aktuální relace uživatele|Počet|Průměr|Aktuální počet uživatelských relací navázat.|Žádné dimenze|
|QueryPoolBusyThreads|Dotaz z fondu podprocesů zaneprázdněn|Počet|Průměr|Počet vytížených vláken ve fondu vláken dotazu.|Žádné dimenze|
|CommandPoolJobQueueLength|Příkaz délku fronty úloh|Počet|Průměr|Počet úloh ve frontě příkaz fondu vláken.|Žádné dimenze|
|ProcessingPoolJobQueueLength|Délka fronty úloh zpracování fondu|Počet|Průměr|Počet úloh jiný I/O ve frontě fondu zpracování vláken.|Žádné dimenze|
|CurrentConnections|Připojení: Aktuální připojení|Počet|Průměr|Aktuální počet připojení klienta.|Žádné dimenze|
|CleanerCurrentPrice|Paměť: Čisticí aktuální cena|Počet|Průměr|Aktuální cena paměti, $a bajtů/čas, normalizovány na 1000.|Žádné dimenze|
|CleanerMemoryShrinkable|Paměť: Čisticí paměti vypočítat|Bajty|Průměr|Množství paměti v bajtech, podstoupí čisticí vyprazdňování pozadím.|Žádné dimenze|
|CleanerMemoryNonshrinkable|Paměť: Čisticí nonshrinkable paměti|Bajty|Průměr|Množství paměti v bajtech, není v souladu čisticí vyprazdňování pozadím.|Žádné dimenze|
|Parametru MemoryUsage|Paměti: Využití paměti|Bajty|Průměr|Využití paměti procesem serveru v rámci výpočet ceny čisticí paměti. Rovná se čítač Process\PrivateBytes plus velikost dat mapované paměti, ignoruje všechny paměti, které bylo namapované nebo přidělené modul xVelocity analýzy v paměti (VertiPaq) překračující modul xVelocity Limit paměti.|Žádné dimenze|
|MemoryLimitHard|Paměti: Pevný Limit paměti|Bajty|Průměr|Omezení pevné paměti z konfiguračního souboru.|Žádné dimenze|
|MemoryLimitHigh|Paměť: Omezení paměti vysoká|Bajty|Průměr|Limit velkého množství paměti, z konfiguračního souboru.|Žádné dimenze|
|MemoryLimitLow|Paměti: Nízká Limit paměti|Bajty|Průměr|Limit nedostatek paměti z konfiguračního souboru.|Žádné dimenze|
|MemoryLimitVertiPaq|Paměti: VertiPaq Limit paměti|Bajty|Průměr|Omezení v paměti z konfiguračního souboru.|Žádné dimenze|
|Kvóta|Paměť: kvóty|Bajty|Průměr|Aktuální kvótu paměti, v bajtech. Kvótu paměti se také označuje jako rezervace paměti grant nebo paměti.|Žádné dimenze|
|QuotaBlocked|Paměti: Blokované kvótu|Počet|Průměr|Aktuální počet požadavků kvóty, které jsou blokovaný, dokud jsou uvolněny kvóty další paměť.|Žádné dimenze|
|VertiPaqNonpaged|Paměť: VertiPaq nestránkovaného fondu|Bajty|Průměr|Bajtů paměti uzamčena v pracovní sadě pro použití stroj v paměti.|Žádné dimenze|
|VertiPaqPaged|Paměť: VertiPaq stránkovaného fondu|Bajty|Průměr|Bajty stránkovaného paměti používané pro data v paměti.|Žádné dimenze|
|RowsReadPerSec|Zpracování: Řádky čtení za sekundu|CountPerSecond|Průměr|Počet řádků přečíst ze všech relačních databází.|Žádné dimenze|
|RowsConvertedPerSec|Zpracování: Řádky převést za sekundu|CountPerSecond|Průměr|Počet řádků převést během zpracování.|Žádné dimenze|
|RowsWrittenPerSec|Zpracování: Řádků zapsaných za sekundu|CountPerSecond|Průměr|Počet řádků zapsaných během zpracování.|Žádné dimenze|
|CommandPoolBusyThreads|Vláken: Příkaz zaneprázdněn z fondu podprocesů|Počet|Průměr|Počet vytížených vláken ve fondu vláken příkaz.|Žádné dimenze|
|CommandPoolIdleThreads|Vláken: Příkaz nečinných vláken fondu|Počet|Průměr|Počet nečinných vláken ve fondu vláken příkaz.|Žádné dimenze|
|LongParsingBusyThreads|Vláken: Analýza zaneprázdněn vláken dlouho|Počet|Průměr|Počet vytížených vláken ve fondu vláken dlouho analýzy.|Žádné dimenze|
|LongParsingIdleThreads|Vláken: Analýza nečinných vláken dlouho|Počet|Průměr|Počet nečinných vláken ve fondu vláken dlouho analýzy.|Žádné dimenze|
|LongParsingJobQueueLength|Vláken: Analýza dlouho délka fronty úloh|Počet|Průměr|Počet úloh ve frontě dlouho analýzy fondu vláken.|Žádné dimenze|
|ProcessingPoolBusyIOJobThreads|Vláken: Fond zaneprázdněn vstupně-výstupní úlohy vláken zpracování|Počet|Průměr|Počet vláken, které jsou spuštěné úlohy vstupně-výstupních operací ve fondu zpracování vláken.|Žádné dimenze|
|ProcessingPoolBusyNonIOThreads|Vláken: Zaneprázdněný jiný I/O vláken fondu zpracování|Počet|Průměr|Počet vláken, které jsou spuštěné úlohy bez I/O ve fondu zpracování vláken.|Žádné dimenze|
|ProcessingPoolIOJobQueueLength|Vláken: Fond délka fronty vstupně-výstupní úlohy zpracování|Počet|Průměr|Počet vstupně-výstupních úloh ve frontě fondu zpracování vláken.|Žádné dimenze|
|ProcessingPoolIdleIOJobThreads|Vláken: Fond nečinnosti vstupně-výstupní úlohy vláken zpracování|Počet|Průměr|Počet vstupně-výstupních úloh ve fondu zpracování vláken nečinných vláken.|Žádné dimenze|
|ProcessingPoolIdleNonIOThreads|Vláken: Nečinnosti vláken jiný I/O fondu zpracování|Počet|Průměr|Počet nečinných vláken ve fondu zpracování vláken, který je vyhrazený pro jiný I/O úlohy.|Žádné dimenze|
|QueryPoolIdleThreads|Vláken: Dotaz nečinných vláken fondu|Počet|Průměr|Počet vstupně-výstupních úloh ve fondu zpracování vláken nečinných vláken.|Žádné dimenze|
|QueryPoolJobQueueLength|Vláken: Dotaz lengt fronty úloh fondu|Počet|Průměr|Počet úloh ve frontě fondu vláken dotazu.|Žádné dimenze|
|ShortParsingBusyThreads|Vláken: Analýza zaneprázdněn vláken krátké|Počet|Průměr|Počet vytížených vláken v krátké analýzy fondu vláken.|Žádné dimenze|
|ShortParsingIdleThreads|Vláken: Analýza nečinných vláken krátké|Počet|Průměr|Počet nečinných vláken v krátké analýzy fondu vláken.|Žádné dimenze|
|ShortParsingJobQueueLength|Vláken: Analýza délka fronty úloh krátké|Počet|Průměr|Počet úloh ve frontě krátké analýzy fondu vláken.|Žádné dimenze|
|memory_thrashing_metric|Zahlcení paměti|Procento|Průměr|Průměrná paměti zahlcení.|Žádné dimenze|
|mashup_engine_qpu_metric|Modul M QPU|Počet|Průměr|Využití QPU procesy modul hybridní webové aplikace|Žádné dimenze|
|mashup_engine_memory_metric|M modul paměti|Bajty|Průměr|Využití paměti procesy modul hybridní webové aplikace|Žádné dimenze|

## <a name="microsoftapimanagementservice"></a>Microsoft.ApiManagement/service

|Metrika|Metriky zobrazovaný název|Jednotka|Typ agregace|Popis|Dimenze|
|---|---|---|---|---|---|
|TotalRequests|Celkový počet brány požadavků|Počet|Celkem|Počet požadavků na bránu|Umístění, název hostitele|
|SuccessfulRequests|Požadavky pro úspěšné brány|Počet|Celkem|Počet požadavků úspěšná brány|Umístění, název hostitele|
|UnauthorizedRequests|Požadavky neoprávněným brány|Počet|Celkem|Počet požadavků neoprávněným brány|Umístění, název hostitele|
|FailedRequests|Požadavky na bránu|Počet|Celkem|Počet selhání v žádostech o brány|Umístění, název hostitele|
|OtherRequests|Ostatní požadavky brány|Počet|Celkem|Počet požadavků na jiné brány|Umístění, název hostitele|
|Doba trvání|Celková doba trvání brány požadavků|počet milisekund|Průměr|Celkové doby trvání z brány požadavků v milisekundách|Umístění, název hostitele|
|Kapacita|Kapacita (Preview)|Procento|Maximální počet|Metrika využití služby ApiManagement|Umístění|

## <a name="microsoftautomationautomationaccounts"></a>Microsoft.Automation/automationAccounts

|Metrika|Metriky zobrazovaný název|Jednotka|Typ agregace|Popis|Dimenze|
|---|---|---|---|---|---|
|TotalJob|Celkový počet úloh|Počet|Celkem|Celkový počet úloh|Žádné dimenze|

## <a name="microsoftbatchbatchaccounts"></a>Microsoft.Batch/batchAccounts

|Metrika|Metriky zobrazovaný název|Jednotka|Typ agregace|Popis|Dimenze|
|---|---|---|---|---|---|
|CoreCount|Vyhrazené počet jader|Počet|Celkem|Celkový počet vyhrazených jader na účtu batch|Žádné dimenze|
|TotalNodeCount|Počet vyhrazených uzlů|Počet|Celkem|Celkový počet vyhrazených uzlů na účtu batch|Žádné dimenze|
|LowPriorityCoreCount|Počet jader LowPriority|Počet|Celkem|Celkový počet nízkou prioritu jader na účtu batch|Žádné dimenze|
|TotalLowPriorityNodeCount|Počet uzlů s nízkou prioritou|Počet|Celkem|Celkový počet uzlů nízkou prioritu na účtu batch|Žádné dimenze|
|CreatingNodeCount|Vytváření počet uzlů|Počet|Celkem|Počet uzlů, které vytváří|Žádné dimenze|
|StartingNodeCount|Počáteční počet uzlů|Počet|Celkem|Počet uzlů spouštění|Žádné dimenze|
|WaitingForStartTaskNodeCount|Čeká se na pro počet uzlů spuštění úloh|Počet|Celkem|Počet uzlů čekání na dokončení úlohy spustit|Žádné dimenze|
|StartTaskFailedNodeCount|Spuštění úlohy se nezdařilo počet uzlů|Počet|Celkem|Počet uzlů, kde se nezdařilo spustit úlohu|Žádné dimenze|
|IdleNodeCount|Počet nečinných uzlů|Počet|Celkem|Počet nečinných uzlů|Žádné dimenze|
|OfflineNodeCount|Offline počet uzlů|Počet|Celkem|Počet offline uzlů|Žádné dimenze|
|RebootingNodeCount|Restartování počet uzlů|Počet|Celkem|Počet restartování uzlů|Žádné dimenze|
|ReimagingNodeCount|Obnovování počet uzlů|Počet|Celkem|Počet uzlů obnovování|Žádné dimenze|
|RunningNodeCount|Spuštění počet uzlů|Počet|Celkem|Počet spuštěných uzly|Žádné dimenze|
|LeavingPoolNodeCount|Opouštění počet uzlů fondu|Počet|Celkem|Počet uzlů a fondu|Žádné dimenze|
|UnusableNodeCount|Nepoužitelná počet uzlů|Počet|Celkem|Počet nepoužitelná uzlů|Žádné dimenze|
|PreemptedNodeCount|Zrušené počet uzlů|Počet|Celkem|Počet uzlů zrušené|Žádné dimenze|
|TaskStartEvent|Úloha spuštění události|Počet|Celkem|Celkový počet úloh, které byly zahájeny|Žádné dimenze|
|TaskCompleteEvent|Dokončení událostí úlohy|Počet|Celkem|Celkový počet úloh, které byly dokončeny|Žádné dimenze|
|TaskFailEvent|Událostí selhání úlohy|Počet|Celkem|Celkový počet úloh, které byly dokončeny ve stavu selhání|Žádné dimenze|
|PoolCreateEvent|Události vytváření fondu|Počet|Celkem|Celkový počet fondy, které byly vytvořeny|Žádné dimenze|
|PoolResizeStartEvent|Události spuštění změny velikosti fondu|Počet|Celkem|Celkový počet změní velikost fondu, které byly zahájeny|Žádné dimenze|
|PoolResizeCompleteEvent|Dokončení události změny velikosti fondu|Počet|Celkem|Celkový počet změní velikost fondu, které byly dokončeny|Žádné dimenze|
|PoolDeleteStartEvent|Události spuštění odstranění fondu|Počet|Celkem|Celkový počet odstranění fondu, které byly zahájeny|Žádné dimenze|
|PoolDeleteCompleteEvent|Události dokončení odstranění fondu|Počet|Celkem|Celkový počet odstranění fondu, které byly dokončeny|Žádné dimenze|

## <a name="microsoftcacheredis"></a>Microsoft.Cache/redis

|Metrika|Metriky zobrazovaný název|Jednotka|Typ agregace|Popis|Dimenze|
|---|---|---|---|---|---|
|connectedclients|Připojení klienti|Počet|Maximální počet||Žádné dimenze|
|totalcommandsprocessed|Celkový počet operací|Počet|Celkem||Žádné dimenze|
|mezipaměti|Přístupů k mezipaměti|Počet|Celkem||Žádné dimenze|
|cachemisses|Neúspěšné přístupy do mezipaměti|Počet|Celkem||Žádné dimenze|
|getcommands|Získá|Počet|Celkem||Žádné dimenze|
|setcommands|Nastaví|Počet|Celkem||Žádné dimenze|
|evictedkeys|Vyřazené klíče|Počet|Celkem||Žádné dimenze|
|totalkeys|Celkový počet klíčů|Počet|Maximální počet||Žádné dimenze|
|expiredkeys|Vypršela platnost klíče|Počet|Celkem||Žádné dimenze|
|usedmemory|Použitá paměť|Bajty|Maximální počet||Žádné dimenze|
|usedmemoryRss|Použitá paměť RSS|Bajty|Maximální počet||Žádné dimenze|
|serverLoad|Zatížení serveru|Procento|Maximální počet||Žádné dimenze|
|cacheWrite|Mezipaměť zápisu|BytesPerSecond|Maximální počet||Žádné dimenze|
|cacheRead|Mezipaměti pro čtení|BytesPerSecond|Maximální počet||Žádné dimenze|
|percentProcessorTime|Procesor|Procento|Maximální počet||Žádné dimenze|
|connectedclients0|Připojených klientů (horizontálního oddílu 0)|Počet|Maximální počet||Žádné dimenze|
|totalcommandsprocessed0|Celkový počet operací (horizontálního oddílu 0)|Počet|Celkem||Žádné dimenze|
|cachehits0|Přístupů k mezipaměti (horizontálního oddílu 0)|Počet|Celkem||Žádné dimenze|
|cachemisses0|Neúspěšné přístupy do mezipaměti (horizontálního oddílu 0)|Počet|Celkem||Žádné dimenze|
|getcommands0|Získá (horizontálního oddílu 0)|Počet|Celkem||Žádné dimenze|
|setcommands0|Nastaví (horizontálního oddílu 0)|Počet|Celkem||Žádné dimenze|
|evictedkeys0|Vyřazené klíče (horizontálního oddílu 0)|Počet|Celkem||Žádné dimenze|
|totalkeys0|Celkový počet klíčů (horizontálního oddílu 0)|Počet|Maximální počet||Žádné dimenze|
|expiredkeys0|Vypršela platnost klíče (horizontálního oddílu 0)|Počet|Celkem||Žádné dimenze|
|usedmemory0|Použitá paměť (horizontálního oddílu 0)|Bajty|Maximální počet||Žádné dimenze|
|usedmemoryRss0|Použitá paměť RSS (horizontálního oddílu 0)|Bajty|Maximální počet||Žádné dimenze|
|serverLoad0|Zatížení serveru (horizontálního oddílu 0)|Procento|Maximální počet||Žádné dimenze|
|cacheWrite0|Mezipaměť zápisu (horizontálního oddílu 0)|BytesPerSecond|Maximální počet||Žádné dimenze|
|cacheRead0|Mezipaměti pro čtení (horizontálního oddílu 0)|BytesPerSecond|Maximální počet||Žádné dimenze|
|percentProcessorTime0|Procesor (horizontálního oddílu 0)|Procento|Maximální počet||Žádné dimenze|
|connectedclients1|Připojených klientů (horizontálních 1)|Počet|Maximální počet||Žádné dimenze|
|totalcommandsprocessed1|Celkový počet operací (horizontálních 1)|Počet|Celkem||Žádné dimenze|
|cachehits1|Přístupů k mezipaměti (horizontálních 1)|Počet|Celkem||Žádné dimenze|
|cachemisses1|Neúspěšné přístupy do mezipaměti (horizontálních 1)|Počet|Celkem||Žádné dimenze|
|getcommands1|Získá (horizontálních 1)|Počet|Celkem||Žádné dimenze|
|setcommands1|Nastaví (horizontálních 1)|Počet|Celkem||Žádné dimenze|
|evictedkeys1|Vyřazené klíče (horizontálních 1)|Počet|Celkem||Žádné dimenze|
|totalkeys1|Celkový počet klíčů (horizontálních 1)|Počet|Maximální počet||Žádné dimenze|
|expiredkeys1|Vypršela platnost klíče (horizontálních 1)|Počet|Celkem||Žádné dimenze|
|usedmemory1|Použitá paměť (horizontálních 1)|Bajty|Maximální počet||Žádné dimenze|
|usedmemoryRss1|Použitá paměť RSS (horizontálních 1)|Bajty|Maximální počet||Žádné dimenze|
|serverLoad1|Zatížení serveru (horizontálních 1)|Procento|Maximální počet||Žádné dimenze|
|cacheWrite1|Mezipaměť zápisu (horizontálních 1)|BytesPerSecond|Maximální počet||Žádné dimenze|
|cacheRead1|Mezipaměti pro čtení (horizontálních 1)|BytesPerSecond|Maximální počet||Žádné dimenze|
|percentProcessorTime1|Procesor (horizontálních 1)|Procento|Maximální počet||Žádné dimenze|
|connectedclients2|Připojených klientů (horizontálních 2)|Počet|Maximální počet||Žádné dimenze|
|totalcommandsprocessed2|Celkový počet operací (horizontálních 2)|Počet|Celkem||Žádné dimenze|
|cachehits2|Přístupů k mezipaměti (horizontálních 2)|Počet|Celkem||Žádné dimenze|
|cachemisses2|Neúspěšné přístupy do mezipaměti (horizontálních 2)|Počet|Celkem||Žádné dimenze|
|getcommands2|Získá (horizontálních 2)|Počet|Celkem||Žádné dimenze|
|setcommands2|Nastaví (horizontálních 2)|Počet|Celkem||Žádné dimenze|
|evictedkeys2|Vyřazené klíče (horizontálních 2)|Počet|Celkem||Žádné dimenze|
|totalkeys2|Celkový počet klíčů (horizontálních 2)|Počet|Maximální počet||Žádné dimenze|
|expiredkeys2|Vypršela platnost klíče (horizontálních 2)|Počet|Celkem||Žádné dimenze|
|usedmemory2|Použitá paměť (horizontálních 2)|Bajty|Maximální počet||Žádné dimenze|
|usedmemoryRss2|Použitá paměť RSS (horizontálních 2)|Bajty|Maximální počet||Žádné dimenze|
|serverLoad2|Zatížení serveru (horizontálních 2)|Procento|Maximální počet||Žádné dimenze|
|cacheWrite2|Mezipaměť zápisu (horizontálních 2)|BytesPerSecond|Maximální počet||Žádné dimenze|
|cacheRead2|Mezipaměti pro čtení (horizontálních 2)|BytesPerSecond|Maximální počet||Žádné dimenze|
|percentProcessorTime2|Procesor (horizontálních 2)|Procento|Maximální počet||Žádné dimenze|
|connectedclients3|Připojených klientů (horizontálních 3)|Počet|Maximální počet||Žádné dimenze|
|totalcommandsprocessed3|Celkový počet operací (horizontálních 3)|Počet|Celkem||Žádné dimenze|
|cachehits3|Přístupů k mezipaměti (horizontálních 3)|Počet|Celkem||Žádné dimenze|
|cachemisses3|Neúspěšné přístupy do mezipaměti (horizontálních 3)|Počet|Celkem||Žádné dimenze|
|getcommands3|Získá (horizontálních 3)|Počet|Celkem||Žádné dimenze|
|setcommands3|Nastaví (horizontálních 3)|Počet|Celkem||Žádné dimenze|
|evictedkeys3|Vyřazené klíče (horizontálních 3)|Počet|Celkem||Žádné dimenze|
|totalkeys3|Celkový počet klíčů (horizontálních 3)|Počet|Maximální počet||Žádné dimenze|
|expiredkeys3|Vypršela platnost klíče (horizontálních 3)|Počet|Celkem||Žádné dimenze|
|usedmemory3|Použitá paměť (horizontálních 3)|Bajty|Maximální počet||Žádné dimenze|
|usedmemoryRss3|Použitá paměť RSS (horizontálních 3)|Bajty|Maximální počet||Žádné dimenze|
|serverLoad3|Zatížení serveru (horizontálních 3)|Procento|Maximální počet||Žádné dimenze|
|cacheWrite3|Mezipaměť zápisu (horizontálních 3)|BytesPerSecond|Maximální počet||Žádné dimenze|
|cacheRead3|Mezipaměti pro čtení (horizontálních 3)|BytesPerSecond|Maximální počet||Žádné dimenze|
|percentProcessorTime3|Procesor (horizontálních 3)|Procento|Maximální počet||Žádné dimenze|
|connectedclients4|Připojených klientů (horizontálních 4)|Počet|Maximální počet||Žádné dimenze|
|totalcommandsprocessed4|Celkový počet operací (horizontálních 4)|Počet|Celkem||Žádné dimenze|
|cachehits4|Přístupů k mezipaměti (horizontálních 4)|Počet|Celkem||Žádné dimenze|
|cachemisses4|Neúspěšné přístupy do mezipaměti (horizontálních 4)|Počet|Celkem||Žádné dimenze|
|getcommands4|Získá (horizontálních 4)|Počet|Celkem||Žádné dimenze|
|setcommands4|Nastaví (horizontálních 4)|Počet|Celkem||Žádné dimenze|
|evictedkeys4|Vyřazené klíče (horizontálních 4)|Počet|Celkem||Žádné dimenze|
|totalkeys4|Celkový počet klíčů (horizontálních 4)|Počet|Maximální počet||Žádné dimenze|
|expiredkeys4|Vypršela platnost klíče (horizontálních 4)|Počet|Celkem||Žádné dimenze|
|usedmemory4|Použitá paměť (horizontálních 4)|Bajty|Maximální počet||Žádné dimenze|
|usedmemoryRss4|Použitá paměť RSS (horizontálních 4)|Bajty|Maximální počet||Žádné dimenze|
|serverLoad4|Zatížení serveru (horizontálních 4)|Procento|Maximální počet||Žádné dimenze|
|cacheWrite4|Mezipaměť zápisu (horizontálních 4)|BytesPerSecond|Maximální počet||Žádné dimenze|
|cacheRead4|Mezipaměti pro čtení (horizontálních 4)|BytesPerSecond|Maximální počet||Žádné dimenze|
|percentProcessorTime4|Procesor (horizontálních 4)|Procento|Maximální počet||Žádné dimenze|
|connectedclients5|Připojených klientů (horizontálních 5)|Počet|Maximální počet||Žádné dimenze|
|totalcommandsprocessed5|Celkový počet operací (horizontálních 5)|Počet|Celkem||Žádné dimenze|
|cachehits5|Přístupů k mezipaměti (horizontálních 5)|Počet|Celkem||Žádné dimenze|
|cachemisses5|Neúspěšné přístupy do mezipaměti (horizontálních 5)|Počet|Celkem||Žádné dimenze|
|getcommands5|Získá (horizontálních 5)|Počet|Celkem||Žádné dimenze|
|setcommands5|Nastaví (horizontálních 5)|Počet|Celkem||Žádné dimenze|
|evictedkeys5|Vyřazené klíče (horizontálních 5)|Počet|Celkem||Žádné dimenze|
|totalkeys5|Celkový počet klíčů (horizontálních 5)|Počet|Maximální počet||Žádné dimenze|
|expiredkeys5|Vypršela platnost klíče (horizontálních 5)|Počet|Celkem||Žádné dimenze|
|usedmemory5|Použitá paměť (horizontálních 5)|Bajty|Maximální počet||Žádné dimenze|
|usedmemoryRss5|Použitá paměť RSS (horizontálních 5)|Bajty|Maximální počet||Žádné dimenze|
|serverLoad5|Zatížení serveru (horizontálních 5)|Procento|Maximální počet||Žádné dimenze|
|cacheWrite5|Mezipaměť zápisu (horizontálních 5)|BytesPerSecond|Maximální počet||Žádné dimenze|
|cacheRead5|Mezipaměti pro čtení (horizontálních 5)|BytesPerSecond|Maximální počet||Žádné dimenze|
|percentProcessorTime5|Procesor (horizontálních 5)|Procento|Maximální počet||Žádné dimenze|
|connectedclients6|Připojených klientů (horizontálních 6)|Počet|Maximální počet||Žádné dimenze|
|totalcommandsprocessed6|Celkový počet operací (horizontálních 6)|Počet|Celkem||Žádné dimenze|
|cachehits6|Přístupů k mezipaměti (horizontálních 6)|Počet|Celkem||Žádné dimenze|
|cachemisses6|Neúspěšné přístupy do mezipaměti (horizontálních 6)|Počet|Celkem||Žádné dimenze|
|getcommands6|Získá (horizontálních 6)|Počet|Celkem||Žádné dimenze|
|setcommands6|Nastaví (horizontálních 6)|Počet|Celkem||Žádné dimenze|
|evictedkeys6|Vyřazené klíče (horizontálních 6)|Počet|Celkem||Žádné dimenze|
|totalkeys6|Celkový počet klíčů (horizontálních 6)|Počet|Maximální počet||Žádné dimenze|
|expiredkeys6|Vypršela platnost klíče (horizontálních 6)|Počet|Celkem||Žádné dimenze|
|usedmemory6|Použitá paměť (horizontálních 6)|Bajty|Maximální počet||Žádné dimenze|
|usedmemoryRss6|Použitá paměť RSS (horizontálních 6)|Bajty|Maximální počet||Žádné dimenze|
|serverLoad6|Zatížení serveru (horizontálních 6)|Procento|Maximální počet||Žádné dimenze|
|cacheWrite6|Mezipaměť zápisu (horizontálních 6)|BytesPerSecond|Maximální počet||Žádné dimenze|
|cacheRead6|Mezipaměti pro čtení (horizontálních 6)|BytesPerSecond|Maximální počet||Žádné dimenze|
|percentProcessorTime6|Procesor (horizontálních 6)|Procento|Maximální počet||Žádné dimenze|
|connectedclients7|Připojených klientů (horizontálních 7)|Počet|Maximální počet||Žádné dimenze|
|totalcommandsprocessed7|Celkový počet operací (horizontálních 7)|Počet|Celkem||Žádné dimenze|
|cachehits7|Přístupů k mezipaměti (horizontálních 7)|Počet|Celkem||Žádné dimenze|
|cachemisses7|Neúspěšné přístupy do mezipaměti (horizontálních 7)|Počet|Celkem||Žádné dimenze|
|getcommands7|Získá (horizontálních 7)|Počet|Celkem||Žádné dimenze|
|setcommands7|Nastaví (horizontálních 7)|Počet|Celkem||Žádné dimenze|
|evictedkeys7|Vyřazené klíče (horizontálních 7)|Počet|Celkem||Žádné dimenze|
|totalkeys7|Celkový počet klíčů (horizontálních 7)|Počet|Maximální počet||Žádné dimenze|
|expiredkeys7|Vypršela platnost klíče (horizontálních 7)|Počet|Celkem||Žádné dimenze|
|usedmemory7|Použitá paměť (horizontálních 7)|Bajty|Maximální počet||Žádné dimenze|
|usedmemoryRss7|Použitá paměť RSS (horizontálních 7)|Bajty|Maximální počet||Žádné dimenze|
|serverLoad7|Zatížení serveru (horizontálních 7)|Procento|Maximální počet||Žádné dimenze|
|cacheWrite7|Mezipaměť zápisu (horizontálních 7)|BytesPerSecond|Maximální počet||Žádné dimenze|
|cacheRead7|Mezipaměti pro čtení (horizontálních 7)|BytesPerSecond|Maximální počet||Žádné dimenze|
|percentProcessorTime7|Procesor (horizontálních 7)|Procento|Maximální počet||Žádné dimenze|
|connectedclients8|Připojených klientů (horizontálních 8)|Počet|Maximální počet||Žádné dimenze|
|totalcommandsprocessed8|Celkový počet operací (horizontálních 8)|Počet|Celkem||Žádné dimenze|
|cachehits8|Přístupů k mezipaměti (horizontálních 8)|Počet|Celkem||Žádné dimenze|
|cachemisses8|Neúspěšné přístupy do mezipaměti (horizontálních 8)|Počet|Celkem||Žádné dimenze|
|getcommands8|Získá (horizontálních 8)|Počet|Celkem||Žádné dimenze|
|setcommands8|Nastaví (horizontálních 8)|Počet|Celkem||Žádné dimenze|
|evictedkeys8|Vyřazené klíče (horizontálních 8)|Počet|Celkem||Žádné dimenze|
|totalkeys8|Celkový počet klíčů (horizontálních 8)|Počet|Maximální počet||Žádné dimenze|
|expiredkeys8|Vypršela platnost klíče (horizontálních 8)|Počet|Celkem||Žádné dimenze|
|usedmemory8|Použitá paměť (horizontálních 8)|Bajty|Maximální počet||Žádné dimenze|
|usedmemoryRss8|Použitá paměť RSS (horizontálních 8)|Bajty|Maximální počet||Žádné dimenze|
|serverLoad8|Zatížení serveru (horizontálních 8)|Procento|Maximální počet||Žádné dimenze|
|cacheWrite8|Mezipaměť zápisu (horizontálních 8)|BytesPerSecond|Maximální počet||Žádné dimenze|
|cacheRead8|Mezipaměti pro čtení (horizontálních 8)|BytesPerSecond|Maximální počet||Žádné dimenze|
|percentProcessorTime8|Procesor (horizontálních 8)|Procento|Maximální počet||Žádné dimenze|
|connectedclients9|Připojených klientů (horizontálních 9)|Počet|Maximální počet||Žádné dimenze|
|totalcommandsprocessed9|Celkový počet operací (horizontálních 9)|Počet|Celkem||Žádné dimenze|
|cachehits9|Přístupů k mezipaměti (horizontálních 9)|Počet|Celkem||Žádné dimenze|
|cachemisses9|Neúspěšné přístupy do mezipaměti (horizontálních 9)|Počet|Celkem||Žádné dimenze|
|getcommands9|Získá (horizontálních 9)|Počet|Celkem||Žádné dimenze|
|setcommands9|Nastaví (horizontálních 9)|Počet|Celkem||Žádné dimenze|
|evictedkeys9|Vyřazené klíče (horizontálních 9)|Počet|Celkem||Žádné dimenze|
|totalkeys9|Celkový počet klíčů (horizontálních 9)|Počet|Maximální počet||Žádné dimenze|
|expiredkeys9|Vypršela platnost klíče (horizontálních 9)|Počet|Celkem||Žádné dimenze|
|usedmemory9|Použitá paměť (horizontálních 9)|Bajty|Maximální počet||Žádné dimenze|
|usedmemoryRss9|Použitá paměť RSS (horizontálních 9)|Bajty|Maximální počet||Žádné dimenze|
|serverLoad9|Zatížení serveru (horizontálních 9)|Procento|Maximální počet||Žádné dimenze|
|cacheWrite9|Mezipaměť zápisu (horizontálních 9)|BytesPerSecond|Maximální počet||Žádné dimenze|
|cacheRead9|Mezipaměti pro čtení (horizontálních 9)|BytesPerSecond|Maximální počet||Žádné dimenze|
|percentProcessorTime9|Procesor (horizontálních 9)|Procento|Maximální počet||Žádné dimenze|

## <a name="microsoftclassiccomputevirtualmachines"></a>Microsoft.ClassicCompute/virtualMachines

|Metrika|Metriky zobrazovaný název|Jednotka|Typ agregace|Popis|Dimenze|
|---|---|---|---|---|---|
|Procento využití procesoru|Procento využití procesoru|Procento|Průměr|Procento přidělené výpočetní jednotky, které jsou aktuálně používán virtuálním počítačům.|Žádné dimenze|
|Sítě v|Sítě v|Bajty|Celkem|Počet bajtů přijatých virtuální počítače (příchozí provoz) na všech síťových rozhraní.|Žádné dimenze|
|Sítě Out|Sítě Out|Bajty|Celkem|Počet bajtů odhlašování na všech síťových rozhraní pomocí virtuální počítače (odchozí provoz).|Žádné dimenze|
|Čtení z disku bajtů/s|Čtení disku|BytesPerSecond|Průměr|Průměrný počet bajtů přečtených z disku během období sledování.|Žádné dimenze|
|Bajty zapisování na disk/s|Zápis disku|BytesPerSecond|Průměr|Průměrný počet bajtů zapsat na disk během období sledování.|Žádné dimenze|
|Čtení z disku operace/s|Čtení z disku operace/s|CountPerSecond|Průměr|Čtení disku IOPS.|Žádné dimenze|
|Operace zápisu disku/s|Operace zápisu disku/s|CountPerSecond|Průměr|Zápis disku IOPS.|Žádné dimenze|

## <a name="microsoftcognitiveservicesaccounts"></a>Microsoft.CognitiveServices/accounts

|Metrika|Metriky zobrazovaný název|Jednotka|Typ agregace|Popis|Dimenze|
|---|---|---|---|---|---|
|TotalCalls|Celkový počet volání|Počet|Celkem|Celkový počet volání.|Žádné dimenze|
|SuccessfulCalls|Úspěšné volání|Počet|Celkem|Počet úspěšných volání.|Žádné dimenze|
|TotalErrors|Celkový počet chyb|Počet|Celkem|Celkový počet volání s odpovědi na chybu (4xx kód odpovědi HTTP nebo 5xx).|Žádné dimenze|
|BlockedCalls|Blokované volání|Počet|Celkem|Počet volání dané překročil míry nebo limit kvóty.|Žádné dimenze|
|ServerErrors|Chyby serveru|Počet|Celkem|Počet volání s vnitřní chybou služby (5xx kód odpovědi HTTP).|Žádné dimenze|
|ClientErrors|Chyby klienta|Počet|Celkem|Počet volání s chyba na straně klienta (4xx kód odpovědi HTTP).|Žádné dimenze|
|DataIn|Data v|Bajty|Celkem|Velikost příchozích dat v bajtech.|Žádné dimenze|
|DataOut|Data|Bajty|Celkem|Velikost odesílaných dat v bajtech.|Žádné dimenze|
|Latence|Latence|Počet milisekund|Průměr|Latence v milisekundách.|Žádné dimenze|
|CharactersTranslated|Znaky přeložit|Počet|Celkem|Celkový počet znaků v příchozí žádosti o text.|Žádné dimenze|
|SpeechSessionDuration|Rozpoznávání řeči dobu trvání relace|Sekundy|Celkem|Celková doba trvání relace řeči v sekundách.|Žádné dimenze|

## <a name="microsoftcomputevirtualmachines"></a>Microsoft.Compute/virtualMachines

|Metrika|Metriky zobrazovaný název|Jednotka|Typ agregace|Popis|Dimenze|
|---|---|---|---|---|---|
|Procento využití procesoru|Procento využití procesoru|Procento|Průměr|Procento přidělené výpočetní jednotky, které jsou aktuálně používán virtuálním počítačům|Žádné dimenze|
|Sítě v|Sítě v|Bajty|Celkem|Počet bajtů přijatých virtuální počítače (příchozí provoz) na všech síťových rozhraních|Žádné dimenze|
|Sítě Out|Sítě Out|Bajty|Celkem|Počet bajtů odhlašování na všech síťových rozhraní pomocí virtuální počítače (odchozí provoz)|Žádné dimenze|
|Bajty čtení disku|Bajty čtení disku|Bajty|Celkem|Celkový počet bajtů přečtených z disku během období sledování|Žádné dimenze|
|Bajty zápisu disku|Bajty zápisu disku|Bajty|Celkem|Celkový počet bajtů zapsaných na disk během období sledování|Žádné dimenze|
|Čtení z disku operace/s|Čtení z disku operace/s|CountPerSecond|Průměr|Čtení disku IOPS|Žádné dimenze|
|Operace zápisu disku/s|Operace zápisu disku/s|CountPerSecond|Průměr|Zápis disku IOPS|Žádné dimenze|
|Zbývající kredit procesoru|Zbývající kredit procesoru|Počet|Průměr|Celkový počet kredity, které jsou k dispozici pro rozšíření|Žádné dimenze|
|Použít kredity procesoru|Použít kredity procesoru|Počet|Průměr|Celkový počet kredity, které zabírají virtuální počítač|Žádné dimenze|

## <a name="microsoftcomputevirtualmachinescalesets"></a>Microsoft.Compute/virtualMachineScaleSets

|Metrika|Metriky zobrazovaný název|Jednotka|Typ agregace|Popis|Dimenze|
|---|---|---|---|---|---|
|Procento využití procesoru|Procento využití procesoru|Procento|Průměr|Procento přidělené výpočetní jednotky, které jsou aktuálně používán virtuálním počítačům|Žádné dimenze|
|Sítě v|Sítě v|Bajty|Celkem|Počet bajtů přijatých virtuální počítače (příchozí provoz) na všech síťových rozhraních|Žádné dimenze|
|Sítě Out|Sítě Out|Bajty|Celkem|Počet bajtů odhlašování na všech síťových rozhraní pomocí virtuální počítače (odchozí provoz)|Žádné dimenze|
|Bajty čtení disku|Bajty čtení disku|Bajty|Celkem|Celkový počet bajtů přečtených z disku během období sledování|Žádné dimenze|
|Bajty zápisu disku|Bajty zápisu disku|Bajty|Celkem|Celkový počet bajtů zapsaných na disk během období sledování|Žádné dimenze|
|Čtení z disku operace/s|Čtení z disku operace/s|CountPerSecond|Průměr|Čtení disku IOPS|Žádné dimenze|
|Operace zápisu disku/s|Operace zápisu disku/s|CountPerSecond|Průměr|Zápis disku IOPS|Žádné dimenze|
|Zbývající kredit procesoru|Zbývající kredit procesoru|Počet|Průměr|Celkový počet kredity, které jsou k dispozici pro rozšíření|Žádné dimenze|
|Použít kredity procesoru|Použít kredity procesoru|Počet|Průměr|Celkový počet kredity, které zabírají virtuální počítač|Žádné dimenze|

## <a name="microsoftcomputevirtualmachinescalesetsvirtualmachines"></a>Microsoft.Compute/virtualMachineScaleSets/virtualMachines

|Metrika|Metriky zobrazovaný název|Jednotka|Typ agregace|Popis|Dimenze|
|---|---|---|---|---|---|
|Procento využití procesoru|Procento využití procesoru|Procento|Průměr|Procento přidělené výpočetní jednotky, které jsou aktuálně používán virtuálním počítačům|Žádné dimenze|
|Sítě v|Sítě v|Bajty|Celkem|Počet bajtů přijatých virtuální počítače (příchozí provoz) na všech síťových rozhraních|Žádné dimenze|
|Sítě Out|Sítě Out|Bajty|Celkem|Počet bajtů odhlašování na všech síťových rozhraní pomocí virtuální počítače (odchozí provoz)|Žádné dimenze|
|Bajty čtení disku|Bajty čtení disku|Bajty|Celkem|Celkový počet bajtů přečtených z disku během období sledování|Žádné dimenze|
|Bajty zápisu disku|Bajty zápisu disku|Bajty|Celkem|Celkový počet bajtů zapsaných na disk během období sledování|Žádné dimenze|
|Čtení z disku operace/s|Čtení z disku operace/s|CountPerSecond|Průměr|Čtení disku IOPS|Žádné dimenze|
|Operace zápisu disku/s|Operace zápisu disku/s|CountPerSecond|Průměr|Zápis disku IOPS|Žádné dimenze|
|Zbývající kredit procesoru|Zbývající kredit procesoru|Počet|Průměr|Celkový počet kredity, které jsou k dispozici pro rozšíření|Žádné dimenze|
|Použít kredity procesoru|Použít kredity procesoru|Počet|Průměr|Celkový počet kredity, které zabírají virtuální počítač|Žádné dimenze|

## <a name="microsoftcustomerinsightshubs"></a>Microsoft.CustomerInsights/hubs

|Metrika|Metriky zobrazovaný název|Jednotka|Typ agregace|Popis|Dimenze|
|---|---|---|---|---|---|
|DCIApiCalls|Přehled rozhraní API volání zákazníka|Počet|Celkem||Žádné dimenze|
|DCIMappingImportOperationSuccessfulLines|Mapování importu operace úspěšné řádky|Počet|Celkem||Žádné dimenze|
|DCIMappingImportOperationFailedLines|Mapování importu se nezdařila řádky|Počet|Celkem||Žádné dimenze|
|DCIMappingImportOperationTotalLines|Mapování importu operaci celkový počet řádků|Počet|Celkem||Žádné dimenze|
|DCIMappingImportOperationRuntimeInSeconds|Mapování importu operaci za běhu v sekundách|Sekundy|Celkem||Žádné dimenze|
|DCIOutboundProfileExportSucceeded|Odchozí profilu exportu byla úspěšná|Počet|Celkem||Žádné dimenze|
|DCIOutboundProfileExportFailed|Odchozí profilu exportu se nezdařila|Počet|Celkem||Žádné dimenze|
|DCIOutboundProfileExportDuration|Doba trvání odchozí profilu exportu|Sekundy|Celkem||Žádné dimenze|
|DCIOutboundKpiExportSucceeded|Odchozí klíčového ukazatele výkonu Export byl úspěšný|Počet|Celkem||Žádné dimenze|
|DCIOutboundKpiExportFailed|Odchozí klíčového ukazatele výkonu Export se nezdařil|Počet|Celkem||Žádné dimenze|
|DCIOutboundKpiExportDuration|Doba trvání odchozí Export klíčového ukazatele výkonu|Sekundy|Celkem||Žádné dimenze|
|DCIOutboundKpiExportStarted|Export odchozí klíčový ukazatel výkonu spustit|Sekundy|Celkem||Žádné dimenze|
|DCIOutboundKpiRecordCount|Počet záznamů odchozí klíčového ukazatele výkonu|Sekundy|Celkem||Žádné dimenze|
|DCIOutboundProfileExportCount|Počet Export odchozí profilů|Sekundy|Celkem||Žádné dimenze|
|DCIOutboundInitialProfileExportFailed|Odchozí počáteční profilu exportu se nezdařila|Sekundy|Celkem||Žádné dimenze|
|DCIOutboundInitialProfileExportSucceeded|Odchozí počáteční profilu exportu byla úspěšná|Sekundy|Celkem||Žádné dimenze|
|DCIOutboundInitialKpiExportFailed|Odchozí počáteční klíčového ukazatele výkonu Export se nezdařil|Sekundy|Celkem||Žádné dimenze|
|DCIOutboundInitialKpiExportSucceeded|Odchozí počáteční klíčového ukazatele výkonu Export byl úspěšný|Sekundy|Celkem||Žádné dimenze|
|DCIOutboundInitialProfileExportDurationInSeconds|Odchozí počáteční profilu exportu doby v sekundách|Sekundy|Celkem||Žádné dimenze|
|AdlaJobForStandardKpiFailed|Úloha Adla pro standardní klíčový ukazatel výkonu se nezdařilo s|Sekundy|Celkem||Žádné dimenze|
|AdlaJobForStandardKpiTimeOut|Úloha Adla pro standardní klíčový ukazatel výkonu časový limit v sekundách|Sekundy|Celkem||Žádné dimenze|
|AdlaJobForStandardKpiCompleted|Úloha Adla pro standardní klíčový ukazatel výkonu dokončit v sekundách|Sekundy|Celkem||Žádné dimenze|
|ImportASAValuesFailed|Hodnoty ASA importu se nezdařila, počet|Počet|Celkem||Žádné dimenze|
|ImportASAValuesSucceeded|Import ASA hodnoty počtu bylo úspěšně dokončeno.|Počet|Celkem||Žádné dimenze|
|DCIProfilesCount|Počet instancí profilu|Počet|poslední||Žádné dimenze|
|DCIInteractionsPerMonthCount|Interakce na počet měsíc|Počet|poslední||Žádné dimenze|
|DCIKpisCount|Počet klíčového ukazatele výkonu|Počet|poslední||Žádné dimenze|
|DCISegmentsCount|Počet segmentů|Počet|poslední||Žádné dimenze|
|DCIPredictiveMatchPoliciesCount|Prediktivní počet shod|Počet|poslední||Žádné dimenze|
|DCIPredictionsCount|Počet předpovědi|Počet|poslední||Žádné dimenze|

## <a name="microsoftdatalakeanalyticsaccounts"></a>Microsoft.DataLakeAnalytics/accounts

|Metrika|Metriky zobrazovaný název|Jednotka|Typ agregace|Popis|Dimenze|
|---|---|---|---|---|---|
|JobEndedSuccess|Úspěšné úlohy|Počet|Celkem|Počet neúspěšných úloh.|Žádné dimenze|
|JobEndedFailure|Neúspěšné úlohy|Počet|Celkem|Počet nezdařených úloh.|Žádné dimenze|
|JobEndedCancelled|Zrušené úlohy|Počet|Celkem|Počet zrušených úloh.|Žádné dimenze|
|JobAUEndedSuccess|Úspěšné AU čas|Sekundy|Celkem|Celkový čas AU pro úspěšné úlohy.|Žádné dimenze|
|JobAUEndedFailure|Neúspěšné AU čas|Sekundy|Celkem|Celkový čas AU pro neúspěšné úlohy.|Žádné dimenze|
|JobAUEndedCancelled|Zrušené AU čas|Sekundy|Celkem|Celkový čas AU pro zrušené úlohy.|Žádné dimenze|

## <a name="microsoftdatalakestoreaccounts"></a>Microsoft.DataLakeStore/accounts

|Metrika|Metriky zobrazovaný název|Jednotka|Typ agregace|Popis|Dimenze|
|---|---|---|---|---|---|
|TotalStorage|Celkový počet úložiště|Bajty|Maximální počet|Celkové množství dat uložených v účtu.|Žádné dimenze|
|DataWritten|Data zapsaná|Bajty|Celkem|Celkové množství dat, zapisuje na účet.|Žádné dimenze|
|DataRead|Čtení dat|Bajty|Celkem|Celkové množství dat číst z účtu.|Žádné dimenze|
|WriteRequests|Požadavků na zápis|Počet|Celkem|Počet dat požadavků na zápis k účtu.|Žádné dimenze|
|ReadRequests|Požadavky pro čtení|Počet|Celkem|Počet data načíst požadavky na účet.|Žádné dimenze|

## <a name="microsoftdbformysqlservers"></a>Microsoft.DBforMySQL/servers

|Metrika|Metriky zobrazovaný název|Jednotka|Typ agregace|Popis|Dimenze|
|---|---|---|---|---|---|
|cpu_percent|Procento využití procesoru|Procento|Průměr|Procento využití procesoru|Žádné dimenze|
|compute_limit|Výpočetní jednotka limit|Počet|Průměr|Výpočetní jednotka limit|Žádné dimenze|
|compute_consumption_percent|Výpočetní jednotka procento|Procento|Průměr|Výpočetní jednotka procento|Žádné dimenze|
|memory_percent|Paměť v procentech|Procento|Průměr|Paměť v procentech|Žádné dimenze|
|io_consumption_percent|Vstupně-výstupní operace v procentech|Procento|Průměr|Vstupně-výstupní operace v procentech|Žádné dimenze|
|storage_percent|Procento úložiště|Procento|Průměr|Procento úložiště|Žádné dimenze|
|storage_used|Využité úložiště|Bajty|Průměr|Využité úložiště|Žádné dimenze|
|storage_limit|Limit úložiště|Bajty|Průměr|Limit úložiště|Žádné dimenze|
|active_connections|Celkový počet aktivních připojení|Počet|Průměr|Celkový počet aktivních připojení|Žádné dimenze|
|connections_failed|Celkový počet selhání připojení|Počet|Průměr|Celkový počet selhání připojení|Žádné dimenze|

## <a name="microsoftdbforpostgresqlservers"></a>Microsoft.DBforPostgreSQL/servers

|Metrika|Metriky zobrazovaný název|Jednotka|Typ agregace|Popis|Dimenze|
|---|---|---|---|---|---|
|cpu_percent|Procento využití procesoru|Procento|Průměr|Procento využití procesoru|Žádné dimenze|
|compute_limit|Výpočetní jednotka limit|Počet|Průměr|Výpočetní jednotka limit|Žádné dimenze|
|compute_consumption_percent|Výpočetní jednotka procento|Procento|Průměr|Výpočetní jednotka procento|Žádné dimenze|
|memory_percent|Paměť v procentech|Procento|Průměr|Paměť v procentech|Žádné dimenze|
|io_consumption_percent|Vstupně-výstupní operace v procentech|Procento|Průměr|Vstupně-výstupní operace v procentech|Žádné dimenze|
|storage_percent|Procento úložiště|Procento|Průměr|Procento úložiště|Žádné dimenze|
|storage_used|Využité úložiště|Bajty|Průměr|Využité úložiště|Žádné dimenze|
|storage_limit|Limit úložiště|Bajty|Průměr|Limit úložiště|Žádné dimenze|
|active_connections|Celkový počet aktivních připojení|Počet|Průměr|Celkový počet aktivních připojení|Žádné dimenze|
|connections_failed|Celkový počet selhání připojení|Počet|Průměr|Celkový počet selhání připojení|Žádné dimenze|

## <a name="microsoftdevicesiothubs"></a>Microsoft.Devices/IotHubs

|Metrika|Metriky zobrazovaný název|Jednotka|Typ agregace|Popis|Dimenze|
|---|---|---|---|---|---|
|d2c.telemetry.ingress.allProtocol|Pokusy o odeslání zprávy telemetrie|Počet|Celkem|Počet zařízení cloud telemetrické zprávy se pokusil odeslat do služby IoT hub|Žádné dimenze|
|d2c.telemetry.ingress.Success|Telemetrické zprávy odeslané|Počet|Celkem|Počet zařízení cloud telemetrické zprávy úspěšně odeslán do služby IoT hub|Žádné dimenze|
|c2d.Commands.egress.COMPLETE.Success|Příkazy byla dokončena|Počet|Celkem|Počet příkazů typu cloud zařízení úspěšně dokončila, a zařízení|Žádné dimenze|
|c2d.Commands.egress.Abandon.Success|Příkazy opuštění|Počet|Celkem|Počet příkazů typu cloud zařízení opuštěny v rámci zařízení|Žádné dimenze|
|c2d.Commands.egress.Reject.Success|Příkazy odmítnut|Počet|Celkem|Počet zařízení byl odmítnut příkazů typu cloud zařízení|Žádné dimenze|
|devices.totalDevices|Celkový počet zařízení|Počet|Celkem|Počet zařízení zaregistrovat do služby IoT hub|Žádné dimenze|
|devices.connectedDevices.allProtocol|Připojená zařízení|Počet|Celkem|Počet zařízení připojených ke službě IoT hub|Žádné dimenze|
|d2c.telemetry.egress.Success|Telemetrické zprávy doručit|Počet|Celkem|Počet zpráv byla úspěšně zapsána do koncových bodů (celkem)|Žádné dimenze|
|d2c.telemetry.egress.dropped|Vyřazené zprávy|Počet|Celkem|Počet zpráv vyřadit, protože koncový bod doručení byla neaktivní|Žádné dimenze|
|d2c.telemetry.egress.orphaned|Osamocené zprávy|Počet|Celkem|Počet zpráv, na které se neshodují žádné cesty, včetně záložní trasy|Žádné dimenze|
|d2c.telemetry.egress.invalid|Neplatné zprávy|Počet|Celkem|Počet zpráv, které nejsou doručeny z důvodu nekompatibility s nástrojem koncový bod|Žádné dimenze|
|d2c.telemetry.egress.Fallback|Zprávy odpovídající záložního stavu|Počet|Celkem|Počet zpráv zapsaných do záložního koncového bodu|Žádné dimenze|
|d2c.endpoints.egress.eventHubs|Zpráv doručených do koncové body centra událostí|Počet|Celkem|Počet zpráv byla úspěšně zapsána do koncové body centra událostí|Žádné dimenze|
|d2c.endpoints.latency.eventHubs|Latence zprávy pro koncové body centra událostí|počet milisekund|Průměr|Průměrná latence mezi příchozí zprávy do služby IoT hub a příchozí zprávy do koncového bodu centra událostí, v milisekundách|Žádné dimenze|
|d2c.endpoints.egress.serviceBusQueues|Doručování zpráv do fronty Service Bus koncové body|Počet|Celkem|Počet zpráv byla úspěšně zapsána do koncových bodů frontou Service Bus|Žádné dimenze|
|d2c.endpoints.latency.serviceBusQueues|Latence zprávy pro koncové body frontou Service Bus|počet milisekund|Průměr|Průměrná latence mezi příchozí zprávy do služby IoT hub a příjem příchozích dat zpráv do fronty Service Bus koncový bod, v milisekundách|Žádné dimenze|
|d2c.endpoints.egress.serviceBusTopics|Doručování zpráv do tématu Service Bus koncové body|Počet|Celkem|Počet zpráv byly úspěšně zapsána do tématu Service Bus koncové body|Žádné dimenze|
|d2c.endpoints.latency.serviceBusTopics|Latence zprávy pro koncové body témata sběrnice|počet milisekund|Průměr|Průměrná latence mezi příchozí zprávy do služby IoT hub a příchozí zprávy do tématu Service Bus koncový bod, v milisekundách|Žádné dimenze|
|d2c.endpoints.egress.builtIn.events|Zpráv doručených do vestavěným koncovým bodem (zprávy nebo události)|Počet|Celkem|Počet zpráv byla úspěšně zapsána do vestavěným koncovým bodem (zprávy nebo události)|Žádné dimenze|
|d2c.endpoints.latency.builtIn.events|Zpráva latence pro předdefinovaný koncový bod (zprávy nebo události)|počet milisekund|Průměr|Průměrná latence mezi příchozí zprávy do služby IoT hub a příjem příchozích dat zpráva do vestavěným koncovým bodem (zprávy událostí), v milisekundách |Žádné dimenze|
|d2c.endpoints.egress.Storage|Zpráv doručených do koncové body úložiště|Počet|Celkem|Počet zpráv byla úspěšně zapsána do koncové body úložiště|Žádné dimenze|
|d2c.endpoints.latency.Storage|Latence zprávy pro koncové body úložiště|počet milisekund|Průměr|Průměrná latence mezi příchozí zprávy do služby IoT hub a příjem příchozích dat zpráva do koncový bod úložiště, v milisekundách|Žádné dimenze|
|d2c.endpoints.egress.Storage.bytes|Data zapsaná do úložiště|Bajty|Celkem|Množství dat v bajtech, zapisovat do úložiště koncové body|Žádné dimenze|
|d2c.endpoints.egress.Storage.BLOBS|Zapisovat do úložiště objektů BLOB|Počet|Celkem|Počet zapsána do koncové body úložiště objektů BLOB|Žádné dimenze|
|d2c.Twin.Read.Success|Úspěšné twin čte ze zařízení|Počet|Celkem|Počet čtení všechny úspěšné twin spouštěná zařízení.|Žádné dimenze|
|d2c.Twin.Read.failure|Čtení twin ze zařízení se nezdařila|Počet|Celkem|Počet všech nepodařilo spouštěná zařízení twin čtení.|Žádné dimenze|
|d2c.Twin.Read.Size|Velikost odpovědi twin čtení ze zařízení|Bajty|Průměr|Průměr, min a max všechny úspěšné, spouštěná zařízení twin čtení.|Žádné dimenze|
|d2c.Twin.Update.Success|Aktualizace úspěšná twin ze zařízení|Počet|Celkem|Počet aktualizací všechny úspěšné twin spouštěná zařízení.|Žádné dimenze|
|d2c.Twin.Update.failure|Aktualizace twin ze zařízení se nezdařila|Počet|Celkem|Počet všech se nezdařila aktualizace twin spouštěná zařízení.|Žádné dimenze|
|d2c.Twin.Update.Size|Velikost twin aktualizace ze zařízení|Bajty|Průměr|Průměr, minimální a maximální velikost všech úspěšné, spouštěná zařízení twin aktualizace.|Žádné dimenze|
|c2d.Methods.Success|Volání úspěšné přímá metoda|Počet|Celkem|Počet všech úspěšných volání přímá metoda.|Žádné dimenze|
|c2d.Methods.failure|Přímá metoda volání se nezdařilo|Počet|Celkem|Počet všech se nezdařila, volání metod direct.|Žádné dimenze|
|c2d.Methods.requestSize|Žádost o velikosti volání přímá metoda|Bajty|Průměr|Průměr, min a max všechny úspěšné požadavky přímá metoda.|Žádné dimenze|
|c2d.Methods.responseSize|Velikost odpovědi volání přímá metoda|Bajty|Průměr|Průměr, min a max všechny úspěšné odpovědi přímá metoda.|Žádné dimenze|
|c2d.Twin.Read.Success|Úspěšné twin čte z back-end|Počet|Celkem|Počet všech úspěšně spustil back-end twin čtení.|Žádné dimenze|
|c2d.Twin.Read.failure|Neúspěšné twin čte z back-end|Počet|Celkem|Počet všech se nezdařilo čtení twin iniciované back-end.|Žádné dimenze|
|c2d.Twin.Read.Size|Velikost odpovědi twin čtení z back-end|Bajty|Průměr|Průměr, min a max všechny úspěšné, iniciované back-end twin čtení.|Žádné dimenze|
|c2d.Twin.Update.Success|Aktualizace úspěšná twin z back-end|Počet|Celkem|Počet aktualizací všechny úspěšné twin iniciované back-end.|Žádné dimenze|
|c2d.Twin.Update.failure|Aktualizace se nezdařila twin z back-end|Počet|Celkem|Počet všech se nezdařila aktualizace twin iniciované back-end.|Žádné dimenze|
|c2d.Twin.Update.Size|Velikost twin aktualizace z back-end|Bajty|Průměr|Průměr, minimální a maximální velikost všech úspěšné, iniciované back-end twin aktualizace.|Žádné dimenze|
|twinQueries.success|Úspěšné twin dotazy|Počet|Celkem|Počet všech úspěšné twin dotazů.|Žádné dimenze|
|twinQueries.failure|Neúspěšné twin dotazy|Počet|Celkem|Počet všechny neúspěšné twin dotazy.|Žádné dimenze|
|twinQueries.resultSize|Objem výsledků dotazů Twin|Bajty|Průměr|Průměr, minimální a maximální velikosti výsledek všechny úspěšné twin dotazy.|Žádné dimenze|
|jobs.createTwinUpdateJob.success|Úspěšné vytvoření úloh aktualizace twin|Počet|Celkem|Počet všech úspěšném vytvoření twin aktualizace úloh.|Žádné dimenze|
|jobs.createTwinUpdateJob.failure|Neúspěšné vytvoření úloh aktualizace twin|Počet|Celkem|Počet všech se nezdařilo vytvoření twin aktualizace úlohy.|Žádné dimenze|
|jobs.createDirectMethodJob.success|Úspěšné vytvoření úloh volání – metoda|Počet|Celkem|Počet všech úspěšném vytvoření přímá metoda volání úloh.|Žádné dimenze|
|jobs.createDirectMethodJob.failure|Neúspěšné vytvoření úloh volání – metoda|Počet|Celkem|Počet všech se nezdařilo vytvoření přímá metoda vyvolání úlohy.|Žádné dimenze|
|jobs.listJobs.success|Úspěšné volání do seznamu úloh|Počet|Celkem|Počet všech úspěšných volání do seznamu úloh.|Žádné dimenze|
|jobs.listJobs.failure|Volání do seznamu úloh se nezdařilo|Počet|Celkem|Počet všech selhání volání do seznamu úloh.|Žádné dimenze|
|jobs.cancelJob.success|Zrušení úloh úspěšné|Počet|Celkem|Počet všech úspěšných volání zrušení úlohy.|Žádné dimenze|
|jobs.cancelJob.failure|Informace o neúspěšné úloze zrušení|Počet|Celkem|Počet všech volání se nezdařilo zrušit úlohu.|Žádné dimenze|
|jobs.queryJobs.success|Až se úloha úspěšně dotazy|Počet|Celkem|Počet všech úspěšných volání do úlohy dotaz.|Žádné dimenze|
|jobs.queryJobs.failure|Úloha se dotazuje se nezdařilo|Počet|Celkem|Počet všech selhání volání do úlohy dotaz.|Žádné dimenze|
|jobs.completed|Dokončené úlohy|Počet|Celkem|Počet všech dokončené úlohy.|Žádné dimenze|
|jobs.Failed|Neúspěšné úlohy|Počet|Celkem|Počet všechny neúspěšné úlohy.|Žádné dimenze|
|d2c.telemetry.ingress.sendThrottle|Počet omezení chyb|Počet|Celkem|Omezí počet omezení chyb z důvodu propustnost zařízení generovaný|Žádné dimenze|
|dailyMessageQuotaUsed|Celkový počet zpráv, které používá|Počet|Průměr|Počet celkový počet zpráv v současné době používá. Toto je kumulativní hodnotu, která je nastaven na hodnotu nula v 00:00 UTC každý den.|Žádné dimenze|

## <a name="microsoftdevicesprovisioningservices"></a>Microsoft.Devices/provisioningServices

|Metrika|Metriky zobrazovaný název|Jednotka|Typ agregace|Popis|Dimenze|
|---|---|---|---|---|---|
|RegistrationAttempts|Pokusy o registraci|Počet|Celkem|Počet pokus o registraci zařízení|Stav ProvisioningServiceName, IotHubName,|
|DeviceAssignments|Zařízení přiřazená|Počet|Celkem|Počet zařízení přiřazená do služby IoT hub|ProvisioningServiceName IotHubName|
|AttestationAttempts|Pokusy o ověření identity|Počet|Celkem|Počet zařízení atestace podle pokus|Protokol ProvisioningServiceName, stav,|

## <a name="microsoftdocumentdbdatabaseaccounts-cosmosdb"></a>Microsoft.DocumentDB/databaseAccounts (CosmosDB)
|Metrika|Metriky zobrazovaný název|Jednotka|Typ agregace|Popis|Dimenze|
|---|---|---|---|---|---|
|TotalRequests|Celkový počet požadavků|Počet|Počet|Počet zpracovaných požadavků|DatabaseAccount, název_kolekce, DatabaseName, oblast, StatusCode|
|MongoRequests|Mongo požadavky|Počet|Počet|Počet zpracovaných požadavků Mongo|DatabaseAccount, název_kolekce, DatabaseName, oblast, kód chyby, CommandName|

## <a name="microsofteventhubnamespaces"></a>Microsoft.EventHub/namespaces

|Metrika|Metriky zobrazovaný název|Jednotka|Typ agregace|Popis|Dimenze|
|---|---|---|---|---|---|
|INREQS|Příchozích požadavků na odeslání|Počet|Celkem|Celkový počet příchozích odesílat požadavky pro obor názvů|Žádné dimenze|
|SUCCREQ|Úspěšné požadavky|Počet|Celkem|Celkový počet úspěšných požadavků pro obor názvů|Žádné dimenze|
|FAILREQ|Neúspěšné požadavky|Počet|Celkem|Celkový počet neúspěšných požadavků pro obor názvů|Žádné dimenze|
|SVRBSY|Chyby zaneprázdněný server|Počet|Celkem|Celkový počet server zaneprázdněn chyby pro obor názvů|Žádné dimenze|
|INTERR|Vnitřní chyby serveru|Počet|Celkem|Celkový počet vnitřní chyby serveru pro obor názvů|Žádné dimenze|
|MISCERR|Další chyby|Počet|Celkem|Celkový počet neúspěšných požadavků pro obor názvů|Žádné dimenze|
|INMSGS|Příchozí zprávy (zastaralé)|Počet|Celkem|Celkový počet příchozích zpráv pro obor názvů. Tato metrika je zastaralý. Místo toho použijte metrika příchozí zprávy|Žádné dimenze|
|EHINMSGS|Příchozí zprávy|Počet|Celkem|Celkový počet příchozích zpráv pro obor názvů|Žádné dimenze|
|OUTMSGS|Odchozí zprávy (zastaralé)|Počet|Celkem|Celkový počet odchozích zpráv pro obor názvů. Tato metrika je zastaralý. Místo toho použijte metrika odchozích zpráv|Žádné dimenze|
|EHOUTMSGS|Odchozí zprávy|Počet|Celkem|Celkový počet odchozích zpráv pro obor názvů|Žádné dimenze|
|EHINMBS|Příchozí bajty (zastaralé)|Bajty|Celkem|Události rozbočovače příchozí zprávy propustnost pro obor názvů. Tato metrika je zastaralý. Místo toho použijte metrika Příchozí bajty|Žádné dimenze|
|EHINBYTES|Příchozí bajty|Bajty|Celkem|Události rozbočovače příchozí zprávy propustnost pro obor názvů|Žádné dimenze|
|EHOUTMBS|Odchozí bajty (zastaralé)|Bajty|Celkem|Události rozbočovače odchozí zprávy propustnost pro obor názvů. Tato metrika je zastaralý. Místo toho použijte metrika Odchozí bajty|Žádné dimenze|
|EHOUTBYTES|Odchozí bajty|Bajty|Celkem|Události rozbočovače odchozí zprávy propustnost pro obor názvů|Žádné dimenze|
|EHABL|Nevyřízené položky zpráv archivu|Počet|Celkem|Zprávy archivu centra událostí v nevyřízených položek pro obor názvů|Žádné dimenze|
|EHAMSGS|Archivace zpráv|Počet|Celkem|Centra událostí archivované zprávy v oboru názvů|Žádné dimenze|
|EHAMBS|Propustnost zpráv archivu|Bajty|Celkem|Centra událostí archivovat propustnost zpráv v oboru názvů|Žádné dimenze|

## <a name="microsoftinsightsautoscalesettings"></a>Microsoft.Insights/AutoscaleSettings

|Metrika|Metriky zobrazovaný název|Jednotka|Typ agregace|Popis|Dimenze|
|---|---|---|---|---|---|
|ObservedMetricValue|Zjištěnými hodnota metriky|Počet|Průměr|Hodnota vypočítaného podle škálování při spuštění|MetricTriggerSource|
|MetricThreshold|Metriky prahová hodnota|Počet|Průměr|Škálování nakonfigurovanou prahovou hodnotu při škálování došlo.|MetricTriggerRule|
|ObservedCapacity|Zjištěnou kapacity|Počet|Průměr|Kapacitu hlášené chcete používat automatické škálování, když se provedla.|Žádné dimenze|
|ScaleActionsInitiated|Akce škálování iniciované|Počet|Celkem|Směr operace škálování.|ScaleDirection|

## <a name="microsoftlogicworkflows"></a>Microsoft.Logic/workflows

|Metrika|Metriky zobrazovaný název|Jednotka|Typ agregace|Popis|Dimenze|
|---|---|---|---|---|---|
|RunsStarted|Spustí spuštění|Počet|Celkem|Počet pracovní postup spouští spuštěna.|Žádné dimenze|
|RunsCompleted|Spustí byla dokončena|Počet|Celkem|Počet pracovní postup spouští dokončené.|Žádné dimenze|
|RunsSucceeded|Spustí bylo úspěšné|Počet|Celkem|Počet pracovní postup spouští úspěšně.|Žádné dimenze|
|RunsFailed|Spustí se nezdařilo|Počet|Celkem|Počet pracovní postup spouští se nezdařilo.|Žádné dimenze|
|RunsCancelled|Spustí zrušena|Počet|Celkem|Počet pracovní postup spouští zrušené.|Žádné dimenze|
|RunLatency|Spustit latence|Sekundy|Průměr|Spustí latence dokončené pracovního postupu.|Žádné dimenze|
|RunSuccessLatency|Spustit latence úspěch|Sekundy|Průměr|Spustí latence úspěšně pracovního postupu.|Žádné dimenze|
|RunThrottledEvents|Spustit omezenému události|Počet|Celkem|Počet akci pracovního postupu nebo aktivační událost omezení událostí.|Žádné dimenze|
|RunFailurePercentage|Spustit procento selhání|Procento|Celkem|Procento pracovního postupu, spustí se nezdařilo.|Žádné dimenze|
|ActionsStarted|Akce spustila |Počet|Celkem|Počet spuštění akce pracovního postupu.|Žádné dimenze|
|ActionsCompleted|Akce |Počet|Celkem|Počet akce pracovního postupu byla dokončena.|Žádné dimenze|
|ActionsSucceeded|Akce byla úspěšná |Počet|Celkem|Počet akce pracovního postupu byla úspěšná.|Žádné dimenze|
|ActionsFailed|Akce se nezdařilo|Počet|Celkem|Počet akce pracovního postupu se nezdařilo.|Žádné dimenze|
|ActionsSkipped|Akce přeskočena |Počet|Celkem|Počet akce pracovního postupu přeskočeno.|Žádné dimenze|
|ActionLatency|Latence akce |Sekundy|Průměr|Latence akce dokončené pracovního postupu.|Žádné dimenze|
|ActionSuccessLatency|Latence úspěch akce |Sekundy|Průměr|Latence akce úspěšně pracovního postupu.|Žádné dimenze|
|ActionThrottledEvents|Akce omezení událostí|Počet|Celkem|Počet pracovního postupu akce omezeny události...|Žádné dimenze|
|TriggersStarted|Spuštění aktivační události |Počet|Celkem|Počet aktivuje pracovní postup spuštění.|Žádné dimenze|
|TriggersCompleted|Aktivace dokončena |Počet|Celkem|Počet aktivační události pracovního postupu byla dokončena.|Žádné dimenze|
|TriggersSucceeded|Aktivační události bylo úspěšně dokončeno |Počet|Celkem|Počet aktivační události pracovního postupu byla úspěšná.|Žádné dimenze|
|TriggersFailed|Aktivace se nezdařila |Počet|Celkem|Počet nezdařených z aktivačních událostí pracovního postupu.|Žádné dimenze|
|TriggersSkipped|Aktivační události přeskočena|Počet|Celkem|Počet aktivuje pracovní postup přeskočeno.|Žádné dimenze|
|TriggersFired|Aktivační události aktivováno |Počet|Celkem|Počet aktivuje pracovní postup aktivována.|Žádné dimenze|
|TriggerLatency|Latence aktivační události |Sekundy|Průměr|Latence služby aktivačních událostí dokončené pracovního postupu.|Žádné dimenze|
|TriggerFireLatency|Aktivační událost ještě efektivněji latence |Sekundy|Průměr|Latence aktivována aktivuje pracovní postup.|Žádné dimenze|
|TriggerSuccessLatency|Aktivační událost úspěch latence |Sekundy|Průměr|Latence služby aktivačních událostí úspěšně pracovního postupu.|Žádné dimenze|
|TriggerThrottledEvents|Aktivaci omezenému událostí|Počet|Celkem|Počet aktivační událost pracovního postupu omezení událostí.|Žádné dimenze|
|BillableActionExecutions|Fakturovatelný akce spuštění|Počet|Celkem|Počet spuštěních akce pracovního postupu získávání účtují.|Žádné dimenze|
|BillableTriggerExecutions|Fakturovatelný aktivační událost spuštění|Počet|Celkem|Počet spuštěních aktivační událost pracovního postupu získávání účtují.|Žádné dimenze|
|TotalBillableExecutions|Celkový počet spuštěních fakturovatelného času|Počet|Celkem|Počet spuštěních pracovního postupu získávání účtují.|Žádné dimenze|

## <a name="microsoftnetworkloadbalancers"></a>Microsoft.Network/loadBalancers

|Metrika|Metriky zobrazovaný název|Jednotka|Typ agregace|Popis|Dimenze|
|---|---|---|---|---|---|
|VipAvailability|Dostupnost virtuálních IP adres|Počet|Průměr|Dostupnost virtuální IP adresy koncových bodů, na základě výsledků testu|Virtuální IP adresa, VipPort|
|DipAvailability|Dostupnost vyhrazené IP adresy|Počet|Průměr|Dostupnost vyhrazené IP adresy koncových bodů, na základě výsledků testu|Typprotokolu, DipPort, virtuální IP adresa, VipPort, DipAddress|
|ByteCount|Počet bajtů|Počet|Celkem|Celkový počet bajtů přenesených v časovém období|Virtuální IP adresa, VipPort, směr|
|PacketCount|Počet paketů|Počet|Celkem|Celkový počet paketů odeslaných v časovém období|Virtuální IP adresa, VipPort, směr|
|SYNCount|Počet SYN|Počet|Celkem|Celkový počet paketů SYN přenášených v časovém období|Virtuální IP adresa, VipPort, směr|
|SnatConnectionCount|Počet připojení překládat pomocí SNAT|Počet|Celkem|Celkový počet nových připojení překládat pomocí SNAT vytvořených v časovém období|Virtuální IP adresa, DipAddress, ConnectionState|

## <a name="microsoftnetworkpublicipaddresses"></a>Microsoft.Network/publicIPAddresses

|Metrika|Metriky zobrazovaný název|Jednotka|Typ agregace|Popis|Dimenze|
|---|---|---|---|---|---|
|PacketsInDDoS|Příchozí pakety DDoS|CountPerSecond|Průměr|Příchozí pakety DDoS|Žádné dimenze|
|PacketsDroppedDDoS|Příchozí pakety vyřadit DDoS|CountPerSecond|Průměr|Příchozí pakety vyřadit DDoS|Žádné dimenze|
|PacketsForwardedDDoS|Příchozí pakety předávané DDoS|CountPerSecond|Průměr|Příchozí pakety předávané DDoS|Žádné dimenze|
|TCPPacketsInDDoS|Příchozí pakety TCP DDoS|CountPerSecond|Průměr|Příchozí pakety TCP DDoS|Žádné dimenze|
|TCPPacketsDroppedDDoS|Příchozí pakety TCP vyřadit DDoS|CountPerSecond|Průměr|Příchozí pakety TCP vyřadit DDoS|Žádné dimenze|
|TCPPacketsForwardedDDoS|Příchozí pakety TCP předávají DDoS|CountPerSecond|Průměr|Příchozí pakety TCP předávají DDoS|Žádné dimenze|
|UDPPacketsInDDoS|Příchozí pakety UDP DDoS|CountPerSecond|Průměr|Příchozí pakety UDP DDoS|Žádné dimenze|
|UDPPacketsDroppedDDoS|Příchozí pakety UDP vyřadit DDoS|CountPerSecond|Průměr|Příchozí pakety UDP vyřadit DDoS|Žádné dimenze|
|UDPPacketsForwardedDDoS|Příchozí pakety UDP předávají DDoS|CountPerSecond|Průměr|Příchozí pakety UDP předávají DDoS|Žádné dimenze|
|BytesInDDoS|Příchozí bajty DDoS|BytesPerSecond|Průměr|Příchozí bajty DDoS|Žádné dimenze|
|BytesDroppedDDoS|Příchozí bajty vyřadit DDoS|BytesPerSecond|Průměr|Příchozí bajty vyřadit DDoS|Žádné dimenze|
|BytesForwardedDDoS|Příchozí bajty předávaných DDoS|BytesPerSecond|Průměr|Příchozí bajty předávaných DDoS|Žádné dimenze|
|TCPBytesInDDoS|Příchozí bajty TCP DDoS|BytesPerSecond|Průměr|Příchozí bajty TCP DDoS|Žádné dimenze|
|TCPBytesDroppedDDoS|Příchozí bajty TCP vyřadit DDoS|BytesPerSecond|Průměr|Příchozí bajty TCP vyřadit DDoS|Žádné dimenze|
|TCPBytesForwardedDDoS|Příchozí bajty TCP předávaných DDoS|BytesPerSecond|Průměr|Příchozí bajty TCP předávaných DDoS|Žádné dimenze|
|UDPBytesInDDoS|Příchozí bajty UDP DDoS|BytesPerSecond|Průměr|Příchozí bajty UDP DDoS|Žádné dimenze|
|UDPBytesDroppedDDoS|Příchozí bajty UDP vyřadit DDoS|BytesPerSecond|Průměr|Příchozí bajty UDP vyřadit DDoS|Žádné dimenze|
|UDPBytesForwardedDDoS|Příchozí bajty UDP předávaných DDoS|BytesPerSecond|Průměr|Příchozí bajty UDP předávaných DDoS|Žádné dimenze|
|IfUnderDDoSAttack|V části DDoS útokům nebo ne|Počet|Průměr|V části DDoS útokům nebo ne|Žádné dimenze|
|DDoSTriggerTCPPackets|Příchozí pakety TCP k aktivaci DDoS zmírnění|CountPerSecond|Průměr|Příchozí pakety TCP k aktivaci DDoS zmírnění|Žádné dimenze|
|DDoSTriggerUDPPackets|Příchozí pakety UDP pro aktivaci DDoS zmírnění|CountPerSecond|Průměr|Příchozí pakety UDP pro aktivaci DDoS zmírnění|Žádné dimenze|
|VipAvailability|Dostupnost|Počet|Průměr|Průměrná dostupnost IPAddress v časovém období|Port|
|ByteCount|Počet bajtů|Počet|Celkem|Celkový počet bajtů přenesených v časovém období|Port, směr|
|PacketCount|Počet paketů|Počet|Celkem|Celkový počet paketů odeslaných v časovém období|Port, směr|
|SynCount|Počet SYN|Počet|Celkem|Celkový počet paketů SYN přenášených v časovém období|Port, směr|

## <a name="microsoftnetworkapplicationgateways"></a>Microsoft.Network/applicationGateways

|Metrika|Metriky zobrazovaný název|Jednotka|Typ agregace|Popis|Dimenze|
|---|---|---|---|---|---|
|Propustnost|Propustnost|BytesPerSecond|Průměr|Počet bajtů za sekundu, které se má zpracovat aplikační bránu|Žádné dimenze|

## <a name="microsoftnetworkvirtualnetworkgateways"></a>Microsoft.Network/virtualNetworkGateways

|Metrika|Metriky zobrazovaný název|Jednotka|Typ agregace|Popis|Dimenze|
|---|---|---|---|---|---|
|TunnelAverageBandwidth|Tunelové propojení šířky pásma|BytesPerSecond|Průměr|Průměrná šířky pásma tunelu v bajtech za sekundu|ConnectionName, Vzdálená adresa IP|
|TunnelPeakBandwidth|Tunelové propojení vrchol šířky pásma|BytesPerSecond|Průměr|Šířky pásma ve špičce tunelu v bajtech za sekundu|ConnectionName, Vzdálená adresa IP|
|TunnelEgressBytes|Tunelové propojení Odchozí bajty|Bajty|Průměr|Odchozí bajty v intervalu 5 minut tunelového propojení|ConnectionName, Vzdálená adresa IP|
|TunnelIngressBytes|Příchozí bajty tunelového propojení|Bajty|Průměr|Příchozí bajty v intervalu 5 minut tunelového propojení|ConnectionName, Vzdálená adresa IP|
|TunnelEgressPackets|Odchozí pakety tunelového propojení|Počet|Průměr|Počet odchozích paketů tunelu v intervalu 5 minut|ConnectionName, Vzdálená adresa IP|
|TunnelIngressPackets|Příchozí pakety tunelového propojení|Počet|Průměr|Počet příchozích paketů tunelu v intervalu 5 minut|ConnectionName, Vzdálená adresa IP|
|TunnelEgressPacketDropTSMismatch|Tunelové propojení odchozí TS neshoda paketu rozevírací|Počet|Průměr|Odchozích paketů vyřaďte z provozu selektor neshoda tunelu v intervalu 5 minut|ConnectionName, Vzdálená adresa IP|
|TunnelIngressPacketDropTSMismatch|Tunelové propojení příchozího TS neshoda paketu rozevírací|Počet|Průměr|Příchozí paket vyřaďte z provozu selektor neshoda tunelu v intervalu 5 minut|ConnectionName, Vzdálená adresa IP|

## <a name="microsoftnetworkexpressroutecircuits"></a>Microsoft.Network/expressRouteCircuits

|Metrika|Metriky zobrazovaný název|Jednotka|Typ agregace|Popis|Dimenze|
|---|---|---|---|---|---|
|BytesIn|BytesIn|Počet|Celkem|Bajty ingressing Azure|Žádné dimenze|
|BytesOut|BytesOut|Počet|Celkem|Bajty egressing Azure|Žádné dimenze|
|BitsInPerSecond|BitsInPerSecond|CountPerSecond|Průměr|Služba BITS ingressing Azure za sekundu|Žádné dimenze|
|BitsOutPerSecond|BitsOutPerSecond|CountPerSecond|Průměr|Služba BITS egressing Azure za sekundu|Žádné dimenze|

## <a name="microsoftnetworktrafficmanagerprofiles"></a>Microsoft.Network/trafficManagerProfiles

|Metrika|Metriky zobrazovaný název|Jednotka|Typ agregace|Popis|Dimenze|
|---|---|---|---|---|---|
|QpsByEndpoint|Dotazy pomocí vrátil koncový bod|Počet|Celkem|Kolikrát koncový bod Traffic Manager vrátil v zadaném časovém období|EndpointName|
|ProbeAgentCurrentEndpointStateByProfileResourceId|Stav koncový bod koncovým bodem|Počet|Maximální počet|1, pokud koncového bodu testu stav "povoleno", jinak hodnota 0.|EndpointName|

## <a name="microsoftnotificationhubsnamespacesnotificationhubs"></a>Microsoft.NotificationHubs/Namespaces/NotificationHubs

|Metrika|Metriky zobrazovaný název|Jednotka|Typ agregace|Popis|Dimenze|
|---|---|---|---|---|---|
|Registration.all|Registrace operací|Počet|Celkem|Počet všech operací úspěšnou registraci (vytváření dotazů aktualizace a odstranění). |Žádné dimenze|
|Registration.Create|Registrace vytvoření operací|Počet|Celkem|Počet vytváření všech úspěšnou registraci.|Žádné dimenze|
|Registration.Update|Operace aktualizace registrace|Počet|Celkem|Počet všech aktualizací úspěšnou registraci.|Žádné dimenze|
|Registration.Get|Operace čtení registrace|Počet|Celkem|Počet všech dotazů úspěšnou registraci.|Žádné dimenze|
|Registration.delete|Operace odstranění registrace|Počet|Celkem|Počet všech odstranění úspěšnou registraci.|Žádné dimenze|
|příchozí|Příchozí zprávy|Počet|Celkem|Počet všech úspěšné odeslání volání rozhraní API. |Žádné dimenze|
|Incoming.Scheduled|Naplánované nabízená oznámení odesílaná|Počet|Celkem|Naplánované nabízená oznámení zrušena|Žádné dimenze|
|Incoming.Scheduled.Cancel|Naplánované nabízená oznámení zrušena|Počet|Celkem|Naplánované nabízená oznámení zrušena|Žádné dimenze|
|Scheduled.Pending|Čekající naplánované oznámení|Počet|Celkem|Čekající naplánované oznámení|Žádné dimenze|
|Installation.all|Operace správy instalace|Počet|Celkem|Operace správy instalace|Žádné dimenze|
|Installation.Get|Získat operace instalace|Počet|Celkem|Získat operace instalace|Žádné dimenze|
|Installation.upsert|Vytvořit nebo aktualizovat operace instalace|Počet|Celkem|Vytvořit nebo aktualizovat operace instalace|Žádné dimenze|
|Installation.patch|Operace instalace opravy|Počet|Celkem|Operace instalace opravy|Žádné dimenze|
|Installation.delete|Instalace operace odstranění|Počet|Celkem|Instalace operace odstranění|Žádné dimenze|
|outgoing.allpns.Success|Úspěšné oznámení|Počet|Celkem|Počet všech úspěšné oznámení.|Žádné dimenze|
|outgoing.allpns.invalidpayload|Datová část chyby|Počet|Celkem|Počet oznámení, které se nezdařila, protože systém PNS vrátil chybu chybná datová část.|Žádné dimenze|
|outgoing.allpns.pnserror|Chyby systému externí oznámení|Počet|Celkem|Počet oznámení, které se nezdařila, protože došlo k potížím při komunikaci se Správou (nezahrnuje problémy ověřování).|Žádné dimenze|
|outgoing.allpns.channelerror|Kanál chyby|Počet|Celkem|Počet nabízených oznámení, která se nezdařila, protože kanál byla neplatná nejsou přidružené aplikace správné omezeny nebo jeho platnost.|Žádné dimenze|
|outgoing.allpns.badorexpiredchannel|Chybný nebo vypršela její platnost kanál chyby|Počet|Celkem|Počet nabízených oznámení, která se nezdařila, protože vypršela platnost, nebo neplatný kanál/token nebo registrationId v registraci.|Žádné dimenze|
|outgoing.wns.Success|Úspěšné oznámení WNS|Počet|Celkem|Počet všech úspěšné oznámení.|Žádné dimenze|
|outgoing.wns.invalidcredentials|Chyb autorizace WNS (Neplatné přihlašovací údaje)|Počet|Celkem|Počet nabízených oznámení, která se nezdařila, protože systém PNS nebyl přijat zadané přihlašovací údaje nebo přihlašovací údaje jsou blokovány. (Windows Live nerozpoznal přihlašovací údaje).|Žádné dimenze|
|outgoing.wns.badchannel|Chyba WNS chybný kanál|Počet|Celkem|Počet nabízených oznámení, která se nezdařila, protože nebyla rozpoznána ChannelURI v registraci (WNS stav: 404 nebyl nalezen).|Žádné dimenze|
|outgoing.wns.expiredchannel|WNS platnost Chyba kanálu|Počet|Celkem|Počet nabízených oznámení, která se nezdařila, protože vypršela platnost ChannelURI (WNS stav: 410 Gone).|Žádné dimenze|
|outgoing.wns.throttled|WNS omezeny oznámení|Počet|Celkem|Počet nabízených oznámení, která se nezdařila, protože WNS je omezení této aplikace (WNS stav: 406 Nepřijatelný).|Žádné dimenze|
|outgoing.wns.tokenproviderunreachable|Chyb autorizace WNS (nedostupné)|Počet|Celkem|Windows Live není dostupný.|Žádné dimenze|
|outgoing.wns.invalidtoken|Chyb autorizace WNS (neplatný Token)|Počet|Celkem|Zadaný pro WNS token není platný (WNS stav: 401 – Neověřeno).|Žádné dimenze|
|outgoing.wns.wrongtoken|Chyb autorizace WNS (chybný Token)|Počet|Celkem|Zadaný pro WNS token je platný, ale pro jinou aplikaci (WNS stav: 403 Zakázáno). To může nastat, když ChannelURI v registraci souvisí s jinou aplikací. Zkontrolujte, jestli klientská aplikace je přidružená ke stejné aplikaci, jehož pověření se v centru oznámení.|Žádné dimenze|
|outgoing.wns.invalidnotificationformat|Formát neplatný oznámení WNS|Počet|Celkem|Formát oznámení není platný (stav WNS: 400). Všimněte si, že WNS není odmítnout všechny neplatné datové části.|Žádné dimenze|
|outgoing.wns.invalidnotificationsize|WNS Chyba neplatné velikosti oznámení|Počet|Celkem|Datová část oznámení je moc velký (WNS stav: 413).|Žádné dimenze|
|outgoing.wns.channelthrottled|Kanál WNS omezení|Počet|Celkem|Oznámení byla vyřazena, protože se omezuje ChannelURI v registraci (hlavička odpovědi WNS: X-WNS-NotificationStatus:channelThrottled).|Žádné dimenze|
|outgoing.wns.channeldisconnected|Kanál WNS odpojení|Počet|Celkem|Oznámení byla vyřazena, protože se omezuje ChannelURI v registraci (hlavička odpovědi WNS: X-WNS-DeviceConnectionStatus: odpojení).|Žádné dimenze|
|outgoing.wns.dropped|WNS vyřadit oznámení|Počet|Celkem|Oznámení byla vyřazena, protože se omezuje ChannelURI v registraci (X-WNS-NotificationStatus: vynechaných, ale není X-WNS-DeviceConnectionStatus: odpojení).|Žádné dimenze|
|outgoing.wns.pnserror|Chyby WNS|Počet|Celkem|Oznámení nejsou doručeny z důvodu chyby komunikace s WNS.|Žádné dimenze|
|outgoing.wns.authenticationerror|Chyby ověřování WNS|Počet|Celkem|Oznámení nejsou doručeny z důvodu chyby komunikace s Windows Live neplatné přihlašovací údaje nebo chybný token.|Žádné dimenze|
|outgoing.apns.Success|Úspěšné oznámení APNS|Počet|Celkem|Počet všech úspěšné oznámení.|Žádné dimenze|
|outgoing.apns.invalidcredentials|Chyb autorizace služby APN|Počet|Celkem|Počet nabízených oznámení, která se nezdařila, protože systém PNS nebyl přijat zadané přihlašovací údaje nebo přihlašovací údaje jsou blokovány.|Žádné dimenze|
|outgoing.apns.badchannel|Chyba služby APN chybný kanál|Počet|Celkem|Počet nabízených oznámení, která se nezdařila, protože token je neplatný (kód stavu služby APN: 8).|Žádné dimenze|
|outgoing.apns.expiredchannel|APNS vypršela platnost Chyba kanálu|Počet|Celkem|Počet token, který došlo ke zrušení platnosti podle kanálu zpětné vazby služby APN.|Žádné dimenze|
|outgoing.apns.invalidnotificationsize|APNS Chyba neplatné velikosti oznámení|Počet|Celkem|Počet nabízených oznámení, která se nezdařila, protože byla příliš velká datová část (APNS stavový kód: 7).|Žádné dimenze|
|outgoing.apns.pnserror|Chyby služby APN|Počet|Celkem|Počet nabízených oznámení, která se nezdařila z důvodu chyb, komunikaci se službou APNS.|Žádné dimenze|
|outgoing.gcm.Success|Úspěšné oznámení GCM|Počet|Celkem|Počet všech úspěšné oznámení.|Žádné dimenze|
|outgoing.gcm.invalidcredentials|Chyb autorizace služby GCM (Neplatné přihlašovací údaje)|Počet|Celkem|Počet nabízených oznámení, která se nezdařila, protože systém PNS nebyl přijat zadané přihlašovací údaje nebo přihlašovací údaje jsou blokovány.|Žádné dimenze|
|outgoing.gcm.badchannel|Chyba GCM chybný kanál|Počet|Celkem|Počet nabízených oznámení, která se nezdařila, protože nebyla rozpoznána registrationId v registraci (GCM výsledek: Neplatná registrační).|Žádné dimenze|
|outgoing.gcm.expiredchannel|GCM platnost Chyba kanálu|Počet|Celkem|Počet nabízených oznámení, která se nezdařila, protože platnost vypršela registrationId v registraci (GCM výsledek: NotRegistered).|Žádné dimenze|
|outgoing.gcm.throttled|Omezení oznámení GCM|Počet|Celkem|Počet nabízených oznámení, která se nezdařila, protože GCM omezuje tuto aplikaci (GCM stavový kód: 501-599 nebo výsledek: není k dispozici).|Žádné dimenze|
|outgoing.gcm.invalidnotificationformat|Formát neplatný oznámení GCM|Počet|Celkem|Počet nabízených oznámení, která se nezdařila, protože nebyla správně naformátována datové části (GCM výsledek: InvalidDataKey nebo InvalidTtl).|Žádné dimenze|
|outgoing.gcm.invalidnotificationsize|GCM Chyba neplatné velikosti oznámení|Počet|Celkem|Počet nabízených oznámení, která se nezdařila, protože byla příliš velká datová část (GCM výsledek: MessageTooBig).|Žádné dimenze|
|outgoing.gcm.wrongchannel|Chyba GCM nesprávné kanálu|Počet|Celkem|Počet oznámení, které se nezdařila, protože není přidružen k aktuální aplikaci registrationId v registraci (GCM výsledek: InvalidPackageName).|Žádné dimenze|
|outgoing.gcm.pnserror|Chyby GCM|Počet|Celkem|Počet nabízených oznámení, která se nezdařila z důvodu chyb, komunikaci se službou GCM.|Žádné dimenze|
|outgoing.gcm.authenticationerror|Chyby ověřování GCM|Počet|Celkem|Počet oznámení, které se nezdařila, protože systém PNS nebyl přijat zadaných pověření přihlašovací údaje jsou zablokované nebo ID odesílatele není správně nakonfigurována v aplikaci (GCM výsledek: MismatchedSenderId).|Žádné dimenze|
|outgoing.mpns.Success|Úspěšné oznámení MPNS|Počet|Celkem|Počet všech úspěšné oznámení.|Žádné dimenze|
|outgoing.mpns.invalidcredentials|Neplatné přihlašovací údaje MPNS|Počet|Celkem|Počet nabízených oznámení, která se nezdařila, protože systém PNS nebyl přijat zadané přihlašovací údaje nebo přihlašovací údaje jsou blokovány.|Žádné dimenze|
|outgoing.mpns.badchannel|Chyba MPNS chybný kanál|Počet|Celkem|Počet nabízených oznámení, která se nezdařila, protože nebyla rozpoznána ChannelURI v registraci (MPNS stav: 404 nebyl nalezen).|Žádné dimenze|
|outgoing.mpns.throttled|MPNS omezeny oznámení|Počet|Celkem|Počet nabízených oznámení, která se nezdařila, protože MPNS se omezení této aplikace (WNS MPNS: 406 Nepřijatelný).|Žádné dimenze|
|outgoing.mpns.invalidnotificationformat|Formát neplatný oznámení MPNS|Počet|Celkem|Počet nabízených oznámení, která se nezdařila, protože datová část oznámení byla příliš velká.|Žádné dimenze|
|outgoing.mpns.channeldisconnected|Kanál MPNS odpojení|Počet|Celkem|Počet nabízených oznámení, která se nezdařila, protože byla odpojena ChannelURI v registraci (MPNS stav: 412 nebyl nalezen).|Žádné dimenze|
|outgoing.mpns.dropped|MPNS vyřadit oznámení|Počet|Celkem|Počet oznámení, které byly vynechány podle MPNS (MPNS hlavičku odpovědi: X NotificationStatus: QueueFull nebo Suppressed).|Žádné dimenze|
|outgoing.mpns.pnserror|Chyby MPNS|Počet|Celkem|Počet nabízených oznámení, která se nezdařilo z důvodu chyby komunikace se MPNS.|Žádné dimenze|
|outgoing.mpns.authenticationerror|Chyby ověřování MPNS|Počet|Celkem|Počet nabízených oznámení, která se nezdařila, protože systém PNS nebyl přijat zadané přihlašovací údaje nebo přihlašovací údaje jsou blokovány.|Žádné dimenze|
|notificationhub.pushes|Všechny odchozí oznámení|Počet|Celkem|Všechny odchozí oznámení centra oznámení|Žádné dimenze|
|Incoming.all.Requests|Všechny příchozí požadavky|Počet|Celkem|Celkový počet příchozích požadavků pro Centrum oznámení|Žádné dimenze|
|Incoming.all.failedrequests|Všechny příchozí neúspěšné požadavky|Počet|Celkem|Celkový počet příchozích neúspěšných požadavků pro Centrum oznámení|Žádné dimenze|

## <a name="microsoftsearchsearchservices"></a>Microsoft.Search/searchServices

|Metrika|Metriky zobrazovaný název|Jednotka|Typ agregace|Popis|Dimenze|
|---|---|---|---|---|---|
|SearchLatency|Latence vyhledávání|Sekundy|Průměr|Hledání Průměrná latence pro službu vyhledávání.|Žádné dimenze|
|SearchQueriesPerSecond|Vyhledávací dotazy za sekundu|CountPerSecond|Průměr|Vyhledávací dotazy za sekundu pro službu vyhledávání.|Žádné dimenze|
|ThrottledSearchQueriesPercentage|Procento omezenému vyhledávací dotazy|Procento|Průměr|Procento vyhledávací dotazy, které byly omezené pro službu vyhledávání.|Žádné dimenze|

## <a name="microsoftservicebusnamespaces"></a>Microsoft.ServiceBus/namespaces

|Metrika|Metriky zobrazovaný název|Jednotka|Typ agregace|Popis|Dimenze|
|---|---|---|---|---|---|
|CPUXNS|Využití procesoru na obor názvů|Procento|Maximální počet|Service bus premium obor názvů procesoru využití metrika|Žádné dimenze|
|WSXNS|Velikost využití paměti na obor názvů|Procento|Maximální počet|Service bus premium obor názvů paměti využití metrika|Žádné dimenze|

## <a name="microsoftsqlserversdatabases"></a>Microsoft.Sql/servers/databases

|Metrika|Metriky zobrazovaný název|Jednotka|Typ agregace|Popis|Dimenze|
|---|---|---|---|---|---|
|cpu_percent|Procento CPU|Procento|Průměr|Procento CPU|Žádné dimenze|
|physical_data_read_percent|Procento datových V/V|Procento|Průměr|Procento datových V/V|Žádné dimenze|
|log_write_percent|Procento vstupně-výstupní operace protokolu|Procento|Průměr|Procento vstupně-výstupní operace protokolu|Žádné dimenze|
|dtu_consumption_percent|Procento DTU|Procento|Průměr|Procento DTU|Žádné dimenze|
|úložiště|Velikost celkový databáze|Bajty|Maximální počet|Velikost celkový databáze|Žádné dimenze|
|connection_successful|Úspěšné připojení|Počet|Celkem|Úspěšné připojení|Žádné dimenze|
|connection_failed|Neúspěšné připojení|Počet|Celkem|Neúspěšné připojení|Žádné dimenze|
|blocked_by_firewall|Blokováno bránou Firewall|Počet|Celkem|Blokováno bránou Firewall|Žádné dimenze|
|zablokování|Zablokování|Počet|Celkem|Zablokování|Žádné dimenze|
|storage_percent|Procento velikosti databáze|Procento|Maximální počet|Procento velikosti databáze|Žádné dimenze|
|xtp_storage_percent|Procento úložiště OLTP v paměti|Procento|Průměr|Procento úložiště OLTP v paměti|Žádné dimenze|
|workers_percent|Procento pracovních procesů|Procento|Průměr|Procento pracovních procesů|Žádné dimenze|
|sessions_percent|Procento relací|Procento|Průměr|Procento relací|Žádné dimenze|
|dtu_limit|Omezení jednotek DTU|Počet|Průměr|Omezení jednotek DTU|Žádné dimenze|
|dtu_used|Použít DTU|Počet|Průměr|Použít DTU|Žádné dimenze|
|dwu_limit|DWU limit|Počet|Maximální počet|DWU limit|Žádné dimenze|
|dwu_consumption_percent|Procento DWU|Procento|Maximální počet|Procento DWU|Žádné dimenze|
|dwu_used|DWU použít|Počet|Maximální počet|DWU použít|Žádné dimenze|
|dw_cpu_percent|Úroveň uzlu DW procento využití procesoru|Procento|Průměr|Úroveň uzlu DW procento využití procesoru|dw_logical_node_id|
|dw_physical_data_read_percent|Procento úroveň vstupně-výstupní operace dat datového skladu uzlu|Procento|Průměr|Procento úroveň vstupně-výstupní operace dat datového skladu uzlu|dw_logical_node_id|

## <a name="microsoftsqlserverselasticpools"></a>Microsoft.Sql/servers/elasticPools

|Metrika|Metriky zobrazovaný název|Jednotka|Typ agregace|Popis|Dimenze|
|---|---|---|---|---|---|
|cpu_percent|Procento CPU|Procento|Průměr|Procento CPU|Žádné dimenze|
|database_cpu_percent|Procento CPU|Procento|Průměr|Procento CPU|DatabaseResourceId|
|physical_data_read_percent|Procento datových V/V|Procento|Průměr|Procento datových V/V|Žádné dimenze|
|database_physical_data_read_percent|Procento datových V/V|Procento|Průměr|Procento datových V/V|DatabaseResourceId|
|log_write_percent|Procento vstupně-výstupní operace protokolu|Procento|Průměr|Procento vstupně-výstupní operace protokolu|Žádné dimenze|
|database_log_write_percent|Procento vstupně-výstupní operace protokolu|Procento|Průměr|Procento vstupně-výstupní operace protokolu|DatabaseResourceId|
|dtu_consumption_percent|Procento DTU|Procento|Průměr|Procento DTU|Žádné dimenze|
|database_dtu_consumption_percent|Procento DTU|Procento|Průměr|Procento DTU|DatabaseResourceId|
|storage_percent|Procento úložiště|Procento|Průměr|Procento úložiště|Žádné dimenze|
|workers_percent|Procento pracovních procesů|Procento|Průměr|Procento pracovních procesů|Žádné dimenze|
|database_workers_percent|Procento pracovních procesů|Procento|Průměr|Procento pracovních procesů|DatabaseResourceId|
|sessions_percent|Procento relací|Procento|Průměr|Procento relací|Žádné dimenze|
|database_sessions_percent|Procento relací|Procento|Průměr|Procento relací|DatabaseResourceId|
|eDTU_limit|omezení eDTU|Počet|Průměr|omezení eDTU|Žádné dimenze|
|storage_limit|Limit úložiště|Bajty|Průměr|Limit úložiště|Žádné dimenze|
|eDTU_used|eDTU použít|Počet|Průměr|eDTU použít|Žádné dimenze|
|storage_used|Využité úložiště|Bajty|Průměr|Využité úložiště|Žádné dimenze|
|database_storage_used|Využité úložiště|Bajty|Průměr|Využité úložiště|DatabaseResourceId|
|xtp_storage_percent|Procento úložiště OLTP v paměti|Procento|Průměr|Procento úložiště OLTP v paměti|Žádné dimenze|

## <a name="microsoftsqlservers"></a>Microsoft.Sql/servers

|Metrika|Metriky zobrazovaný název|Jednotka|Typ agregace|Popis|Dimenze|
|---|---|---|---|---|---|
|dtu_consumption_percent|Procento DTU|Procento|Průměr|Procento DTU|ElasticPoolResourceId|
|database_dtu_consumption_percent|Procento DTU|Procento|Průměr|Procento DTU|DatabaseResourceId ElasticPoolResourceId|
|storage_used|Využité úložiště|Bajty|Průměr|Využité úložiště|ElasticPoolResourceId|
|database_storage_used|Využité úložiště|Bajty|Průměr|Využité úložiště|DatabaseResourceId ElasticPoolResourceId|

## <a name="microsoftstoragestorageaccounts"></a>Microsoft.Storage/storageAccounts.

|Metrika|Metriky zobrazovaný název|Jednotka|Typ agregace|Popis|Dimenze|
|---|---|---|---|---|---|
|UsedCapacity|Využité kapacitě|Bajty|Průměr|Účet používaný kapacity|Žádné dimenze|
|Transakce|Transakce|Počet|Celkem|Počet požadavků na služby úložiště nebo zadaná operace rozhraní API. Tato hodnota zahrnuje úspěšné i neúspěšné požadavky, jakož i požadavků, které vytváří chyby. Hodnota ResponseType dimenzi používejte pro počet jiný typ odpovědi.|Hodnota ResponseType, GeoType, ApiName|
|Příchozí přenos dat|Příchozí přenos dat|Bajty|Celkem|Množství dat příjem příchozích dat v bajtech. Tato hodnota zahrnuje příjem příchozích dat z externí klienta do Azure Storage, jakož i příjem příchozích dat v rámci Azure.|GeoType ApiName|
|Odchozí|Odchozí|Bajty|Celkem|Množství odchozí data v bajtech. Tato hodnota zahrnuje odchozí z externí klienta do Azure Storage a také odchozí v rámci Azure. Toto číslo v důsledku toho nemusí odpovídat fakturovatelný odchozí.|GeoType ApiName|
|SuccessServerLatency|Latence serveru úspěšné|počet milisekund|Průměr|Průměrná latence úložiště Azure používá ke zpracování v případě úspěšné žádosti v milisekundách. Tato hodnota nezahrnuje latenci sítě, zadaný v AverageE2ELatency.|GeoType ApiName|
|SuccessE2ELatency|Latence E2E úspěch|počet milisekund|Průměr|Průměrná latence začátku do konce úspěšné požadavky na služby úložiště nebo zadaná operace rozhraní API, v milisekundách. Tato hodnota zahrnuje požadovaná doba zpracování v rámci Azure Storage pro čtení požadavku, odeslání odpovědi a přijímat potvrzení odpovědi.|GeoType ApiName|
|Dostupnost|Dostupnost|Procento|Průměr|Procento dostupnosti pro službu úložiště nebo zadaná operace rozhraní API. Dostupnost se vypočítává tak, že hodnota TotalBillableRequests dělení podle počtu příslušné požadavky, včetně těch, které vytváří neočekávaným chybám. Všechny neočekávaným chybám výsledkem je omezená dostupnost pro službu úložiště nebo zadaná operace rozhraní API.|GeoType ApiName|

## <a name="microsoftstoragestorageaccountsblobservices"></a>Microsoft.Storage/storageAccounts/blobServices

|Metrika|Metriky zobrazovaný název|Jednotka|Typ agregace|Popis|Dimenze|
|---|---|---|---|---|---|
|BlobCapacity|Objekt BLOB kapacity|Bajty|Průměr|Velikost úložiště, které používá služba účtu úložiště objektů Blob v bajtech.|BlobType|
|BlobCount|Počet objektů BLOB|Počet|Průměr|Počet objektů Blob v účtu úložiště služby objektů Blob.|BlobType|
|ContainerCount|Počet kontejner objektů BLOB|Počet|Průměr|Počet kontejnerů v účtu úložiště služby objektů Blob.|Žádné dimenze|
|Transakce|Transakce|Počet|Celkem|Počet požadavků na služby úložiště nebo zadaná operace rozhraní API. Tato hodnota zahrnuje úspěšné i neúspěšné požadavky, jakož i požadavků, které vytváří chyby. Hodnota ResponseType dimenzi používejte pro počet jiný typ odpovědi.|Hodnota ResponseType, GeoType, ApiName|
|Příchozí přenos dat|Příchozí přenos dat|Bajty|Celkem|Množství dat příjem příchozích dat v bajtech. Tato hodnota zahrnuje příjem příchozích dat z externí klienta do Azure Storage, jakož i příjem příchozích dat v rámci Azure.|GeoType ApiName|
|Odchozí|Odchozí|Bajty|Celkem|Množství odchozí data v bajtech. Tato hodnota zahrnuje odchozí z externí klienta do Azure Storage a také odchozí v rámci Azure. Toto číslo v důsledku toho nemusí odpovídat fakturovatelný odchozí.|GeoType ApiName|
|SuccessServerLatency|Latence serveru úspěšné|počet milisekund|Průměr|Průměrná latence úložiště Azure používá ke zpracování v případě úspěšné žádosti v milisekundách. Tato hodnota nezahrnuje latenci sítě, zadaný v AverageE2ELatency.|GeoType ApiName|
|SuccessE2ELatency|Latence E2E úspěch|počet milisekund|Průměr|Průměrná latence začátku do konce úspěšné požadavky na služby úložiště nebo zadaná operace rozhraní API, v milisekundách. Tato hodnota zahrnuje požadovaná doba zpracování v rámci Azure Storage pro čtení požadavku, odeslání odpovědi a přijímat potvrzení odpovědi.|GeoType ApiName|
|Dostupnost|Dostupnost|Procento|Průměr|Procento dostupnosti pro službu úložiště nebo zadaná operace rozhraní API. Dostupnost se vypočítává tak, že hodnota TotalBillableRequests dělení podle počtu příslušné požadavky, včetně těch, které vytváří neočekávaným chybám. Všechny neočekávaným chybám výsledkem je omezená dostupnost pro službu úložiště nebo zadaná operace rozhraní API.|GeoType ApiName|

## <a name="microsoftstoragestorageaccountstableservices"></a>Microsoft.Storage/storageAccounts/tableServices

|Metrika|Metriky zobrazovaný název|Jednotka|Typ agregace|Popis|Dimenze|
|---|---|---|---|---|---|
|TableCapacity|Tabulka kapacity|Bajty|Průměr|Velikost úložiště používá účet úložiště služby Table v bajtech.|Žádné dimenze|
|TableCount|Počet tabulky|Počet|Průměr|Počet tabulky v účtu úložiště služby Table.|Žádné dimenze|
|TableEntityCount|Počet entit tabulky|Počet|Průměr|Počet entit tabulky v účtu úložiště služby Table.|Žádné dimenze|
|Transakce|Transakce|Počet|Celkem|Počet požadavků na služby úložiště nebo zadaná operace rozhraní API. Tato hodnota zahrnuje úspěšné i neúspěšné požadavky, jakož i požadavků, které vytváří chyby. Hodnota ResponseType dimenzi používejte pro počet jiný typ odpovědi.|Hodnota ResponseType, GeoType, ApiName|
|Příchozí přenos dat|Příchozí přenos dat|Bajty|Celkem|Množství dat příjem příchozích dat v bajtech. Tato hodnota zahrnuje příjem příchozích dat z externí klienta do Azure Storage, jakož i příjem příchozích dat v rámci Azure.|GeoType ApiName|
|Odchozí|Odchozí|Bajty|Celkem|Množství odchozí data v bajtech. Tato hodnota zahrnuje odchozí z externí klienta do Azure Storage a také odchozí v rámci Azure. Toto číslo v důsledku toho nemusí odpovídat fakturovatelný odchozí.|GeoType ApiName|
|SuccessServerLatency|Latence serveru úspěšné|počet milisekund|Průměr|Průměrná latence úložiště Azure používá ke zpracování v případě úspěšné žádosti v milisekundách. Tato hodnota nezahrnuje latenci sítě, zadaný v AverageE2ELatency.|GeoType ApiName|
|SuccessE2ELatency|Latence E2E úspěch|počet milisekund|Průměr|Průměrná latence začátku do konce úspěšné požadavky na služby úložiště nebo zadaná operace rozhraní API, v milisekundách. Tato hodnota zahrnuje požadovaná doba zpracování v rámci Azure Storage pro čtení požadavku, odeslání odpovědi a přijímat potvrzení odpovědi.|GeoType ApiName|
|Dostupnost|Dostupnost|Procento|Průměr|Procento dostupnosti pro službu úložiště nebo zadaná operace rozhraní API. Dostupnost se vypočítává tak, že hodnota TotalBillableRequests dělení podle počtu příslušné požadavky, včetně těch, které vytváří neočekávaným chybám. Všechny neočekávaným chybám výsledkem je omezená dostupnost pro službu úložiště nebo zadaná operace rozhraní API.|GeoType ApiName|

## <a name="microsoftstoragestorageaccountsqueueservices"></a>Microsoft.Storage/storageAccounts/queueServices

|Metrika|Metriky zobrazovaný název|Jednotka|Typ agregace|Popis|Dimenze|
|---|---|---|---|---|---|
|QueueCapacity|Fronty kapacity|Bajty|Průměr|Velikost úložiště, které používá služba účet úložiště fronty v bajtech.|Žádné dimenze|
|QueueCount|Počet fronty|Počet|Průměr|Počet fronty v účtu úložiště služby front.|Žádné dimenze|
|QueueMessageCount|Počet zpráv fronty|Počet|Průměr|Přibližný počet fronty zpráv v účtu úložiště služby front.|Žádné dimenze|
|Transakce|Transakce|Počet|Celkem|Počet požadavků na služby úložiště nebo zadaná operace rozhraní API. Tato hodnota zahrnuje úspěšné i neúspěšné požadavky, jakož i požadavků, které vytváří chyby. Hodnota ResponseType dimenzi používejte pro počet jiný typ odpovědi.|Hodnota ResponseType, GeoType, ApiName|
|Příchozí přenos dat|Příchozí přenos dat|Bajty|Celkem|Množství dat příjem příchozích dat v bajtech. Tato hodnota zahrnuje příjem příchozích dat z externí klienta do Azure Storage, jakož i příjem příchozích dat v rámci Azure.|GeoType ApiName|
|Odchozí|Odchozí|Bajty|Celkem|Množství odchozí data v bajtech. Tato hodnota zahrnuje odchozí z externí klienta do Azure Storage a také odchozí v rámci Azure. Toto číslo v důsledku toho nemusí odpovídat fakturovatelný odchozí.|GeoType ApiName|
|SuccessServerLatency|Latence serveru úspěšné|počet milisekund|Průměr|Průměrná latence úložiště Azure používá ke zpracování v případě úspěšné žádosti v milisekundách. Tato hodnota nezahrnuje latenci sítě, zadaný v AverageE2ELatency.|GeoType ApiName|
|SuccessE2ELatency|Latence E2E úspěch|počet milisekund|Průměr|Průměrná latence začátku do konce úspěšné požadavky na služby úložiště nebo zadaná operace rozhraní API, v milisekundách. Tato hodnota zahrnuje požadovaná doba zpracování v rámci Azure Storage pro čtení požadavku, odeslání odpovědi a přijímat potvrzení odpovědi.|GeoType ApiName|
|Dostupnost|Dostupnost|Procento|Průměr|Procento dostupnosti pro službu úložiště nebo zadaná operace rozhraní API. Dostupnost se vypočítává tak, že hodnota TotalBillableRequests dělení podle počtu příslušné požadavky, včetně těch, které vytváří neočekávaným chybám. Všechny neočekávaným chybám výsledkem je omezená dostupnost pro službu úložiště nebo zadaná operace rozhraní API.|GeoType ApiName|

## <a name="microsoftstoragestorageaccountsfileservices"></a>Microsoft.Storage/storageAccounts/fileServices

|Metrika|Metriky zobrazovaný název|Jednotka|Typ agregace|Popis|Dimenze|
|---|---|---|---|---|---|
|FileCapacity|Soubor kapacity|Bajty|Průměr|Velikost úložiště, které používá služba účet úložiště souboru v bajtech.|Žádné dimenze|
|FileCount|Počet souborů|Počet|Průměr|Počet souborů v účtu úložiště služby souborů.|Žádné dimenze|
|FileShareCount|Počet sdílené složky souborů|Počet|Průměr|Počet souborů sdílených složek v účtu úložiště služby souborů.|Žádné dimenze|
|Transakce|Transakce|Počet|Celkem|Počet požadavků na služby úložiště nebo zadaná operace rozhraní API. Tato hodnota zahrnuje úspěšné i neúspěšné požadavky, jakož i požadavků, které vytváří chyby. Hodnota ResponseType dimenzi používejte pro počet jiný typ odpovědi.|Hodnota ResponseType, GeoType, ApiName|
|Příchozí přenos dat|Příchozí přenos dat|Bajty|Celkem|Množství dat příjem příchozích dat v bajtech. Tato hodnota zahrnuje příjem příchozích dat z externí klienta do Azure Storage, jakož i příjem příchozích dat v rámci Azure.|GeoType ApiName|
|Odchozí|Odchozí|Bajty|Celkem|Množství odchozí data v bajtech. Tato hodnota zahrnuje odchozí z externí klienta do Azure Storage a také odchozí v rámci Azure. Toto číslo v důsledku toho nemusí odpovídat fakturovatelný odchozí.|GeoType ApiName|
|SuccessServerLatency|Latence serveru úspěšné|počet milisekund|Průměr|Průměrná latence úložiště Azure používá ke zpracování v případě úspěšné žádosti v milisekundách. Tato hodnota nezahrnuje latenci sítě, zadaný v AverageE2ELatency.|GeoType ApiName|
|SuccessE2ELatency|Latence E2E úspěch|počet milisekund|Průměr|Průměrná latence začátku do konce úspěšné požadavky na služby úložiště nebo zadaná operace rozhraní API, v milisekundách. Tato hodnota zahrnuje požadovaná doba zpracování v rámci Azure Storage pro čtení požadavku, odeslání odpovědi a přijímat potvrzení odpovědi.|GeoType ApiName|
|Dostupnost|Dostupnost|Procento|Průměr|Procento dostupnosti pro službu úložiště nebo zadaná operace rozhraní API. Dostupnost se vypočítává tak, že hodnota TotalBillableRequests dělení podle počtu příslušné požadavky, včetně těch, které vytváří neočekávaným chybám. Všechny neočekávaným chybám výsledkem je omezená dostupnost pro službu úložiště nebo zadaná operace rozhraní API.|GeoType ApiName|

## <a name="microsoftstreamanalyticsstreamingjobs"></a>Microsoft.StreamAnalytics/streamingjobs

|Metrika|Metriky zobrazovaný název|Jednotka|Typ agregace|Popis|Dimenze|
|---|---|---|---|---|---|
|ResourceUtilization|% Využití SU|Procento|Maximální počet|% Využití SU|Žádné dimenze|
|InputEvents|Vstupní události|Počet|Celkem|Vstupní události|Žádné dimenze|
|InputEventBytes|Vstupní událost bajtů|Bajty|Celkem|Vstupní událost bajtů|Žádné dimenze|
|LateInputEvents|Pozdní vstupní události|Počet|Celkem|Pozdní vstupní události|Žádné dimenze|
|OutputEvents|Výstup události|Počet|Celkem|Výstup události|Žádné dimenze|
|ConversionErrors|Chyby při převodu dat|Počet|Celkem|Chyby při převodu dat|Žádné dimenze|
|Chyby|Chyby za běhu|Počet|Celkem|Chyby za běhu|Žádné dimenze|
|DroppedOrAdjustedEvents|Události mimo pořadí|Počet|Celkem|Události mimo pořadí|Žádné dimenze|
|AMLCalloutRequests|Požadavky funkce|Počet|Celkem|Požadavky funkce|Žádné dimenze|
|AMLCalloutFailedRequests|Požadavky neúspěšné funkce|Počet|Celkem|Požadavky neúspěšné funkce|Žádné dimenze|
|AMLCalloutInputEvents|Události – funkce|Počet|Celkem|Události – funkce|Žádné dimenze|

## <a name="microsoftwebserverfarms"></a>Microsoft.Web/serverfarms

|Metrika|Metriky zobrazovaný název|Jednotka|Typ agregace|Popis|Dimenze|
|---|---|---|---|---|---|
|CpuPercentage|Procento procesoru|Procento|Průměr|Procento procesoru|Instance|
|MemoryPercentage|Procento paměti|Procento|Průměr|Procento paměti|Instance|
|DiskQueueLength|Délka fronty disku|Počet|Celkem|Délka fronty disku|Instance|
|HttpQueueLength|Délka fronty http|Počet|Celkem|Délka fronty http|Instance|
|BytesReceived|Data v|Bajty|Celkem|Data v|Instance|
|BytesSent|Data|Bajty|Celkem|Data|Instance|

## <a name="microsoftwebsites-excluding-functions"></a>Microsoft.Web/sites (s výjimkou funkce)

|Metrika|Metriky zobrazovaný název|Jednotka|Typ agregace|Popis|Dimenze|
|---|---|---|---|---|---|
|CpuTime|Čas procesoru|Sekundy|Celkem|Čas procesoru|Instance|
|Požadavky|Požadavky|Počet|Celkem|Požadavky|Instance|
|BytesReceived|Data v|Bajty|Celkem|Data v|Instance|
|BytesSent|Data|Bajty|Celkem|Data|Instance|
|Http101|HTTP 101|Počet|Celkem|HTTP 101|Instance|
|Http2xx|Http 2xx|Počet|Celkem|Http 2xx|Instance|
|Http3xx|Http 3xx|Počet|Celkem|Http 3xx|Instance|
|Http401|HTTP 401|Počet|Celkem|HTTP 401|Instance|
|Http403|HTTP 403|Počet|Celkem|HTTP 403|Instance|
|Http404|HTTP 404|Počet|Celkem|HTTP 404|Instance|
|Http406|HTTP 406|Počet|Celkem|HTTP 406|Instance|
|Http4xx|Http 4xx|Počet|Celkem|Http 4xx|Instance|
|Http5xx|Chyby protokolu HTTP serveru|Počet|Celkem|Chyby protokolu HTTP serveru|Instance|
|MemoryWorkingSet|Paměť pracovní sady|Bajty|Průměr|Paměť pracovní sady|Instance|
|AverageMemoryWorkingSet|Průměrná paměti pracovní sady|Bajty|Průměr|Průměrná paměti pracovní sady|Instance|
|AverageResponseTime|Průměrná doba odezvy|Sekundy|Průměr|Průměrná doba odezvy|Instance|
|FunctionExecutionUnits|Funkce spuštění jednotky|Počet|Průměr|Funkce spuštění jednotky|Instance|
|FunctionExecutionCount|Počet provedení – funkce|Počet|Průměr|Počet provedení – funkce|Instance|

## <a name="microsoftwebsites-functions"></a>Microsoft.Web/sites (funkce)

|Metrika|Metriky zobrazovaný název|Jednotka|Typ agregace|Popis|Dimenze|
|---|---|---|---|---|---|
|BytesReceived|Data v|Bajty|Celkem|Data v|Instance|
|BytesSent|Data|Bajty|Celkem|Data|Instance|
|Http5xx|Chyby protokolu HTTP serveru|Počet|Celkem|Chyby protokolu HTTP serveru|Instance|
|MemoryWorkingSet|Paměť pracovní sady|Bajty|Průměr|Paměť pracovní sady|Instance|
|AverageMemoryWorkingSet|Průměrná paměti pracovní sady|Bajty|Průměr|Průměrná paměti pracovní sady|Instance|
|FunctionExecutionUnits|Funkce spuštění jednotky|Počet|Průměr|Funkce spuštění jednotky|Instance|
|FunctionExecutionCount|Počet provedení – funkce|Počet|Průměr|Počet provedení – funkce|Instance|

## <a name="microsoftwebsitesslots"></a>Microsoft.Web/sites/slots

|Metrika|Metriky zobrazovaný název|Jednotka|Typ agregace|Popis|Dimenze|
|---|---|---|---|---|---|
|CpuTime|Čas procesoru|Sekundy|Celkem|Čas procesoru|Instance|
|Požadavky|Požadavky|Počet|Celkem|Požadavky|Instance|
|BytesReceived|Data v|Bajty|Celkem|Data v|Instance|
|BytesSent|Data|Bajty|Celkem|Data|Instance|
|Http101|HTTP 101|Počet|Celkem|HTTP 101|Instance|
|Http2xx|Http 2xx|Počet|Celkem|Http 2xx|Instance|
|Http3xx|Http 3xx|Počet|Celkem|Http 3xx|Instance|
|Http401|HTTP 401|Počet|Celkem|HTTP 401|Instance|
|Http403|HTTP 403|Počet|Celkem|HTTP 403|Instance|
|Http404|HTTP 404|Počet|Celkem|HTTP 404|Instance|
|Http406|HTTP 406|Počet|Celkem|HTTP 406|Instance|
|Http4xx|Http 4xx|Počet|Celkem|Http 4xx|Instance|
|Http5xx|Chyby protokolu HTTP serveru|Počet|Celkem|Chyby protokolu HTTP serveru|Instance|
|MemoryWorkingSet|Paměť pracovní sady|Bajty|Průměr|Paměť pracovní sady|Instance|
|AverageMemoryWorkingSet|Průměrná paměti pracovní sady|Bajty|Průměr|Průměrná paměti pracovní sady|Instance|
|AverageResponseTime|Průměrná doba odezvy|Sekundy|Průměr|Průměrná doba odezvy|Instance|
|FunctionExecutionUnits|Funkce spuštění jednotky|Počet|Průměr|Funkce spuštění jednotky|Instance|
|FunctionExecutionCount|Počet provedení – funkce|Počet|Průměr|Počet provedení – funkce|Instance|

## <a name="microsoftwebhostingenvironmentsmultirolepools"></a>Microsoft.Web/hostingEnvironments/multiRolePools

|Metrika|Metriky zobrazovaný název|Jednotka|Typ agregace|Popis|Dimenze|
|---|---|---|---|---|---|
|Požadavky|Požadavky|Počet|Celkem|Požadavky|Instance|
|BytesReceived|Data v|Bajty|Celkem|Data v|Instance|
|BytesSent|Data|Bajty|Celkem|Data|Instance|
|Http101|HTTP 101|Počet|Celkem|HTTP 101|Instance|
|Http2xx|Http 2xx|Počet|Celkem|Http 2xx|Instance|
|Http3xx|Http 3xx|Počet|Celkem|Http 3xx|Instance|
|Http401|HTTP 401|Počet|Celkem|HTTP 401|Instance|
|Http403|HTTP 403|Počet|Celkem|HTTP 403|Instance|
|Http404|HTTP 404|Počet|Celkem|HTTP 404|Instance|
|Http406|HTTP 406|Počet|Celkem|HTTP 406|Instance|
|Http4xx|Http 4xx|Počet|Celkem|Http 4xx|Instance|
|Http5xx|Chyby protokolu HTTP serveru|Počet|Celkem|Chyby protokolu HTTP serveru|Instance|
|AverageResponseTime|Průměrná doba odezvy|Sekundy|Průměr|Průměrná doba odezvy|Instance|
|CpuPercentage|Procento procesoru|Procento|Průměr|Procento procesoru|Instance|
|MemoryPercentage|Procento paměti|Procento|Průměr|Procento paměti|Instance|
|DiskQueueLength|Délka fronty disku|Počet|Celkem|Délka fronty disku|Instance|
|HttpQueueLength|Délka fronty http|Počet|Celkem|Délka fronty http|Instance|
|ActiveRequests|Aktivních žádostí|Počet|Celkem|Aktivních žádostí|Instance|
|TotalFrontEnds|Celkový počet front-end|Počet|Průměr|Celkový počet front-end|Instance|
|SmallAppServicePlanInstances|Pracovníci plán krátkodobého používání aplikační služby|Počet|Průměr|Pracovníci plán krátkodobého používání aplikační služby|Instance|
|MediumAppServicePlanInstances|Pracovníci plán střednědobého používání aplikační služby|Počet|Průměr|Pracovníci plán střednědobého používání aplikační služby|Instance|
|LargeAppServicePlanInstances|Pracovníci plán používání aplikační služby|Počet|Průměr|Pracovníci plán používání aplikační služby|Instance|

## <a name="microsoftwebhostingenvironmentsworkerpools"></a>Microsoft.Web/hostingEnvironments/workerPools

|Metrika|Metriky zobrazovaný název|Jednotka|Typ agregace|Popis|Dimenze|
|---|---|---|---|---|---|
|Hodnoty WorkersTotal|Celkový počet pracovních procesů|Počet|Průměr|Celkový počet pracovních procesů|Instance|
|WorkersAvailable|K dispozici pracovních procesů|Počet|Průměr|K dispozici pracovních procesů|Instance|
|Workersused za|Použít pracovní procesy|Počet|Průměr|Použít pracovní procesy|Instance|

## <a name="next-steps"></a>Další kroky
* [Přečtěte si informace o metriky v Azure monitorování](monitoring-overview-metrics.md)
* [Vytvářet upozornění na metriky](insights-receive-alert-notifications.md)
* [Export metriky pro úložiště, centra událostí nebo analýzy protokolů](monitoring-overview-of-diagnostic-logs.md)
