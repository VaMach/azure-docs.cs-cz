---
title: "Jak funguje distribuovaných datech v Azure SQL Data Warehouse | Microsoft Docs"
description: "Zjistěte, jak distribuci dat pro masivně paralelní zpracování MPP a možností distribuce tabulek v Azure SQL Data Warehouse a Parallel Data Warehouse."
services: sql-data-warehouse
documentationcenter: NA
author: jrowlandjones
manager: jhubbard
editor: 
ms.assetid: bae494a6-7ac5-4c38-8ca3-ab2696c63a9f
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: tables
ms.date: 07/12/2017
ms.author: jrj;barbkess
ms.openlocfilehash: 3c166acb17193caae32d7bad133ec510ff679353
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="distributed-data-and-distributed-tables-for-massively-parallel-processing-mpp"></a>Rozložení dat a distribuované tabulky pro masivně paralelní zpracování MPP)
Zjistěte, jak distribuci dat uživatele v Azure SQL Data Warehouse a Parallel Data Warehouse, které jsou systémy masivně paralelní zpracování (MPP) společnosti Microsoft. Navrhování váš datový sklad efektivně používat distribuovaných datech umožňuje dosáhnout zpracování výhody architektury MPP dotazů. Několik možností návrhu databáze může mít významný dopad na zlepšení výkonu dotazů.  

> [!NOTE]
> Azure SQL Data Warehouse a Parallel Data Warehouse použít stejné návrh masivně paralelní zpracování MPP (), ale mají několik rozdílů kvůli základní platformy. SQL Data Warehouse je platforma jako služba (PaaS), která běží na Azure. Parallel Data Warehouse se spustí na Analytics Platform System (AP), což je místní zařízení, která běží na systému Windows Server.
> 
> 

## <a name="what-is-distributed-data"></a>Co je rozložení dat?
V SQL Data Warehouse a Parallel Data Warehouse distribuované data odkazují na uživatelská data, která je uložená v několika umístěních napříč MPP systému. Každé z těchto umístění funguje jako nezávislé úložiště a výpočetní jednotky používající dotazy na jeho část data. Rozložení dat je nezbytné, aby spuštění dotazů paralelně dosažení vysokého výkonu dotazu.

K distribuci dat, datový sklad přiřadí jeden distribuované umístění každý řádek tabulky uživatele.  Můžete distribuovat tabulky s způsobu distribuce hash nebo metodu kruhového dotazování. Metoda distribuce je zadaný v příkazu CREATE TABLE. 

## <a name="hash-distributed-tables"></a>Distribuovat algoritmu hash tabulky
Následující diagram znázorňuje, jak získá úplnou (tabulka distribuované) uložené jako tabulku distribuovat algoritmu hash. Deterministické funkce přiřadí každý řádek patřit do jednoho distribučního. V definici tabulky jednoho ze sloupců je určený jako distribuční sloupce. Funkce hash použije hodnotu ve sloupci distribuční přiřadit distribuční každý řádek.

Existují důležité informace o výkonu pro výběr distribuční sloupce, například typy dotazů odlišnosti, zkosení dat a spustit v systému.

![Tabulka distribuované](media/sql-data-warehouse-distributed-data/hash-distributed-table.png "distribuované tabulky")  

* Každý řádek patří do jednoho distribučního.  
* Algoritmus hash deterministický přiřadí do jednoho distribučního každý řádek.  
* Počet řádků tabulky na distribuční se liší, jak je znázorněno pomocí různých velikostí tabulek.

## <a name="round-robin-distributed-tables"></a>Distribuované tabulky pomocí kruhového dotazování
Distribuované tabulku kruhového dotazování distribuuje řádky přiřazením každý řádek distribuční sekvenční způsobem. Je rychlý načíst data do tabulky pomocí kruhového dotazování, ale může být pomalejší výkon dotazů.  Připojení pomocí kruhového dotazování tabulky obvykle vyžaduje promísení řádky, které povolit dotaz k vytvoření přesné výsledky, který přebírá čas.

## <a name="distributed-storage-locations-are-called-distributions"></a>Umístění distribuované úložiště se nazývají distribuce
Každé distribuované umístění se nazývá distribuce. Když dotazu běží paralelně, každý distribuční provede dotaz SQL na jeho část data. SQL Data Warehouse používá ke spuštění dotazu SQL Database. Parallel Data Warehouse používá SQL Server spusťte dotaz. Tento návrh architektury shared nothing, je nezbytné k dosažení škálování paralelní výpočty.

### <a name="can-i-view-the-distributions"></a>Zobrazení distribucí
Každý distribuční má ID distribuce a je viditelný v zobrazeních systému, které se vztahují k SQL Data Warehouse a Parallel Data Warehouse. Distribuce ID můžete použít k řešení potíží s výkon dotazů a jiné problémy. Seznam zobrazení systému najdete v tématu [MPP systémové zobrazení](sql-data-warehouse-reference-tsql-statements.md).

## <a name="difference-between-a-distribution-and-a-compute-node"></a>Rozdíl mezi distribučním a výpočetního uzlu
Distribuce je základní jednotkou pro ukládání dat v distribuované a zpracování paralelní dotazy. Distribuce se seskupují do výpočetních uzlů. Každý výpočetní uzel sleduje jeden nebo více distribuce.  

* Analytics Platform System používá jako centrální součást hardwarových možnostech architektura a škálování výpočetních uzlů. Vždy používá osm distribuce na výpočetním uzlu, jak je vidět v diagramu pro tabulky distribuovat algoritmu hash. Počet výpočetních uzlů a proto počet distribuce, je dáno počet výpočetních uzlů, které jste si koupili pro zařízení. Například pokud si koupíte osm výpočetní uzly, získáte 64 distribuce (8 výpočetní uzly x 8 distribuce/uzel). 
* SQL Data Warehouse má pevný počet 60 distribuce a flexibilní počet výpočetních uzlů. Výpočetní uzly jsou implementovány pomocí Azure výpočetních a paměťových prostředků. Počet výpočetních uzlů, můžete změnit podle zatížení služby back-end a výpočetní kapacity (Dwu), můžete zadat pro datový sklad. Když se změní počet výpočetních uzlů, změní se také počet distribuce na výpočetním uzlu. 

### <a name="can-i-view-the-compute-nodes"></a>Můžete zobrazit výpočetní uzly?
Každý výpočetní uzel obsahuje ID uzlu a je viditelný v zobrazeních systému, které se vztahují k SQL Data Warehouse a Parallel Data Warehouse.  Uvidíte výpočetního uzlu tak, že vyhledá node_id sloupec v systémových zobrazeních, jejichž názvy začínají řetězcem sys.pdw_nodes. Seznam zobrazení systému najdete v tématu [MPP systémové zobrazení](sql-data-warehouse-reference-tsql-statements.md).

## <a name="Replicated"></a>Replikované tabulky
Obsahuje tabulku, která se replikují plně kopie tabulky uložena na každém výpočetním uzlu. Replikace tabulku eliminuje nutnost k přenosu dat mezi výpočetní uzly před spojení nebo agregace. Replikované tabulky jsou vhodná s malé tabulky pouze z důvodu dodatečné úložiště potřebné pro uložení plném tabulka na každém výpočetním uzlu.  

Následující diagram znázorňuje replikované tabulky, který je uložený na každém výpočetním uzlu. Pro SQL Data Warehouse replikované tabulce zkopírován plně distribuční databázi na každém výpočetním uzlu. Parallel Data Warehouse replikované tabulce ukládají na všechny disky přiřazené k výpočetním uzlu.  Tato strategie disku je implementována pomocí skupin souborů systému SQL Server.  

![Replikované tabulky](media/sql-data-warehouse-distributed-data/replicated-table.png "replikované tabulky") 

## <a name="next-steps"></a>Další kroky
Pomocí distribuovaných tabulky efektivně naleznete v části [distribuci tabulek v SQL Data Warehouse](sql-data-warehouse-tables-distribute.md)  

