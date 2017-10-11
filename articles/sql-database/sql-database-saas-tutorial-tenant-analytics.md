---
title: "Spouštění analytických dotazů pro více databází SQL Azure | Dokumentace Microsoftu"
description: "Extrahovat data z databáze klienta do databáze analýzy pro offline analýzu"
keywords: kurz k sql database
services: sql-database
documentationcenter: 
author: stevestein
manager: jhubbard
editor: 
ms.assetid: 
ms.service: sql-database
ms.custom: scale out apps
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/16/2017
ms.author: billgib; sstein
ms.openlocfilehash: 4e32407d5f321198358e07980907c3420aaf56c6
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="extract-data-from-tenant-databases-into-an-analytics-database-for-offline-analysis"></a>Extrahovat data z databáze klienta do databáze analýzy pro offline analýzu

V tomto kurzu použijete elastické úlohy ke spouštění dotazů na každou databázi klienta. Úloha extrahuje data prodeje lístků a načte ji do databáze analýzy (nebo datového skladu) pro analýzu. Databáze analýzy je pak dotazován extrahovat statistiky z této každodenní provozních dat všech klientů.


V tomto kurzu se naučíte:

> [!div class="checklist"]
> * Vytvoření analytické databáze tenantů
> * Vytvoření plánované úlohy k načtení dat a naplnění analytické databáze

Předpokladem dokončení tohoto kurzu je splnění následujících požadavků:

* Adresář Wingtip SaaS aplikace je nasazená. Nasazení za méně než pět minut najdete v tématu [nasazení a seznamte se s Wingtip SaaS aplikace](sql-database-saas-tutorial.md)
* Je nainstalované prostředí Azure PowerShell. Podrobnosti najdete v článku [Začínáme s prostředím Azure PowerShell](https://docs.microsoft.com/powershell/azure/get-started-azureps).
* Je nainstalovaná nejnovější verze SQL Server Management Studia (SSMS). [Stažení a instalace SSMS](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms)

## <a name="tenant-operational-analytics-pattern"></a>Vzor provozní analýzy tenanta

Jednou ze skvělých příležitostí, které nabízejí aplikace SaaS, je používání rozsáhlého množství dat tenantů, která jsou uložená v cloudu. Tato data je možné využívat k získání přehledu o provozu a používání vaší aplikace a tenanta. Můžou řídit vývoj funkcí, vylepšení použitelnosti a další investice do aplikace a platformy. V jedné databázi s více tenanty je přístup k těmto datům jednoduchý, ale třeba v případě distribuce mezi tisícovky databází se situace komplikuje. Jedním z přístupů k těmto datům je použití elastických úloh, které umožňují zaznamenat výsledky dotazů vracejících výsledky z provedení úlohy do výstupní databáze a tabulky.

## <a name="get-the-wingtip-application-scripts"></a>Získání skriptů aplikace Wingtip

Adresář Wingtip SaaS skripty a zdrojový kód aplikace, které jsou k dispozici v [WingtipSaaS](https://github.com/Microsoft/WingtipSaaS) úložiště github. [Postup stažení skripty Wingtip SaaS](sql-database-wtp-overview.md#download-and-unblock-the-wingtip-saas-scripts).

## <a name="deploy-a-database-for-tenant-analytics-results"></a>Nasazení databáze pro výsledky analýzy tenanta

Tento kurz vyžaduje, abyste měli nasazenou databázi, do které se budou zaznamenávat výsledky z provedení úlohy se skripty, která obsahuje dotazy vracející výsledky. Pro tento účel si vytvoříme databázi nazvanou tenantanalytics.

1. Otevřete skript …\\Learning Modules\\Operational Analytics\\Tenant Analytics\\*Demo-TenantAnalyticsDB.ps1* ve složce *PowerShell ISE* a nastavte následující hodnotu:
   * **$DemoScenario** = **2** *Nasazení provozní analytické databáze* 
1. Stiskněte klávesu **F5** ke spuštění ukázkového skriptu (který volá skript *Deploy-TenantAnalyticsDB.ps1*), který vytvoří databázi analýzy tenanta.

## <a name="create-some-data-for-the-demo"></a>Vytvoření dat pro ukázku

1. Otevřete skript …\\Learning Modules\\Operational Analytics\\Tenant Analytics\\*Demo-TenantAnalyticsDB.ps1* ve složce *PowerShell ISE* a nastavte následující hodnotu:
   * **$DemoScenario** = **1** *Nákup lístků na akce na všech místech*
1. Stiskněte klávesu **F5** ke spuštění skriptu a vytvoření historie nákupu lístků.


## <a name="create-a-scheduled-job-to-retrieve-tenant-analytics-about-ticket-purchases"></a>Vytvoření plánované úlohy k načtení analýzy tenanta k nákupu lístků

Tento skript vytvoří úlohu k načtení informací o nákupu lístků ze všech tenantů. Po shrnutí do jedné tabulky můžete získat bohatou informační metriku o vzorcích nákupu lístků napříč tenanty.

1. Otevřete SSMS a připojte se k serveru catalog-&lt;user&gt;.database.windows.net.
1. Otevřete složku ...\\Learning Modules\\Operational Analytics\\Tenant Analytics\\*TicketPurchasesfromAllTenants.sql*
1. Upravit &lt;uživatele&gt;, použít uživatelské jméno použít při nasazení aplikace Wingtip SaaS v horní části na skript, **sp\_přidat\_cíl\_skupiny\_člen** a **sp\_přidat\_krok úlohy**
1. Klikněte pravým tlačítkem, vyberte **připojení**a připojte se k katalogu -&lt;uživatele&gt;. database.windows.net serveru, pokud ještě není připojen.
1. Zkontrolujte, že jste připojení k databázi **jobaccount**, a stisknutím klávesy **F5** spusťte skript.

* **sp\_add\_target\_group** vytvoří cílovou skupinu s názvem *TenantGroup*. Teď potřebujeme přidat cílové členy.
* **SP\_přidat\_cíl\_skupiny\_člen** přidá *server* cíle typ člena, které považuje za všechny databáze v rámci tohoto serveru (Poznámka: Toto je customer1-&lt; Uživatel&gt; server obsahující databáze klienta) v čase úlohy by měl být součástí provádění úlohy.
* **sp\_add\_job** vytvoří novou týdně plánovanou úlohu nazvanou Nákup lístků ze všech tenantů.
* **sp\_add\_jobstep** vytvoří krok úlohy, který obsahuje text příkazu T-SQL k načtení všech informací o nákupu lístků ze všech tenantů, a zkopíruje výslednou sadu výsledků do tabulky nazvané *AllTicketsPurchasesfromAllTenants*
* Zbývající pohledy ve skriptu zobrazují existující objekty a monitorují provádění úlohy. Zkontrolujte stavovou hodnotu ze sloupce **lifecycle**, která vám umožní monitorovat stav. V případě úspěchu je úloha zdárně dokončena na všech databázích tenantů i na dvou dalších databázích obsahujících referenční tabulku.

Úspěšné spuštění skriptu by mělo vrátit podobné výsledky:

![výsledky](media/sql-database-saas-tutorial-tenant-analytics/ticket-purchases-job.png)

## <a name="create-a-job-to-retrieve-a-summary-count-of-ticket-purchases-from-all-tenants"></a>Vytvoření úlohy k načtení souhrnného počtu nákupů lístků ze všech tenantů

Tento skript vytvoří úlohu k načtení souhrnu všech nákupů lístků ze všech tenantů.

1. Otevřete SSMS a připojte se k serveru*catalog-&lt;User&gt;.database.windows.net*.
1. Otevřete skript …\\Learning Modules\\Provision and Catalog\\Operational Analytics\\Tenant Analytics\\*Results-TicketPurchasesfromAllTenants.sql*
1. Upravit &lt;uživatele&gt;, použít uživatelské jméno použít při nasazení aplikace Wingtip SaaS ve skriptu, v **sp\_přidat\_krok úlohy** uložené procedury
1. Klikněte pravým tlačítkem, vyberte **připojení**a připojte se k katalogu -&lt;uživatele&gt;. database.windows.net serveru, pokud ještě není připojen.
1. Zkontrolujte, že jste připojení k databázi **tenantanalytics**, a stisknutím klávesy **F5** spusťte skript.

Úspěšné spuštění skriptu by mělo vrátit podobné výsledky:

![výsledky](media/sql-database-saas-tutorial-tenant-analytics/total-sales.png)



* **sp\_add\_job** vytvoří novou týdně plánovanou úlohu nazvanou ResultsTicketsOrders

* **sp\_add\_jobstep** vytvoří krok úlohy, který obsahuje text příkazu T-SQL k načtení všech informací o nákupu lístků ze všech tenantů, a zkopíruje výslednou sadu výsledků do tabulky nazvané CountofTicketOrders

* Zbývající pohledy ve skriptu zobrazují existující objekty a monitorují provádění úlohy. Zkontrolujte stavovou hodnotu ze sloupce **lifecycle**, která vám umožní monitorovat stav. V případě úspěchu je úloha zdárně dokončena na všech databázích tenantů i na dvou dalších databázích obsahujících referenční tabulku.


## <a name="next-steps"></a>Další kroky

V tomto kurzu jste se naučili:

> [!div class="checklist"]
> * Nasazení analytické databáze tenantů
> * Vytvoření plánované úlohy k načtení analytických dat mezi tenanty

Blahopřejeme!

## <a name="additional-resources"></a>Další zdroje

* Další [návodů, které stavět na adresář Wingtip SaaS aplikace](sql-database-wtp-overview.md#sql-database-wingtip-saas-tutorials)
* [Elastické úlohy](sql-database-elastic-jobs-overview.md)
