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
ms.date: 11/14/2017
ms.author: billgib
ms.openlocfilehash: 346177be29ec196464f4f441858222ac5d5eb8c3
ms.sourcegitcommit: 9a61faf3463003375a53279e3adce241b5700879
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/15/2017
---
# <a name="manage-schema-for-multiple-tenants-in-a-multi-tenant-application-that-uses-azure-sql-database"></a>Správa schématu pro více klientů ve víceklientské aplikaci, která používá databázi SQL Azure

[První adresář Wingtip lístky SaaS víceklientské databázový kurz](saas-multitenantdb-get-started-deploy.md) ukazuje, jak zřídit horizontálně dělené víceklientské databázi a zaregistrovat v katalogu aplikací. Podobně jako všechny aplikace aplikace SaaS lístky Wingtip bude v průběhu času vyvíjejí a v některých případech bude vyžadovat změny databáze. Změny se mohou týkat nového nebo změněného schématu, nových nebo změněných referenčních dat a úloh spojených s pravidelnou údržbou databáze, které zajišťují optimální výkon aplikace. U SaaS aplikace musí být tyto změny nasazeny koordinovaně u potenciálně velkého počtu klientských databází. Pro tyto změny se v budoucnu klienta databáze musí být součástí procesu zřizování.

Tento kurz zkoumá dva scénáře: nasazení aktualizací referenčních dat u všech tenantů a opětovné ladění indexu u tabulky, která obsahuje referenční data. [Elastické úlohy](sql-database-elastic-jobs-overview.md) funkce se používá k provedení těchto operací napříč klienta databáze a *zlaté* klienta databáze, která se používá jako šablonu pro nové databáze.

V tomto kurzu se naučíte:

> [!div class="checklist"]

> * Vytvoření účtu úlohy
> * Dotazování mezi několik klientů
> * Aktualizovat data ve všech databázích tenantů
> * Vytvořit index tabulky ve všech databázích tenantů


Předpokladem dokončení tohoto kurzu je splnění následujících požadavků:

* Nasazení aplikace Wingtip lístky SaaS víceklientské databáze. Nasazení za méně než pět minut najdete v tématu [nasazení a seznamte se s aplikací Wingtip lístky SaaS víceklientské databáze](saas-multitenantdb-get-started-deploy.md)
* Je nainstalované prostředí Azure PowerShell. Podrobnosti najdete v článku [Začínáme s prostředím Azure PowerShell](https://docs.microsoft.com/powershell/azure/get-started-azureps).
* Je nainstalovaná nejnovější verze SQL Server Management Studia (SSMS). [Stažení a instalace SSMS](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms)

> [!NOTE]
> *Tento kurz používá funkce služby SQL Database, které fungují ve verzi Limited Preview (úlohy služby Elastic Database). Pokud chcete tento kurz absolvovat, pošlete ID svého předplatného na SaaSFeedback@microsoft.com. Jako předmět uveďte Elastic Jobs Preview. Jakmile dostanete potvrzení o aktivaci vašeho předplatného, [stáhněte a nainstalujte si nejnovější předběžnou verzi rutin úloh](https://github.com/jaredmoo/azure-powershell/releases). Tato předběžná verze je omezená, takže obraťte se na SaaSFeedback@microsoft.com pro dotazy související s ani nepodporuje.*


## <a name="introduction-to-saas-schema-management-patterns"></a>Úvod do principu správy schématu SaaS

Model databáze. horizontálně dělené víceklientské použít v této ukázce umožňuje klientům databáze do obsahovat libovolný počet klientů. Tato ukázka prozkoumá potenciálně použít kombinaci více klientů a jednoho klienta databáze, povolení model správy klienta "hybridní". Údržba a správa těchto databází je složitá. Služba [Elastic Jobs](sql-database-elastic-jobs-overview.md) usnadňuje správu a řízení datové vrstvy SQL. Úlohy umožňují bezpečně a spolehlivě spouštět na skupině databází úkoly (skripty T-SQL), a to nezávisle na interakci nebo vstupech uživatele. Tento způsob je možné použít k nasazení schématu a k běžným změnám referenčních dat u všech tenantů v aplikaci. Službu Elastic Jobs můžete použít k údržbě *zlaté* kopie databáze, která slouží k vytváření nových tenantů, aby se vždy použilo nejnovější schéma a nejnovější referenční data.

![obrazovka](media/saas-multitenantdb-schema-management/schema-management.png)

## <a name="elastic-jobs-limited-preview"></a>Elastic Jobs verze Limited Preview

K dispozici je nová verze služby Elastic Jobs. Jde o integrovanou funkci Azure SQL Database, která nevyžaduje další služby ani součásti. Tato nová verze služby Elastic Jobs je v současnosti ve verzi Limited Preview. Verze Limited Preview v současnosti podporuje prostředí PowerShell, které umožňuje vytvářet účty úloh, a příkazy T-SQL, které umožňují vytvářet a spravovat úlohy.

## <a name="get-the-wingtip-tickets-saas-multi-tenant-database-application-scripts"></a>Získat adresář Wingtip lístky SaaS víceklientské databázové skripty aplikace

Adresář Wingtip lístky SaaS víceklientské databázové skripty a zdrojový kód aplikace, které jsou k dispozici v [WingtipTicketsSaaS MultiTenantDB](https://github.com/Microsoft/WingtipTicketsSaaS-MultiTenantDB) úložiště GitHub. <!-- [Steps to download the Wingtip Tickets SaaS Multi-tenant Database scripts](saas-multitenantdb-wingtip-app-guidance-tips.md#download-and-unblock-the-wingtip-saas-scripts)-->

## <a name="create-a-job-account-database-and-new-job-account"></a>Vytvoření databáze účtu úlohy a nového účtu úlohy

Tento kurz vyžaduje, abyste k vytvoření databáze účtu úlohy a účtu úlohy použili PowerShell. Stejně jako MSDB a SQL Agent i služba Elastic Jobs ukládá definice úloh, stavy úloh a historii do Azure SQL Database. Jakmile vytvoříte účet úlohy, můžete hned začít vytvářet a monitorovat úlohy.

1. V **prostředí PowerShell ISE**, otevřete *... \\Učení moduly\\Správa schématu\\ukázku SchemaManagement.ps1*.
1. Stisknutím klávesy **F5** spusťte skript.

Skript *Demo-SchemaManagement.ps1* volá skript *Deploy-SchemaManagement.ps1*, který na serveru katalogu vytvoří databázi *S2* pojmenovanou **jobaccount**. Tento skript pak vytvoří účet úlohy a předá databázi jobaccount jako parametr volání vytvoření účtu úlohy.

## <a name="create-a-job-to-deploy-new-reference-data-to-all-tenants"></a>Vytvoření úlohy, která u všech tenantů nasadí nová referenční data

Každá databáze klientů zahrnuje sadu typy místo v tabulce **VenueTypes** , definování typu události, které jsou hostované na místo. V tomto cvičení nasazujete aktualizaci nasadit do všech databází. Chcete-li přidat dva typy další místo: *Motorky Racing* a *křížovou kartou plaveckých*. Tyto typy míst odpovídají obrázkům na pozadí zobrazeným v aplikaci událostí tenanta.

Klikněte na rozevírací nabídku Venue Type (Typ místa) a ověřte, že jako typ místa je k dispozici jen 10 možností. Konkrétně ověřte, že v seznamu není „Motorcycle Racing“ ani „Swimming Club“.

Teď vytvoříme úlohu, která aktualizuje **VenueTypes** tabulky v nastavení databází klientů a přidejte nové typy místo.

K vytvoření nové úlohy použijeme sadu uložených systémových procedur úloh vytvořených v databázi jobaccount při vytvoření účtu úlohy.

1. V aplikaci SSMS, připojení k serveru klienta: tenants1-mt -\<uživatele\>. database.windows.net
2. Přejděte do *tenants1* databáze na *tenants1-mt -\<uživatele\>. database.windows.net* serveru a dotaz *VenueTypes* a Potvrďte, že *Motorky Racing* a *křížovou kartou plaveckých* jsou **není** v seznamu výsledků.
3. Připojení k serveru katalogu: katalogu-mt -\<uživatele\>. database.windows.net
4. Připojení k databázi jobaccount v katalogu serveru.
5. V aplikaci SSMS otevřete soubor... \\Učení moduly\\Správa schématu\\DeployReferenceData.sql
6. Upravit příkaz: nastavte @User = &lt;uživatele&gt; a nahraďte hodnotu uživatele použít při nasazení aplikace Wingtip lístky SaaS víceklientské databáze.
7. Stisknutím klávesy **F5** spusťte skript.

    * **SP\_přidat\_cíl\_skupiny** vytvoří cílová skupina název DemoServerGroup, nyní přidat cíl členy do skupiny.
    * **SP\_přidat\_cíl\_skupiny\_člen** přidá *server* cíle typ člena, které považuje za všechny databáze v rámci tohoto serveru (Poznámka: Toto je tenants1 - mt – &lt;uživatele&gt; server obsahující databáze klientů) v době provádění úlohy, které mají být zahrnuty do úlohy *databáze* cíle typ člena pro "zlatá" databázi (basetenantdb), který se nachází na katalog-mt -&lt;uživatele&gt; serveru a nakonec *databáze* cíle typ člena zahrnout adhocreporting databázi, která se používá novější kurzu.
    * **SP\_přidat\_úlohy** vytvoří úlohu názvem "Nasazení referenční Data".
    * **SP\_přidat\_krok úlohy** vytvoří krok úlohy obsahující text příkazů T-SQL aktualizovat na referenční tabulku VenueTypes.
    * Zbývající pohledy ve skriptu zobrazují existující objekty a monitorují provádění úlohy. Ke kontrole hodnota stavu v použijte tyto dotazy **životního cyklu** sloupec k určení, kdy úloha úspěšně dokončil na klienty databáze a dva další databáze, které obsahují na referenční tabulku.

1. V aplikaci SSMS, přejděte do databáze klienta na *tenants1-mt -&lt;uživatele&gt;*  serveru a dotaz *VenueTypes* tabulka, která se potvrďte, že *Motorky Racing* a *křížovou kartou plaveckých* jsou nyní **přidat* do tabulky.


## <a name="create-a-job-to-manage-the-reference-table-index"></a>Vytvoření úlohy pro správu indexu referenční tabulky

Jde o podobné cvičení, jako bylo to předchozí. V tomto cvičení vytvoříme úlohu, která znovu sestaví index primárního klíče referenční tabulky. Jde o typickou operaci při správě databáze, kterou správce většinou provádí po načtení velkého objemu dat do tabulky.


1. V aplikaci SSMS, připojení k databázi jobaccount v katalogu-mt -&lt;uživatele&gt;. database.windows.net serveru.
2. V aplikaci SSMS otevřete... \\Učení moduly\\Správa schématu\\OnlineReindex.sql.
3. Stiskněte klávesu **F5** pro spuštění skriptu

    * **SP\_přidat\_úlohy** vytvoří novou úlohu názvem "Online nové indexování Primárníklíč\_\_VenueTyp\_\_265E44FD7FD4C885".
    * **SP\_přidat\_krok úlohy** vytvoří krok úlohy obsahující text T-SQL příkaz k aktualizaci indexu.
    * Zbývající zobrazení ve skriptu monitorování provádění úlohy. Použít tyto dotazy ke kontrole hodnota stavu v **životního cyklu** k určení, když úloha úspěšně dokončil na na všechny členy skupiny cílový sloupec.

## <a name="next-steps"></a>Další kroky

V tomto kurzu jste se naučili:

> [!div class="checklist"]

> * Vytvořit účet úlohy pro dotazy do více tenantů
> * Aktualizovat data ve všech databázích tenantů
> * Vytvořit index tabulky ve všech databázích tenantů

[Kurz náhodné analýzy](saas-multitenantdb-adhoc-reporting.md)


## <a name="additional-resources"></a>Další zdroje

<!--* Additional tutorials that build upon the Wingtip Tickets SaaS Multi-tenant Database application deployment (*Tutorial link to come*)
(saas-multitenantdb-wingtip-app-overview.md#sql-database-wingtip-saas-tutorials)-->
* [Správa cloudových databází s horizontálním navýšením kapacity](sql-database-elastic-jobs-overview.md)
* [Vytvoření a správa databází s horizontálním navýšením kapacity](sql-database-elastic-jobs-create-and-manage.md)
