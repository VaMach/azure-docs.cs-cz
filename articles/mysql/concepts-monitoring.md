---
title: "Monitorování pro databázi MySQL v Azure databázi | Microsoft Docs"
description: "Tento článek popisuje metriky pro monitorování a generování výstrah pro databázi Azure pro databázi MySQL, včetně procesoru, omezení, úložiště a statistických údajů o připojení."
services: mysql
author: rachel-msft
ms.author: raagyema
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.topic: article
ms.date: 10/24/2017
ms.openlocfilehash: 9af447d54faa8ee96e4b79beb274b437eea57626
ms.sourcegitcommit: b979d446ccbe0224109f71b3948d6235eb04a967
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/25/2017
---
# <a name="monitoring-in-azure-database-for-mysql"></a>Monitorování v Azure databáze pro databázi MySQL
Data o serverech monitorování pomáhá řešení a optimalizovat pro úlohy. Databáze pro databázi MySQL Azure poskytuje různé metriky, které poskytují přehled o chování prostředků podporuje MySQL server. 

## <a name="metrics"></a>Metriky
Všechny metriky Azure mají frekvencí jedné minuty a jednotlivé metriky poskytuje 30 dní od historie. 

Nakonfigurujte upozornění na metriky. Podrobné pokyny najdete v tématu [jak nastavit výstrahy](howto-alert-on-metric.md). 

Další úlohy zahrnují nastavení automatizované akce, provádění pokročilou analýzu a archivaci historie. Další informace najdete v tématu [přehled metrik Azure](../monitoring-and-diagnostics/monitoring-overview-metrics.md).

### <a name="list-of-metrics"></a>Seznam metriky
Pro databázi Azure pro databázi MySQL k dispozici jsou tyto metriky:

|Metrika|Metriky zobrazovaný název|Jednotka|Popis|
|---|---|---|---|---|
|cpu_percent|Procento využití procesoru|Procento|Procento procesoru používán.|
|compute_limit|Výpočetní jednotka limit|Počet|Tento server maximální počet výpočetních jednotek|
|compute_consumption_percent|Výpočetní jednotka procento|Procento|Je maximální procento výpočetní jednotky použít mimo server.|
|memory_percent|Paměť v procentech|Procento|Procento paměti.|
|io_consumption_percent|Vstupně-výstupní operace v procentech|Procento|Procento vstupů/výstupů používán.|
|storage_percent|Procento úložiště|Procento|Je maximální procento využití úložiště mimo server.|
|storage_used|Využité úložiště|Bajty|Velikost úložiště, které používá. Úložiště používá služba zahrnuje soubory databáze, transakční protokoly a protokoly serveru.|
|storage_limit|Limit úložiště|Bajty|Maximální velikost úložiště pro tento server.|
|active_connections|Celkový počet aktivních připojení|Počet|Počet aktivních připojení k serveru.|
|connections_failed|Celkový počet selhání připojení|Počet|Počet selhání připojení k serveru.|


> [!NOTE]
> Výpočetní že jednotka se skládá z paměti a procesoru. Procento výpočetní jednotky je maximální počet (% paměti, využití procesoru %). Zkontrolujte grafy paměti a procesoru ke kotvícímu bodu, který je přispívání do výpočetní jednotky procento změny. Další informace najdete v tématu [výpočetní jednotky](concepts-compute-unit-and-storage.md).

## <a name="next-steps"></a>Další kroky
- Podrobné pokyny najdete v tématu [jak nastavit výstrahy](howto-alert-on-metric.md). 
- Další informace o tom, jak přístup a export metriky pomocí portálu Azure, rozhraní REST API nebo rozhraní příkazového řádku najdete v tématu [přehled metrik Azure](../monitoring-and-diagnostics/monitoring-overview-metrics.md).
