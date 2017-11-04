---
title: "Monitorování úložiště v paměti XTP | Microsoft Docs"
description: "Odhad a monitorování úložiště v paměti XTP používat, kapacity; Vyřešte chyby kapacity 41823"
services: sql-database
documentationcenter: 
author: jodebrui
manager: jhubbard
editor: 
ms.assetid: b617308e-692c-4938-8fa2-070034a3ecef
ms.service: sql-database
ms.custom: monitor & tune
ms.workload: Inactive
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/25/2017
ms.author: jodebrui
ms.openlocfilehash: 613a9ced91d71cc9a65ea67e6ede1a78a03b4bd5
ms.sourcegitcommit: e5355615d11d69fc8d3101ca97067b3ebb3a45ef
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/31/2017
---
# <a name="monitor-in-memory-oltp-storage"></a>Monitorování OLTP v paměti úložiště
Při použití [OLTP v paměti](sql-database-in-memory.md), data v paměťově optimalizované tabulky a proměnných tabulek, které se nachází v úložišti OLTP v paměti. Má maximální velikost úložiště OLTP v paměti, která je popsána v jednotlivých úrovních služby Premium [jedna databáze prostředků omezení](sql-database-resource-limits.md#single-database-storage-sizes-and-performance-levels) a [limitů elastického fondu prostředků](sql-database-resource-limits.md#elastic-pool-change-storage-size). Po překročení tohoto limitu vložení a aktualizace operace může spustit (došlo k chybě 41823). V tomto okamžiku budete potřebovat buď odstranit data získat paměť, nebo upgradujte úroveň výkonu databáze.

## <a name="determine-whether-data-will-fit-within-the-in-memory-storage-cap"></a>Určení, zda budou data nevejdou do limitu úložiště v paměti
Určete úložiště CAP o různých úrovních služeb Premium. V tématu [jedna databáze prostředků omezení](sql-database-resource-limits.md#single-database-storage-sizes-and-performance-levels) a [limitů elastického fondu prostředků](sql-database-resource-limits.md#elastic-pool-change-storage-size).

Odhadnout požadavky na paměť pro paměťově optimalizované tabulky funguje pro SQL Server stejným způsobem jako se nepodporuje v Azure SQL Database. Trvat několik minut, přečtěte si toto téma na [MSDN](https://msdn.microsoft.com/library/dn282389.aspx).

Všimněte si, že tabulky a proměnné řádky tabulky, protože jako indexy, započítávat velikost dat max uživatele. Příkaz ALTER TABLE navíc vyžaduje dostatek místa k vytvoření nové verze celou tabulku a jeho indexů.

## <a name="monitoring-and-alerting"></a>Monitorování a upozorňování
Můžete sledovat využití úložiště v paměti jako procentní podíl limitu úložiště pro vaše úroveň výkonu v [portál Azure](https://portal.azure.com/): 

1. V okně databáze vyhledejte pole využití prostředků a klikněte na Upravit.
2. Vyberte metriku `In-Memory OLTP Storage percentage`.
3. Přidání oznámení, klikněte na pole využití prostředků a otevřete okno metriky a potom klikněte na Přidat upozornění.

Nebo použijte následující dotaz pro zobrazení využití úložiště v paměti:

    SELECT xtp_storage_percent FROM sys.dm_db_resource_stats


## <a name="correct-out-of-memory-situations---error-41823"></a>Opravte z důvodu nedostatku paměti situacích - chyba 41823
V operacích INSERT, UPDATE a vytvořit selhání s chybovou zprávou 41823 spuštěna výsledky z důvodu nedostatku paměti.

Chybová zpráva 41823 označuje, že paměťově optimalizované tabulky a proměnných tabulek, které překročily maximální velikost.

Chcete tuto chybu vyřešit buď:

* Odstranění dat z paměťově optimalizované tabulky potenciálně snižování zátěže dat do tabulky tradiční, založené na disku; nebo,
* Upgradujte úroveň služby s dostatečně velké úložiště v paměti pro data, která je potřeba udržovat v paměťově optimalizovaných tabulkách.

## <a name="next-steps"></a>Další kroky
Monitorování na pokyny najdete v části [monitorování databáze Azure SQL pomocí zobrazení dynamické správy](sql-database-monitoring-with-dmvs.md).
