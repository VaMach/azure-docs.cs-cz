---
title: "Spouštění analytických dotazů v několika tenantech (ukázková SaaS aplikace využívající Azure SQL Database) | Dokumentace Microsoftu"
description: "Spouštění analytických dotazů v několika tenantech"
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
ms.openlocfilehash: b512e2f7833be1947ef7674d6e0266879789ac5a
ms.contentlocale: cs-cz
ms.lasthandoff: 05/10/2017


---
# <a name="run-analytics-queries-against-multiple-tenants"></a>Spouštění analytických dotazů v několika tenantech

V tomto kurzu budete spouštět analytické dotazy u všech tenantů v katalogu. Vytvoří se elastická úloha, která spouští dotazy. Úloha načítá data a nahraje je do samostatné analytické databáze vytvořené na katalogovém serveru. Tato databáze může být dotazována k extrahování přehledů, které jsou skryté v každodenních provozních datech všech tenantů. Výstupem úlohy je tabulka vytvořená z dotazů, které vracejí výsledky v rámci analytické databáze tenantů.


Co se v tomto kurzu naučíte:

> [!div class="checklist"]
> * Vytvoření analytické databáze tenantů
> * Vytvoření plánované úlohy k načtení dat a naplnění analytické databáze

Předpokladem dokončení tohoto kurzu je splnění následujících požadavků:

* Je nasazená aplikace WTP. Informace o nasazení, které netrvá ani pět minut, najdete v článku [Nasazení SaaS aplikace WTP a seznámení s ní](sql-database-saas-tutorial.md).
* Prostředí Azure PowerShell je nainstalované. Podrobnosti najdete v článku [Začínáme s prostředím Azure PowerShell](https://docs.microsoft.com/powershell/azure/get-started-azureps).
* Je nainstalovaná nejnovější verze SQL Server Management Studia (SSMS). [Stažení a instalace SSMS](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms)

## <a name="tenant-operational-analytics-pattern"></a>Vzor provozní analýzy tenanta

Jednou ze skvělých příležitostí, které nabízejí aplikace SaaS, je používání rozsáhlého množství dat tenantů, která jsou uložená v cloudu. Tato data je možné využívat k získání přehledu o provozu a používání vaší aplikace a tenanta. Můžou řídit vývoj funkcí, vylepšení použitelnosti a další investice do aplikace a platformy. V jedné databázi s více tenanty je přístup k těmto datům jednoduchý, ale třeba v případě distribuce mezi tisícovky databází se situace komplikuje. Jedním z přístupů k těmto datům je použití elastických úloh, které umožňují zaznamenat výsledky dotazů vracejících výsledky z provedení úlohy do výstupní databáze a tabulky.

## <a name="get-the-wingtip-application-scripts"></a>Získání skriptů aplikace Wingtip

Skripty a zdrojový kód aplikace Wingtip Tickets jsou k dispozici v úložišti GitHubu [WingtipSaaS](https://github.com/Microsoft/WingtipSaaS). Soubory se skripty jsou ve [složce Learning Modules](https://github.com/Microsoft/WingtipSaaS/tree/master/Learning%20Modules). Stáhněte si složku **Learning Modules** do svého místního počítače. Dejte pozor, abyste zachovali strukturu složky.

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

1. Otevřete SSMS a připojte se k serveru catalog-\<user\>.database.windows.net.
1. Otevřete složku ...\\Learning Modules\\Operational Analytics\\Tenant Analytics\\*TicketPurchasesfromAllTenants.sql*
1. Změňte parametr \<WtpUser\> a použijte uživatelské jméno, které jste použili při nasazení aplikace WTP v horní části skriptu, **sp\_add\_target\_group\_member** a **sp\_add\_jobstep**
1. Klikněte pravým tlačítkem, vyberte **Connection** (Připojení) a připojte se k serveru catalog-\<WtpUser\>.database.windows.net, pokud jste se k němu ještě nepřipojili.
1. Zkontrolujte, že jste připojení k databázi **jobaccount**, a stisknutím klávesy **F5** spusťte skript.

* **sp\_add\_target\_group** vytvoří cílovou skupinu s názvem *TenantGroup*. Teď potřebujeme přidat cílové členy.
* **sp\_add\_target\_group\_member** přidá typ člena *server*, který předpokládá, že všechny databáze na daném serveru (server customer1-&lt;WtpUser&gt; obsahující databáze tenantů) budou v okamžiku spuštění úlohy do této úlohy zahrnuté.
* **sp\_add\_job** vytvoří novou týdně plánovanou úlohu nazvanou Nákup lístků ze všech tenantů.
* **sp\_add\_jobstep** vytvoří krok úlohy, který obsahuje text příkazu T-SQL k načtení všech informací o nákupu lístků ze všech tenantů, a zkopíruje výslednou sadu výsledků do tabulky nazvané *AllTicketsPurchasesfromAllTenants*
* Zbývající pohledy ve skriptu zobrazují existující objekty a monitorují provádění úlohy. Zkontrolujte stavovou hodnotu ze sloupce **lifecycle**, která vám umožní monitorovat stav. V případě úspěchu je úloha zdárně dokončena na všech databázích tenantů i na dvou dalších databázích obsahujících referenční tabulku.

Úspěšné spuštění skriptu by mělo vrátit podobné výsledky:

![výsledky](media/sql-database-saas-tutorial-tenant-analytics/ticket-purchases-job.png)

## <a name="create-a-job-to-retrieve-a-summary-count-of-ticket-purchases-from-all-tenants"></a>Vytvoření úlohy k načtení souhrnného počtu nákupů lístků ze všech tenantů

Tento skript vytvoří úlohu k načtení souhrnu všech nákupů lístků ze všech tenantů.

1. Otevřete SSMS a připojte se k serveru*catalog-&lt;User&gt;.database.windows.net*.
1. Otevřete skript …\\Learning Modules\\Provision and Catalog\\Operational Analytics\\Tenant Analytics\\*Results-TicketPurchasesfromAllTenants.sql*
1. Změňte parametr &lt;WtpUser&gt; a použijte uživatelské jméno, které jste použili při nasazení aplikace WTP ve skriptu v uložené proceduře **sp\_add\_jobstep**
1. Klikněte pravým tlačítkem, vyberte **Connection** (Připojení) a připojte se k serveru catalog-\<WtpUser\>.database.windows.net, pokud jste se k němu ještě nepřipojili.
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

* [Další kurzy, které vycházejí z původně nasazené aplikace WTP (Wingtip Tickets Platform)](sql-database-wtp-overview.md#sql-database-wtp-saas-tutorials)
* [Elastické úlohy](sql-database-elastic-jobs-overview.md)
