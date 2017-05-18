---
title: "Nastavení a spuštění analýzy protokolu (ukázková SaaS aplikace využívající Azure SQL Database) | Dokumentace Microsoftu"
description: "Nastavení a používání Log Analytics s ukázkovou SaaS aplikací WTP"
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
ms.sourcegitcommit: 71fea4a41b2e3a60f2f610609a14372e678b7ec4
ms.openlocfilehash: 7cb9b7dd90123a91cabe66fd8efa8ae4c9e2fa01
ms.contentlocale: cs-cz
ms.lasthandoff: 05/10/2017


---
# <a name="setup-and-use-log-analytics-oms-with-the-wtp-sample-saas-app"></a>Nastavení a používání Log Analytics (OMS) s ukázkovou SaaS aplikací WTP

V tomto kurzu nastavujete a používáte *Log Analytics([OMS](https://www.microsoft.com/cloud-platform/operations-management-suite))* s aplikací WTP ke sledování elastických fondů a databází. Je nástavbou kurzu [Monitorování a správa výkonu](sql-database-saas-tutorial-performance-monitoring.md) a uvádí, jak se používá *Log Analytics* k posílení monitorování a upozorňování poskytovaného na portálu Azure Portal. Služba Log Analytics je zvlášť vhodná pro monitorování a upozorňování v různém rozsahu, protože podporuje stovky fondů a stovky tisíc databází. Poskytuje také jedno řešení pro monitorování, které může integrovat monitorování různých aplikací a služeb Azure napříč několika předplatnými Azure.

Co se v tomto kurzu naučíte:

> [!div class="checklist"]
> * Instalace a konfigurace Log Analytics (OMS)
> * Monitorování fondů a databází pomocí Log Analytics

Předpokladem dokončení tohoto kurzu je splnění následujících požadavků:

* Je nasazená aplikace WTP. Informace o nasazení, které netrvá ani pět minut, najdete v článku [Nasazení SaaS aplikace WTP a seznámení s ní](sql-database-saas-tutorial.md).
* Prostředí Azure PowerShell je nainstalované. Podrobnosti najdete v článku [Začínáme s prostředím Azure PowerShell](https://docs.microsoft.com/powershell/azure/get-started-azureps).

Diskusi ke scénářům a vzorcům SaaS a způsob, jakým ovlivňují požadavky na řešení pro monitorování, najdete v [kurzu Monitorování a správa výkonu](sql-database-saas-tutorial-performance-monitoring.md).

## <a name="monitoring-and-managing-performance-with-log-analytics-oms"></a>Monitorování a správa výkonu pomocí Log Analytics (OMS)

Ve službě SQL Database je monitorování a upozorňování k dispozici v databázích a fondech. Toto integrované monitorování a upozorňování je specifické podle prostředků a je vhodné pro malý počet prostředků, ale méně vhodné pro monitorování velkých instalací nebo poskytnutí sjednoceného pohledu na různé prostředky a předplatná.

Log Analytics je možné používat u velkoobjemových scénářů. To je samostatná služba Azure, která poskytuje analýzy přes vydávané diagnostické protokoly a telemetrii získanou v pracovním prostoru Log Analytics, které můžou shromažďovat telemetrii z mnoha služeb a dají se používat k dotazování a nastavování výstrah. Log Analytics poskytuje integrovaný jazyk dotazů a nástroje vizualizace dat umožňující analýzy a vizualizaci provozních dat. Řešení SQL Analytics nabízí několik předdefinovaných elastických fondů a zobrazení a dotazů na monitorování a upozorňování databáze a umožňuje přidávat vlastní ad-hoc dotazy a podle potřeby je ukládat. OMS poskytuje také vlastní návrhář zobrazení.

Pracovní prostory a analytická řešení Log Analytics se dají otevírat z portálu Azure Portal i v OMS. Azure Portal je novější přístupový bod, ale v některých oblastech může být za portálem OMS.

### <a name="start-the-load-generator-to-create-data-to-analyze"></a>Spuštění generátoru zatížení pro vytváření dat k analýze

1. Spusťte skript **Demo-PerformanceMonitoringAndManagement.ps1** ve složce **PowerShell ISE**. Tento skript nechte spuštěný, protože je možné, že během tohoto kurzu budete spouštět několik scénářů generování zatížení.
1. Pokud používáte méně než pět tenantů, zřiďte dávku tenantů pro zajištění zajímavějšího kontextu monitorování:
   1. Nastavte **$DemoScenario = 1,** **Zřízení dávky tenantů**
   1. Stisknutím klávesy **F5** spusťte skript.

1. Nastavte **$DemoScenario** = 2, **Generování normální intenzity zatížení (asi 40 DTU)**.
1. Stisknutím klávesy **F5** spusťte skript.

## <a name="get-the-wingtip-application-scripts"></a>Získání skriptů aplikace Wingtip

Skripty a zdrojový kód aplikace Wingtip Tickets jsou k dispozici v úložišti GitHubu [WingtipSaaS](https://github.com/Microsoft/WingtipSaaS). Soubory se skripty jsou ve [složce Learning Modules](https://github.com/Microsoft/WingtipSaaS/tree/master/Learning%20Modules). Stáhněte si složku **Learning Modules** do svého místního počítače. Dejte pozor, abyste zachovali strukturu složky.

## <a name="installing-and-configuring-log-analytics-and-the-azure-sql-analytics-solution"></a>Instalace a konfigurace Log Analytics a řešení Azure SQL Analytics

Log Analytics je samostatná služba, která vyžaduje konfiguraci. Log Analytics shromažďuje data a telemetrii protokolů a metriku v pracovním prostoru Log Analytics. Pracovní prostor je prostředek stejně jako ostatní prostředky v Azure a je potřeba ho vytvořit. I když pracovní prostor nemusí být vytvořený ve stejné skupině prostředků jako aplikace, které monitoruje, dává to často největší smysl. V případě aplikace WTP to umožňuje snadné odstranění pracovního prostoru s aplikací pouhým odstraněním skupiny prostředků.

1. Otevřete ...\\Learning Modules\\Performance Monitoring and Management\\Log Analytics\\*Demo-LogAnalytics.ps1* v **Integrovaném skriptovacím prostředí (ISE) v prostředí PowerShell**.
1. Stisknutím klávesy **F5** spusťte skript.

Teď by mělo být možné otevřít Log Analytics na portálu Azure Portal (nebo portálu OMS). Shromáždění telemetrie a její zviditelnění v pracovním prostoru Log Analytics bude trvat pár minut. Čím déle necháte systém shromažďovat data, tím zajímavější bude práce s nimi. Teď si můžete dát chvilku pauzu – jen se ujistěte, že je generátor pořád spuštěný!


## <a name="use-log-analytics-and-the-sql-analytics-solution-to-monitor-pools-and-databases"></a>Monitorování fondů a databází pomocí Log Analytics a řešení SQL Analytics


V tomto cvičení otevřete Log Analytics a portál OMS, abyste se mohli podívat na telemetrii shromažďovanou pro databáze a fondy WTP.

1. Přejděte na portál [Azure Portal](https://portal.azure.com) a otevřete Log Analytics kliknutím na Další služby. Potom vyhledejte Log Analytics:

   ![otevření log analytics](media/sql-database-saas-tutorial-log-analytics/log-analytics-open.png)

1. Vyberte pracovní prostor s názvem *wtploganalytics-&lt;USER&gt;*.

1. Vyberte **Přehled** k otevření řešení Log Analytics na portálu Azure Portal.
   ![overview-link](media/sql-database-saas-tutorial-log-analytics/click-overview.png)

    **DŮLEŽITÉ**: Může to několik minut trvat, než začne být řešení aktivní. Buďte prosím trpěliví.

1. Kliknutím otevřete dlaždici Azure SQL Analytics.

    ![overview](media/sql-database-saas-tutorial-log-analytics/overview.png)

    ![analytics](media/sql-database-saas-tutorial-log-analytics/analytics.png)

1. Zobrazení v okně řešení se prochází po straně a má vlastní posuvník v dolní části (pokud je to potřeba, aktualizujte okno).

1. Prozkoumejte různá zobrazení, a to tak, že na ně budete klikat nebo že budete klikat na jednotlivé prostředky. Otevře se průzkumník pro procházení k podrobnostem, kde se můžete pomocí časového posuvníku vlevo nahoře nebo kliknutím na svislý pruh zaměřit na užší časový interval. V tomto zobrazení můžete vybírat jednotlivé databáze nebo fondy pro zaměření na konkrétní prostředky:

    ![graf](media/sql-database-saas-tutorial-log-analytics/chart.png)

1. Pokud budete zpět v okně řešení a přejdete zcela doprava, najdete některé uložené dotazy, které můžete kliknutím otevřít a prozkoumat. Můžete je zkoušet měnit a můžete si uložit některé zajímavé dotazy, které vytvoříte, a později je znovu otevřít a používat s jinými prostředky.

1. Až se vrátíte do okna pracovního prostoru Log Analytics, vyberte Portál OMS a otevřete řešení tady.

    ![oms](media/sql-database-saas-tutorial-log-analytics/oms.png)

1. Na portálu OMS můžete konfigurovat výstrahy. Klikněte na část výstrahy v zobrazení DTU databáze.

1. V zobrazení prohledávání protokolu uvidíte sloupcový graf představované metriky.

    ![prohledávání protokolů](media/sql-database-saas-tutorial-log-analytics/log-search.png)

1. Pokud kliknete na výstrahu na panelu nástrojů, zobrazí se konfigurace výstrahy, kterou můžete změnit.

    ![přidání pravidla výstrahy](media/sql-database-saas-tutorial-log-analytics/add-alert.png)

Monitorování a upozorňování v Log Analytics a OMS je založené na dotazech na data v pracovním prostoru, na rozdíl od výstrah v oknech jednotlivých prostředků, které jsou specifické podle prostředků. Proto můžete definovat výstrahu, která dohlíží na všechny databáze místo definování samostatné výstrahy pro každou databázi. Nebo můžete zapsat výstrahu, která používá složený dotaz přes více typů prostředků. Dotazy jsou omezené jenom daty dostupnými v pracovním prostoru.

Log Analytics pro SQL Database se účtuje podle objemu dat v pracovním prostoru. V tomto kurzu jste vytvořili bezplatný pracovní prostor omezený na 500 MB na den. Po dosažení tohoto limitu se data přestanou přidávat do pracovního prostoru.


## <a name="next-steps"></a>Další kroky

V tomto kurzu jste se naučili:

> [!div class="checklist"]
> * Instalace a konfigurace Log Analytics (OMS)
> * Monitorování fondů a databází pomocí Log Analytics

[Kurz Analýza tenanta](sql-database-saas-tutorial-tenant-analytics.md)

## <a name="additional-resources"></a>Další zdroje

* [Další kurzy, které vycházejí z původně nasazené aplikace WTP (Wingtip Tickets Platform)](sql-database-wtp-overview.md#sql-database-wtp-saas-tutorials)
* [Azure Log Analytics](../log-analytics/log-analytics-azure-sql.md)
* [OMS](https://blogs.technet.microsoft.com/msoms/2017/02/21/azure-sql-analytics-solution-public-preview/)

