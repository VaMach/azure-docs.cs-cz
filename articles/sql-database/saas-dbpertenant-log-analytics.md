---
title: "Použití Log Analytics s aplikací s více tenanty využívající službu SQL Database | Dokumentace Microsoftu"
description: "Nastavit a používat analýzy protokolů (OMS) víceklientské aplikace SaaS databáze SQL Azure"
keywords: kurz k sql database
services: sql-database
documentationcenter: 
author: stevestein
manager: craigg
editor: 
ms.assetid: 
ms.service: sql-database
ms.custom: scale out apps
ms.workload: Inactive
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/13/2017
ms.author: billgib; sstein
ms.openlocfilehash: 48e8eb91a5febcc1109bee3404bb534bd0391f88
ms.sourcegitcommit: f847fcbf7f89405c1e2d327702cbd3f2399c4bc2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/28/2017
---
# <a name="set-up-and-use-log-analytics-oms-with-a-multi-tenant-azure-sql-database-saas-app"></a>Nastavení a použití analýzy protokolů (OMS) víceklientské aplikace SaaS databáze SQL Azure

V tomto kurzu, nastavení a použití *analýzy protokolů ([OMS](https://www.microsoft.com/cloud-platform/operations-management-suite))* pro monitorování elastické fondy a databází. V tomto kurzu vychází [sledování výkonu a správy kurzu](saas-dbpertenant-performance-monitoring.md). Ukazuje, jak používat *analýzy protokolů* k posílení monitorování a generování výstrah v portálu Azure. Analýzy protokolů je vhodná pro monitorování a výstrahy ve velkém měřítku, protože podporuje stovky fondy a stovky tisíc databáze. Poskytuje také jedno řešení pro monitorování, které může integrovat monitorování různých aplikací a služeb Azure napříč několika předplatnými Azure.

Co se v tomto kurzu naučíte:

> [!div class="checklist"]
> * Instalace a konfigurace Log Analytics (OMS)
> * Monitorování fondů a databází pomocí Log Analytics

Předpokladem dokončení tohoto kurzu je splnění následujících požadavků:

* Nasazení aplikace Wingtip lístky SaaS databáze za klienta. Nasazení za méně než pět minut najdete v tématu [nasazení a seznamte se s aplikací Wingtip lístky SaaS databáze za klienta](saas-dbpertenant-get-started-deploy.md)
* Je nainstalované prostředí Azure PowerShell. Podrobnosti najdete v článku [Začínáme s prostředím Azure PowerShell](https://docs.microsoft.com/powershell/azure/get-started-azureps).

Diskusi ke scénářům a vzorcům SaaS a způsob, jakým ovlivňují požadavky na řešení pro monitorování, najdete v [kurzu Monitorování a správa výkonu](saas-dbpertenant-performance-monitoring.md).

## <a name="monitoring-and-managing-performance-with-log-analytics-oms"></a>Monitorování a správa výkonu pomocí Log Analytics (OMS)

Ve službě SQL Database je monitorování a upozorňování k dispozici v databázích a fondech. Toto integrované monitorování a upozorňování je specifické podle prostředků a je vhodné pro malý počet prostředků, ale méně vhodné pro monitorování velkých instalací nebo poskytnutí sjednoceného pohledu na různé prostředky a předplatná.

Log Analytics je možné používat u velkoobjemových scénářů. To je samostatná služba Azure, která poskytuje analýzy přes vydávané diagnostické protokoly a telemetrii získanou v pracovním prostoru Log Analytics, které můžou shromažďovat telemetrii z mnoha služeb a dají se používat k dotazování a nastavování výstrah. Log Analytics poskytuje integrovaný jazyk dotazů a nástroje vizualizace dat umožňující analýzy a vizualizaci provozních dat. Řešení SQL analýzy poskytuje několik předem definovaných elastický fond a monitorování a výstrah, zobrazení a dotazy databáze a umožňuje přidávat své vlastní dotazy ad hoc a uložíte podle potřeby. OMS poskytuje také vlastní návrhář zobrazení.

Pracovní prostory a analytická řešení Log Analytics se dají otevírat z portálu Azure Portal i v OMS. Azure Portal je novější přístupový bod, ale v některých oblastech může být za portálem OMS.

### <a name="create-data-by-starting-the-load-generator"></a>Vytvořit data spuštěním generátor zatížení 

1. V **prostředí PowerShell ISE**, otevřete **ukázku PerformanceMonitoringAndManagement.ps1**. Tento skript nechte spuštěný, protože je možné, že během tohoto kurzu budete spouštět několik scénářů generování zatížení.
1. Pokud máte méně než pět klientů, monitorování zřídit dávky klientům poskytnout dalších zajímavých kontextu:
   1. Nastavte **$DemoScenario = 1,** **Zřízení dávky tenantů**
   1. Chcete-li spustit skript, stiskněte **F5**.

1. Nastavit **$DemoScenario** = 2, **generování normální intenzitou zatížení (asi 40 DTU)**.
1. Chcete-li spustit skript, stiskněte **F5**.

## <a name="get-the-wingtip-tickets-saas-database-per-tenant-application-scripts"></a>Získat aplikační skripty Wingtip lístky SaaS databáze za klienta

Adresář Wingtip lístky SaaS víceklientské databázové skripty a zdrojový kód aplikace, které jsou k dispozici v [WingtipTicketsSaaS DbPerTenant](https://github.com/Microsoft/WingtipTicketsSaaS-DbPerTenant) úložiště GitHub. Podívejte se [obecné pokyny](saas-tenancy-wingtip-app-guidance-tips.md) kroky, jak stáhnout a odblokování skripty Wingtip lístky SaaS.

## <a name="installing-and-configuring-log-analytics-and-the-azure-sql-analytics-solution"></a>Instalace a konfigurace Log Analytics a řešení Azure SQL Analytics

Log Analytics je samostatná služba, která vyžaduje konfiguraci. Log Analytics shromažďuje data a telemetrii protokolů a metriku v pracovním prostoru Log Analytics. Pracovní prostor je prostředek stejně jako ostatní prostředky v Azure a je potřeba ho vytvořit. I když pracovní prostor nemusí být vytvořený ve stejné skupině prostředků jako aplikace, které monitoruje, dává to často největší smysl. Pro aplikace SaaS databáze za lístky Wingtip klienta SaaS to umožňuje snadno odstranit s aplikací odstraněním skupiny prostředků v pracovním prostoru.

1. V **prostředí PowerShell ISE**, otevřete... \\Učení moduly\\sledování výkonu a správy\\protokolu analýzy\\*ukázku LogAnalytics.ps1*.
1. Chcete-li spustit skript, stiskněte **F5**.

V tomto okamžiku byste měli mít open Log Analytics v portálu Azure (nebo na portálu OMS). Jak dlouho trvá několik minut, než telemetrie, které se mají shromažďovat v pracovním prostoru analýzy protokolů a pak bude viditelný. Čím delší necháte systému shromažďování dat je více zajímavé zkušenosti. Teď si můžete dát chvilku pauzu – jen se ujistěte, že je generátor pořád spuštěný!


## <a name="use-log-analytics-and-the-sql-analytics-solution-to-monitor-pools-and-databases"></a>Monitorování fondů a databází pomocí Log Analytics a řešení SQL Analytics


V tomto cvičení otevřete analýzy protokolů a podívejte se na telemetrie shromážděných pro databáze a fondy na portálu OMS.

1. Přejděte na portál [Azure Portal](https://portal.azure.com) a otevřete Log Analytics kliknutím na Další služby. Potom vyhledejte Log Analytics:

   ![otevření log analytics](media/saas-dbpertenant-log-analytics/log-analytics-open.png)

1. Vyberte pracovní prostor s názvem *wtploganalytics-&lt;USER&gt;*.

1. Vyberte **Přehled** k otevření řešení Log Analytics na portálu Azure Portal.
   ![overview-link](media/saas-dbpertenant-log-analytics/click-overview.png)

    > [!IMPORTANT]
    > Může trvat několik minut, než je aktivní řešení. Buďte prosím trpěliví.

1. Kliknutím otevřete dlaždici Azure SQL Analytics.

    ![overview](media/saas-dbpertenant-log-analytics/overview.png)

    ![analytics](media/saas-dbpertenant-log-analytics/analytics.png)

1. Zobrazení v okně řešení se prochází po straně a má vlastní posuvník v dolní části (pokud je to potřeba, aktualizujte okno).

1. Prozkoumejte různá zobrazení, a to tak, že na ně budete klikat nebo že budete klikat na jednotlivé prostředky. Otevře se průzkumník pro procházení k podrobnostem, kde se můžete pomocí časového posuvníku vlevo nahoře nebo kliknutím na svislý pruh zaměřit na užší časový interval. V tomto zobrazení můžete vybírat jednotlivé databáze nebo fondy pro zaměření na konkrétní prostředky:

    ![graf](media/saas-dbpertenant-log-analytics/chart.png)

1. Pokud budete zpět v okně řešení a přejdete zcela doprava, najdete některé uložené dotazy, které můžete kliknutím otevřít a prozkoumat. Můžete vyzkoušet tyto úpravy a uložit všechny zajímavé dotazy, které můžete vytvořit, které budete moct znovu otevřít a použít s další prostředky.

1. Až se vrátíte do okna pracovního prostoru Log Analytics, vyberte Portál OMS a otevřete řešení tady.

    ![oms](media/saas-dbpertenant-log-analytics/oms.png)

1. Na portálu OMS můžete konfigurovat výstrahy. Klikněte na část výstrahy v zobrazení DTU databáze.

1. V zobrazení prohledávání protokolu uvidíte sloupcový graf představované metriky.

    ![prohledávání protokolů](media/saas-dbpertenant-log-analytics/log-search.png)

1. Pokud kliknete na panelu nástrojů na upozornění, bude moci zobrazit konfiguraci výstrah a můžete ho změnit.

    ![přidání pravidla výstrahy](media/saas-dbpertenant-log-analytics/add-alert.png)

Monitorování a upozorňování v Log Analytics a OMS je založené na dotazech na data v pracovním prostoru, na rozdíl od výstrah v oknech jednotlivých prostředků, které jsou specifické podle prostředků. Proto můžete definovat výstrahu, která dohlíží na všechny databáze místo definování samostatné výstrahy pro každou databázi. Nebo můžete zapsat výstrahu, která používá složený dotaz přes více typů prostředků. Dotazy jsou omezené jenom daty dostupnými v pracovním prostoru.

Log Analytics pro SQL Database se účtuje podle objemu dat v pracovním prostoru. V tomto kurzu jste vytvořili bezplatný pracovní prostor omezený na 500 MB na den. Po dosažení tohoto limitu se data přestanou přidávat do pracovního prostoru.


## <a name="next-steps"></a>Další kroky

V tomto kurzu jste se naučili:

> [!div class="checklist"]
> * Instalace a konfigurace Log Analytics (OMS)
> * Monitorování fondů a databází pomocí Log Analytics

[Kurz Analýza tenanta](saas-dbpertenant-log-analytics.md)

## <a name="additional-resources"></a>Další zdroje

* [Další kurzy, které sestavení po počátečním nasazení aplikace Wingtip lístky SaaS databáze za klienta](saas-dbpertenant-wingtip-app-overview.md#sql-database-wingtip-saas-tutorials)
* [Azure Log Analytics](../log-analytics/log-analytics-azure-sql.md)
* [OMS](https://blogs.technet.microsoft.com/msoms/2017/02/21/azure-sql-analytics-solution-public-preview/)
