---
title: "Azure SQL Database – automatické ladění | Microsoft Docs"
description: "Azure SQL Database analyzuje dotazu SQL a automaticky přizpůsobí zatížení uživatele."
services: sql-database
documentationcenter: 
author: jovanpop-msft
manager: jhubbard
editor: 
ms.assetid: 
ms.service: sql-database
ms.custom: monitor & tune
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: On Demand
ms.date: 09/19/2017
ms.author: jovanpop
ms.openlocfilehash: 10f8cca8e519b28f0fe29605419b860f73e04a87
ms.sourcegitcommit: e5355615d11d69fc8d3101ca97067b3ebb3a45ef
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/31/2017
---
# <a name="automatic-tuning-in-azure-sql-database"></a>Automatické ladění ve službě Azure SQL Database

Databáze SQL Azure je, že data plně spravovaná služba, která monitoruje dotazy, které jsou spouštěny na databázi a automaticky zvyšuje výkon zatížení databáze. Azure SQL Database má integrované [automatické ladění](https://docs.microsoft.com/sql/relational-databases/automatic-tuning/automatic-tuning) intelligence mechanismus, který můžete automaticky ladit a dynamicky přizpůsobení databázi do úlohy vylepšit výkon vašich dotazů. Automatické ladění ve službě Azure SQL Database, může být jedním z nejdůležitějších funkcích, můžete povolit na Azure SQL Database za účelem optimalizace výkonu dotazů.

> [!VIDEO https://channel9.msdn.com/Blogs/Azure-in-the-Enterprise/Enabling-Azure-SQL-Database-Auto-Tuning-at-Scale-for-Microsoft-IT/player]
>

## <a name="automatic-tuning-options"></a>Automatické možnosti ladění

[Automatické ladění](https://docs.microsoft.com/sql/relational-databases/automatic-tuning/automatic-tuning) jsou možnosti, které jsou k dispozici ve službě Azure SQL Database:
 1. **Vytvoření indexu** identifikující indexy, které může zvýšit výkon vašich úloh, vytváří indexy a ověřuje, že se zlepšit výkon dotazů.
 2. **DROP INDEX** identifikující redundantní a duplicitní indexy a indexy, které nebyly použity v dlouhé časové období.
 3. **Oprava REGRESE plán** , identifikuje dotazy SQL, které používají plán spuštění, které jsou nižší než předchozí kvalitní plán a místo regressed plánu používá poslední známé funkční plánu.

Azure SQL Database identifikuje **CREATE INDEX**, **DROP INDEX**, a **plánování oprava REGRESE** doporučení, které můžete optimalizovat vaši databázi a zobrazí je v Azure portál. Další informace o identifikaci indexy, které by mělo být změněno na [najít doporučení indexu na portálu Azure](sql-database-advisor-portal.md). Můžete buď použít ručně doporučení pomocí portálu nebo můžete nechat Azure SQL Database automaticky používat doporučení, sledování úloh po provedení změny a ověřte, že doporučení zvýšený výkon vašich úloh.

Automatické ladění možnosti může být nezávisle zapnout nebo vypnout na databázi, nebo můžou být nakonfigurované na logickém serveru a použít na každou databázi, která dědí nastavení ze serveru. Konfigurace [automatické ladění](https://docs.microsoft.com/sql/relational-databases/automatic-tuning/automatic-tuning) možnosti na serveru a dědí nastavení u databází na serveru je doporučená metoda pro konfiguraci automatické ladění, protože se zjednodušuje Správa automatické možnosti ladění na velký počet databází.

Najdete v tomto článku postup [povolit automatické ladění](sql-database-automatic-tuning-enable.md) pomocí portálu Azure.

## <a name="next-steps"></a>Další kroky

- Povolit automatické ladění ve službě Azure SQL Database a nechat automatické ladění funkce plně spravovat vaše úlohy najdete v tématu [povolit automatické ladění](sql-database-automatic-tuning-enable.md).
- Chcete-li použít ruční ladění, můžete zjistit [ladění doporučení na portálu Azure](sql-database-advisor-portal.md) a ty, které zlepšit výkon vašich dotazů použít ručně.
- Další informace o vestavěné inteligentní, který vyladí [Azure SQL Database](https://azure.microsoft.com/blog/artificial-intelligence-tunes-azure-sql-databases/).
- Další informace o [automatické ladění](https://docs.microsoft.com/sql/relational-databases/automatic-tuning/automatic-tuning) v Azure SQL Database a SQL Server 2017.
