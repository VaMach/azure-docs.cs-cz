---
title: "Správa schématu Azure SQL Database v aplikaci s více tenanty | Dokumentace Microsoftu"
description: "Správa schématu pro více tenantů v aplikaci s více tenanty využívající službu Azure SQL Database"
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
ms.date: 07/28/2017
ms.author: billgib; sstein
ms.openlocfilehash: 14912df26074b525585594cc1b5d32c85ce9094f
ms.sourcegitcommit: e5355615d11d69fc8d3101ca97067b3ebb3a45ef
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/31/2017
---
# <a name="manage-schema-for-multiple-tenants-in-a-multi-tenant-application-that-uses-azure-sql-database"></a>Správa schématu pro více tenantů v aplikaci s více tenanty využívající službu Azure SQL Database

[První kurz Wingtip SaaS](sql-database-saas-tutorial.md) ukazuje, jak zřídit databázi klienta a zaregistrovat v katalogu aplikací. Všechny aplikace, jako je aplikace Wingtip SaaS bude v průběhu času vyvíjejí a v některých případech bude vyžadovat změny databáze. Změny se mohou týkat nového nebo změněného schématu, nových nebo změněných referenčních dat a úloh spojených s pravidelnou údržbou databáze, které zajišťují optimální výkon aplikace. U SaaS aplikace musí být tyto změny nasazeny koordinovaně u potenciálně velkého počtu klientských databází. Pro tyto změny se v budoucnu klienta databáze musí být součástí procesu zřizování.

Tento kurz zkoumá dva scénáře: nasazení aktualizací referenčních dat u všech tenantů a opětovné ladění indexu u tabulky, která obsahuje referenční data. [Elastické úlohy](sql-database-elastic-jobs-overview.md) funkce se používá k provedení těchto operací napříč všech klientů a *zlaté* klienta databáze, která se používá jako šablonu pro nové databáze.

V tomto kurzu se naučíte:

> [!div class="checklist"]

> * Vytvoření účtu úlohy
> * Dotazování mezi několik klientů
> * Aktualizovat data ve všech databázích tenantů
> * Vytvořit index tabulky ve všech databázích tenantů


Předpokladem dokončení tohoto kurzu je splnění následujících požadavků:

* Adresář Wingtip SaaS aplikace je nasazená. Nasazení za méně než pět minut najdete v tématu [nasazení a seznamte se s Wingtip SaaS aplikace](sql-database-saas-tutorial.md)
* Je nainstalované prostředí Azure PowerShell. Podrobnosti najdete v článku [Začínáme s prostředím Azure PowerShell](https://docs.microsoft.com/powershell/azure/get-started-azureps).
* Je nainstalovaná nejnovější verze SQL Server Management Studia (SSMS). [Stažení a instalace SSMS](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms)

*Tento kurz používá funkce služby SQL Database, které fungují ve verzi Limited Preview (úlohy služby Elastic Database). Pokud chcete tento kurz absolvovat, pošlete ID svého předplatného na SaaSFeedback@microsoft.com. Jako předmět uveďte Elastic Jobs Preview. Jakmile dostanete potvrzení o aktivaci vašeho předplatného, [stáhněte a nainstalujte si nejnovější předběžnou verzi rutin úloh](https://github.com/jaredmoo/azure-powershell/releases). Tato předběžná verze je omezená, takže obraťte se na SaaSFeedback@microsoft.com pro dotazy související s ani nepodporuje.*


## <a name="introduction-to-saas-schema-management-patterns"></a>Úvod do principu správy schématu SaaS

Princip jednoho tenanta na databázi SaaS má řadu výhod, které vyplývají z výsledné izolace dat, ale současně přináší další komplikace spojené s údržbou a správou mnoha databází. Služba [Elastic Jobs](sql-database-elastic-jobs-overview.md) usnadňuje správu a řízení datové vrstvy SQL. Úlohy umožňují bezpečně a spolehlivě spouštět na skupině databází úkoly (skripty T-SQL), a to nezávisle na interakci nebo vstupech uživatele. Tento způsob je možné použít k nasazení schématu a k běžným změnám referenčních dat u všech tenantů v aplikaci. Službu Elastic Jobs můžete použít k údržbě *zlaté* kopie databáze, která slouží k vytváření nových tenantů, aby se vždy použilo nejnovější schéma a nejnovější referenční data.

![obrazovka](media/sql-database-saas-tutorial-schema-management/schema-management.png)


## <a name="elastic-jobs-limited-preview"></a>Elastic Jobs verze Limited Preview

K dispozici je nová verze služby Elastic Jobs. Jde o integrovanou funkci Azure SQL Database, která nevyžaduje další služby ani součásti. Tato nová verze služby Elastic Jobs je v současnosti ve verzi Limited Preview. Verze Limited Preview v současnosti podporuje prostředí PowerShell, které umožňuje vytvářet účty úloh, a příkazy T-SQL, které umožňují vytvářet a spravovat úlohy.

> [!NOTE]
> *Tento kurz používá funkce služby SQL Database, které fungují ve verzi Limited Preview (úlohy služby Elastic Database). Pokud chcete tento kurz absolvovat, pošlete ID svého předplatného na SaaSFeedback@microsoft.com. Jako předmět uveďte Elastic Jobs Preview. Jakmile dostanete potvrzení o aktivaci vašeho předplatného, [stáhněte a nainstalujte si nejnovější předběžnou verzi rutin úloh](https://github.com/jaredmoo/azure-powershell/releases). Tato předběžná verze je omezená, takže obraťte se na SaaSFeedback@microsoft.com pro dotazy související s ani nepodporuje.*

## <a name="get-the-wingtip-application-scripts"></a>Získání skriptů aplikace Wingtip

Adresář Wingtip SaaS skripty a zdrojový kód aplikace, které jsou k dispozici v [WingtipSaaS](https://github.com/Microsoft/WingtipSaaS) úložiště github. [Postup stažení skripty Wingtip SaaS](sql-database-wtp-overview.md#download-and-unblock-the-wingtip-saas-scripts).

## <a name="create-a-job-account-database-and-new-job-account"></a>Vytvoření databáze účtu úlohy a nového účtu úlohy

Tento kurz vyžaduje, abyste k vytvoření databáze účtu úlohy a účtu úlohy použili PowerShell. Stejně jako MSDB a SQL Agent i služba Elastic Jobs ukládá definice úloh, stavy úloh a historii do Azure SQL Database. Jakmile vytvoříte účet úlohy, můžete hned začít vytvářet a monitorovat úlohy.

1. V **integrovaném skriptovacím prostředí (ISE) v prostředí PowerShell** otevřete skript ...\\Learning Modules\\Schema Management\\*Demo-SchemaManagement.ps1*.
1. Stisknutím klávesy **F5** spusťte skript.

Skript *Demo-SchemaManagement.ps1* volá skript *Deploy-SchemaManagement.ps1*, který na serveru katalogu vytvoří databázi *S2* pojmenovanou **jobaccount**. Tento skript pak vytvoří účet úlohy a předá databázi jobaccount jako parametr volání vytvoření účtu úlohy.

## <a name="create-a-job-to-deploy-new-reference-data-to-all-tenants"></a>Vytvoření úlohy, která u všech tenantů nasadí nová referenční data

Každá databáze tenanta zahrnuje sadu typů míst, které definují druh událostí hostovaných jako místo. V tomto příkladu nasadíte aktualizaci u všech databází tenantů. Aktualizace přidá dva další typy míst: *Motorcycle Racing* (Motocyklové závody) a *Swimming Club* (Plavecký klub). Tyto typy míst odpovídají obrázkům na pozadí zobrazeným v aplikaci událostí tenanta.

Klikněte na rozevírací nabídku Venue Type (Typ místa) a ověřte, že jako typ místa je k dispozici jen 10 možností. Konkrétně ověřte, že v seznamu není „Motorcycle Racing“ ani „Swimming Club“.

Teď vytvoříte úlohu, která aktualizuje tabulku *VenueTypes* ve všech databázích tenantů a přidá do ní nové typy míst.

K vytvoření nové úlohy použijeme sadu uložených systémových procedur úloh vytvořených v databázi jobaccount při vytvoření účtu úlohy.

1. Otevřete SSMS a připojte se k serveru katalogu: server catalog-\<uživatel\>.database.windows.net
1. Připojte se také k serveru tenanta: tenants1-\<uživatel\>.database.windows.net
1. Přejděte k databázi *contosoconcerthall* na serveru *tenants1* a zadejte dotaz do tabulky *VenueTypes*, abyste si potvrdili, že v seznamu výsledků **není** *Motorcycle Racing* ani *Swimming Club*.
1. Otevřete soubor …\\Learning Modules\\Schema Management\\DeployReferenceData.sql
1. Upravit příkaz: nastavte @wtpUser = &lt;uživatele&gt; a nahraďte hodnotu uživatele použít při nasazení aplikace Wingtip
1. Zkontrolujte, že jste připojeni k databázi jobaccount, a stisknutím klávesy **F5** spusťte skript.

* **sp\_add\_target\_group** vytvoří cílovou skupinu s názvem DemoServerGroup. Teď potřebujeme přidat cílové členy.
* **SP\_přidat\_cíl\_skupiny\_člen** přidá *server* cíle typ člena, které považuje za všechny databáze v rámci tohoto serveru (Poznámka: Toto je tenants1-&lt; Uživatel&gt; server obsahující databáze klienta) v čase úlohy spuštění by měl být součástí úlohy, je přidání druhý *databáze* cíle typ člena, konkrétně "zlatá" databáze (basetenantdb) která se nachází v katalogu -&lt;uživatele&gt; serveru a nakonec jiné *databáze* cíle typ člena skupiny zahrnout adhocanalytics databázi, která se používá novější kurzu.
* **sp\_add\_job** vytvoří úlohu s názvem „Reference Data Deployment“.
* **SP\_přidat\_krok úlohy** vytvoří krok úlohy obsahující text příkazů T-SQL aktualizovat na referenční tabulku VenueTypes
* Zbývající pohledy ve skriptu zobrazují existující objekty a monitorují provádění úlohy. Ke kontrole hodnota stavu v použijte tyto dotazy **životního cyklu** sloupec k určení, kdy úloha úspěšně dokončil všechny databáze klienta a dva další databáze, které obsahují na referenční tabulku.

1. V SSMS přejděte k databázi *contosoconcerthall* na serveru *tenants1* a zadejte dotaz do tabulky *VenueTypes*, abyste si potvrdili, že v seznamu výsledků teď **je** *Motorcycle Racing* i *Swimming Club*.


## <a name="create-a-job-to-manage-the-reference-table-index"></a>Vytvoření úlohy pro správu indexu referenční tabulky

Jde o podobné cvičení, jako bylo to předchozí. V tomto cvičení vytvoříme úlohu, která znovu sestaví index primárního klíče referenční tabulky. Jde o typickou operaci při správě databáze, kterou správce většinou provádí po načtení velkého objemu dat do tabulky.

K vytvoření úlohy použijeme stejné úlohy uložených procedur „system“.

1. Otevřete aplikaci SSMS a připojte se k katalogu -&lt;uživatele&gt;. database.windows.net serveru
1. Otevřete soubor …\\Learning Modules\\Schema Management\\OnlineReindex.sql.
1. Klikněte pravým tlačítkem, vyberte připojení a připojení ke katalogu -&lt;uživatele&gt;. database.windows.net serveru, pokud ještě není připojen.
1. Zkontrolujte, že jste připojeni k databázi jobaccount, a stisknutím klávesy F5 spusťte skript.

* sp\_add\_job vytvoří novou úlohu s názvem „Online Reindex PK\_\_VenueTyp\_\_265E44FD7FD4C885“.
* sp\_add\_jobstep vytvoří krok úlohy obsahující text příkazu T-SQL, který aktualizuje index.




## <a name="next-steps"></a>Další kroky

V tomto kurzu jste se naučili:

> [!div class="checklist"]

> * Vytvořit účet úlohy pro dotazy do více tenantů
> * Aktualizovat data ve všech databázích tenantů
> * Vytvořit index tabulky ve všech databázích tenantů

[Kurz náhodné analýzy](sql-database-saas-tutorial-adhoc-analytics.md)


## <a name="additional-resources"></a>Další zdroje

* [Další kurzy, které stavět na adresář Wingtip SaaS nasazení aplikace](sql-database-wtp-overview.md#sql-database-wingtip-saas-tutorials)
* [Správa cloudových databází s horizontálním navýšením kapacity](sql-database-elastic-jobs-overview.md)
* [Vytvoření a správa databází s horizontálním navýšením kapacity](sql-database-elastic-jobs-create-and-manage.md)
