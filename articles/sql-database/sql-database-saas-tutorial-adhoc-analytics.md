---
title: "Spouštění analytických dotazů ad hoc pro více databází SQL Azure | Dokumentace Microsoftu"
description: "Spouštění analytických dotazů ad hoc ve více databázích v aplikaci s více tenanty"
keywords: kurz k sql database
services: sql-database
documentationcenter: 
author: stevestein
manager: jhubbard
editor: 
ms.assetid: 
ms.service: sql-database
ms.custom: tutorial
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 05/10/2017
ms.author: billgib; sstein
ms.translationtype: Human Translation
ms.sourcegitcommit: fc4172b27b93a49c613eb915252895e845b96892
ms.openlocfilehash: dd41e7f1f131f6c18e03d2434982c3d681342b8b
ms.contentlocale: cs-cz
ms.lasthandoff: 05/12/2017


---
# <a name="run-ad-hoc-analytics-queries-across-all-wtp-tenants"></a>Spouštění analytických dotazů ad hoc v tenantech WTP

V tomto kurzu vytvoříte analytickou databázi ad hoc a spustíte několik dotazů ve všech tenantech. Tyto dotazy dokážou extrahovat statistiky ukryté v běžných provozních datech aplikace WTP.

Aplikace WTP používá ke spouštění analytických dotazů ad hoc (ve více tenantech) Spuštění analytics dotazů ad-hoc (mezi několik klientů), aplikace WTP používá nástroj [Elastic Query](sql-database-elastic-query-overview.md) a taky analytickou databázi.


V tomto kurzu se naučíte:

> [!div class="checklist"]

> * Nasazení analytické databáze ad hoc
> * Spouštění distribuovaných dotazů ve všech tenantských databázích



Předpokladem dokončení tohoto kurzu je splnění následujících požadavků:

* Je nasazená aplikace WTP. Informace o nasazení, které netrvá ani pět minut, najdete v článku [Nasazení SaaS aplikace WTP a seznámení s ní](sql-database-saas-tutorial.md).
* Je nainstalované prostředí Azure PowerShell. Podrobnosti najdete v článku [Začínáme s prostředím Azure PowerShell](https://docs.microsoft.com/powershell/azure/get-started-azureps).


## <a name="ad-hoc-analytics-pattern"></a>Vzor analýzy ad hoc

Mezi skvělé příležitosti, které nabízejí aplikace SaaS, patří používání rozsáhlého množství dat tenantů, která jste centrálně uložili do cloudu. Na základě těchto dat můžete získat statistiky provozu a využití vašich aplikací, tenantů, jejich uživatelů a jejich preferencí a chování. Těmito statistikami se potom může řídit vývoj funkcí, vylepšení použitelnosti a další investice do aplikací a služeb.

V jedné databázi s více tenanty je přístup k těmto datům jednoduchý, ale třeba v případě distribuce mezi tisícovky databází se situace komplikuje. Jednou z možností je použít nástroj Elastic Query, který umožňuje dotazování ad hoc v distribuované sadě databází se společným schématem. Elastic Query používá jednu *hlavní* databázi s definovanými externími tabulkami, které zrcadlí tabulky v distribuovaných (tenantských) databázích. Dotazy odeslané do této hlavní databáze se kompilují a vzniká plán distribuovaného dotazu, který zajistí předávání částí dotazu potřebným tenantským databázím. Nástroj Elastic Query na základě mapy horizontálních oddílů v databázi katalogu poskytuje umístění databází tenantů. Vytvoření i dotazování probíhá jednoduše pomocí běžného jazyka T-SQL a podporuje ad hoc dotazování z nástrojů, jako je Power BI a Excel.

## <a name="get-the-wingtip-application-scripts"></a>Získání skriptů aplikace Wingtip

Skripty a zdrojový kód aplikace Wingtip Tickets jsou dostupné v úložišti GitHubu [WingtipSaaS](https://github.com/Microsoft/WingtipSaaS). Soubory se skripty jsou ve [složce Learning Modules](https://github.com/Microsoft/WingtipSaaS/tree/master/Learning%20Modules). Stáhněte si složku **Learning Modules** do místního počítače. Dejte pozor, abyste zachovali strukturu složky.

## <a name="deploy-the-database-used-for-ad-hoc-analytics-queries"></a>Nasazení databáze používané pro analytické dotazy ad hoc

V tomto cvičení nasadíte analytickou databázi ad hoc, která obsahuje schéma používané pro vydávání dotazů ad hoc zahrnujících všechny databáze tenantů.

1. Otevřete skript ...\\Learning Modules\\Operational Analytics\\Adhoc Analytics\\*Demo-AdhocAnalytics.ps1* ve složce *PowerShell ISE* a nastavte následující hodnoty:
   * **$DemoScenario** = 2, **Deploy Ad-hoc analytics database** (Nasadit analytickou databázi ad hoc).

1. Stisknutím klávesy **F5** spusťte skript, vytvořte novou databázi SQL pro analýzu ad hoc a přidejte ji do katalogu WTP. Tabulky TicketPurchases, Tickets a Venues se přidají jako externí tabulky, které umožňují dotazy.
   Nevadí, pokud se vám v této fázi zobrazují upozornění jako *Server RPC není k dispozici*.


Teď máte databázi *adhocanalytics*, kterou můžete použít ke spouštění distribuovaných dotazů a shromažďování statistik ze všech tenantů.

## <a name="run-ad-hoc-analytics-queries"></a>Spouštění analytických dotazů ad hoc

V tomto cvičení spustíte analytické dotazy ad hoc, abyste získali statistiky tenantů z aplikace WTP.

1. Otevřete skript …\\Learning Modules\\Operational Analytics\\Adhoc Analytics\\*Demo-AdhocAnalyticsQueries.sql* v SSMS.
1. Zkontrolujte připojení k databázi **adhocanalytics**.
1. Vyberte konkrétní dotaz, který chcete spustit, a stiskněte klávesu **F5**:

    ![query](media/sql-database-saas-tutorial-adhoc-analytics/query.png)




## <a name="next-steps"></a>Další kroky

V tomto kurzu jste se naučili:

> [!div class="checklist"]

> * Nasazení analytické databáze ad hoc
> * Spouštění distribuovaných dotazů ve všech tenantských databázích

[Kurz Log Analytics (OMS)](sql-database-saas-tutorial-log-analytics.md)

## <a name="additional-resources"></a>Další zdroje

* [Další výukové kurzy, které vycházejí z původně nasazené aplikace Wingtip Tickets Platform (WTP)](sql-database-wtp-overview.md#sql-database-wtp-saas-tutorials)
* [Elastic Query](sql-database-elastic-query-overview.md)

