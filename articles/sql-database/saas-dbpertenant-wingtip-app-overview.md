---
title: "Příklad víceklientského aplikace Azure SQL Database - Wingtip SaaS | Microsoft Docs"
description: "Další informace pomocí víceklientské ukázkovou aplikaci, která používá databázi SQL Azure, například adresář Wingtip SaaS"
keywords: kurz k sql database
services: sql-database
author: stevestein
manager: craigg
ms.service: sql-database
ms.custom: scale out apps
ms.workload: On Demand
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/12/2017
ms.author: sstein
ms.openlocfilehash: ddd51c23c7e7d01e38b02c79c27d1951eea61e70
ms.sourcegitcommit: 732e5df390dea94c363fc99b9d781e64cb75e220
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/14/2017
---
# <a name="introduction-to-a-sql-database-multi-tenant-saas-app-example"></a>Úvod do příklad aplikace SaaS více klientů databáze SQL

*Wingtip SaaS* aplikace je víceklientské ukázkovou aplikaci, která demonstruje jedinečných výhod databáze SQL. Aplikace používá vzorec SaaS aplikace databází na tenanta k poskytování služeb více tenantům. Tato aplikace je určená k prezentují funkce databáze SQL Azure, které umožňují SaaS scénáře, včetně několika vzory návrhu a správu SaaS. Pro rychlé zprovoznění, nasadí aplikaci Wingtip SaaS za méně než pět minut!

Jsou k dispozici v aplikaci zdrojový kód a správu skriptů [WingtipSaaS](https://github.com/Microsoft/WingtipSaaS) úložiště github. Ke spouštění skriptů, [Learning moduly složka pro stahování](#download-and-unblock-the-wingtip-saas-scripts) do místního počítače.

## <a name="application-architecture"></a>Architektura aplikace

Aplikace Wingtip SaaS používá model databáze za klienta a elastické fondy SQL Pokud chcete maximalizovat efektivitu. Pro zřizování a mapování klientům svá data se používá databázi katalogu. Základní aplikace Wingtip SaaS používá fond s tři klienty ukázka plus databáze katalogu. Dokončení řadu SaaS Wingtip kurzy za následek doplňky vyhledá nasazení se zavedením analytické databáze mezidatabázové Správa schématu, atd.


![Architektura Wingtip SaaS](media/saas-dbpertenant-wingtip-app-overview/app-architecture.png)


Při průchodu přes kurzů k a práci s aplikací, je potřeba zaměřit se na vzory SaaS, které se vztahují na datové vrstvě. Jinými slovy je třeba se zaměřit na datovou vrstvu a ne na analýzu aplikace jako takové. Principy provádění těchto SaaS vzory je klíč k implementaci tyto vzory ve svých aplikacích při zvažování veškeré nezbytné úpravy pro vaše konkrétní obchodní požadavky.

## <a name="sql-database-wingtip-saas-tutorials"></a>Kurzy SaaS Wingtip databáze SQL

Po nasazení aplikace, prozkoumejte následující kurzy, které sestavení po počátečním nasazení. Tyto kurzy prozkoumejte běžných vzorů SaaS, které využít integrované funkce SQL Database, SQL Data Warehouse a jinými službami Azure. Kurzy zahrnují skripty prostředí PowerShell, s podrobné vysvětlení, které výrazně zjednodušit pochopení a implementace stejné vzorů správu SaaS ve svých aplikacích.


| Kurz | Popis |
|:--|:--|
| [Pokyny a tipů, například aplikace SaaS víceklientské Azure SQL Database](saas-dbpertenant-wingtip-app-guidance-tips.md) | **ZAČNĚTE ZDE!** Stažení a spuštění skriptů prostředí PowerShell pro přípravu částí aplikace. |
|[Nasazení a prozkoumejte Wingtip SaaS aplikace](saas-dbpertenant-get-started-deploy.md)|  Nasazení a prozkoumejte aplikace Wingtip SaaS k předplatnému Azure. |
|[Zřizování a katalog klientů](saas-dbpertenant-provision-and-catalog.md)| Zjistěte, jak se aplikace připojí klientům pomocí databáze katalogu a jak katalogu mapuje klienty na svá data. |
|[Sledování a správa výkonu](saas-dbpertenant-performance-monitoring.md)| Zjistěte, jak používat monitorování funkcí služby SQL Database a jak nastavit upozornění při překročení prahové hodnoty výkonu. |
|[Monitorování s analýzy protokolů (OMS)](saas-dbpertenant-log-analytics.md) | Další informace o použití [analýzy protokolů](../log-analytics/log-analytics-overview.md) monitorování velké množství prostředků, napříč více fondů. |
|[Obnovení jednoho klienta](saas-dbpertenant-restore-single-tenant.md)| Zjistěte, jak k obnovení databázi klienta do předchozího bodu v čase. Kroky k obnovení databázi paralelní ponechat stávající databázi klienta online, jsou také zahrnuté. |
|[Správa schématu klienta](saas-tenancy-schema-management.md)| Zjistěte, jak aktualizovat schéma a aktualizovat referenční data napříč všichni klienti Wingtip SaaS. |
|[Spustit analytics ad-hoc](saas-tenancy-adhoc-analytics.md) | Vytvoření databáze analýzy ad-hoc a spusťte v reálném čase distribuované dotazy mezi všechny klienty.  |
|[Spusťte klienta analytics](saas-tenancy-tenant-analytics.md) | Extrahování dat klienta do služby analytics databáze nebo data warehouse ke spuštění v režimu offline analytické dotazy. |


## <a name="next-steps"></a>Další kroky

- [Pokyny a tipů, například aplikace SaaS víceklientské Azure SQL Database](saas-dbpertenant-wingtip-app-guidance-tips.md)

- [Nasazení aplikace Wingtip SaaS](saas-dbpertenant-get-started-deploy.md)
