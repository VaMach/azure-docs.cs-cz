---
title: "Správa schématu Azure SQL Database v aplikaci s více tenanty | Dokumentace Microsoftu"
description: "Správa schématu pro více tenantů v aplikaci s více tenanty využívající službu Azure SQL Database"
keywords: kurz k sql database
services: sql-database
documentationcenter: 
author: MightyPen
manager: craigg
editor: 
ms.service: sql-database
ms.custom: scale out apps
ms.workload: Inactive
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/03/2018
ms.reviewers: billgib
ms.author: genemi
ms.openlocfilehash: 135764a7d89dcf711ff7fe9416850f1af9329479
ms.sourcegitcommit: df4ddc55b42b593f165d56531f591fdb1e689686
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/04/2018
---
# <a name="manage-schema-for-multiple-tenants-in-a-multi-tenant-application-that-uses-azure-sql-database"></a>Správa schématu pro více klientů ve víceklientské aplikaci, která používá databázi SQL Azure

V tomto kurzu prověří výzvy zachování potenciálně masivní firemního vozového databází v softwaru jako služby (SaaS) aplikaci v cloudu. Řešení je ukázán pro správu rozšíření schématu, která jsou definovat a provádět během doby platnosti aplikace.

Změny při zpracovaní jakékoli aplikace, může dojít k jeho sloupců tabulky nebo jiné schéma, nebo jeho referenční data nebo výkon související položky. Aplikaci SaaS musí být nasazený koordinovaným způsobem tyto změny mezi mnoha databázemi stávajícího klienta. A tyto změny musí být obsaženy v databázích budoucí klienta, které budou přidány do aplikace. Proto změny také musí být součástí procesu, který poskytuje nové databáze.

#### <a name="two-scenarios"></a>Dva scénáře

V tomto kurzu zde popsány v následujících dvou scénářích:
- Nasaďte aktualizace dat odkaz pro všechny klienty.
- Retuning indexu pro tabulku, která obsahuje referenční data.

[Elastické úlohy](sql-database-elastic-jobs-overview.md) funkce databáze SQL Azure se používá k provedení těchto operací mezi databázemi klienta. Úlohy také pracovat se zlaté šablony klienta databází. Tato šablona se používá při zřizování nových databází.

V tomto kurzu se naučíte:

> [!div class="checklist"]
> * Vytvořte účet úlohy.
> * Dotazování mezi několik klientů.
> * Aktualizujte data v všechny databáze klienta.
> * Vytvoření indexu pro tabulku v všechny databáze klienta.

## <a name="prerequisites"></a>Požadavky

- Aplikace Wingtip lístky musí nasazené:
    - Pokyny najdete v tématu první kurz, který představuje *Wingtip lístky* databáze víceklientské aplikace SaaS:<br />[Nasazení a prozkoumejte horizontálně dělené víceklientské aplikace, která používá Azure SQL Database](saas-multitenantdb-get-started-deploy.md).
        - Spuštění procesu nasazení pro méně než pět minut.
    - Musíte mít *horizontálně dělené víceklientské* verzi Wingtip nainstalována. Verze pro *samostatné* a *databáze za klienta* nepodporují tohoto kurzu.

- Musí být nainstalována nejnovější verze systému SQL Server Management Studio (SSMS). [Stažení a instalace aplikace SSMS](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms).

- Musí být nainstalovaný Azure PowerShell. Podrobnosti najdete v tématu [Začínáme s Azure Powershellem](https://docs.microsoft.com/powershell/azure/get-started-azureps).

> [!NOTE]
> Tento kurz používá funkce služby Azure SQL Database, které jsou v omezené preview ([úlohy elastické databáze](sql-database-elastic-database-client-library.md)). Pokud chcete provést v tomto kurzu, zadejte svoje ID předplatného pro  *SaaSFeedback@microsoft.com*  s předmětem = elastické úlohy Preview. Jakmile dostanete potvrzení o aktivaci vašeho předplatného, [stáhněte a nainstalujte si nejnovější předběžnou verzi rutin úloh](https://github.com/jaredmoo/azure-powershell/releases). Tato předběžná verze je omezená, takže obraťte se na  *SaaSFeedback@microsoft.com*  pro dotazy související s ani nepodporuje.

## <a name="introduction-to-saas-schema-management-patterns"></a>Úvod k modelům správy schématu SaaS

Model databáze. horizontálně dělené víceklientské použít v této ukázce povoluje klientům databázi tak, aby obsahovala jeden nebo více klientů. Tato ukázka prozkoumá potenciálně použít kombinaci mnoho klienta a jedna klienta databáze, povolení *hybridní* modelu správy klienta. Správa těchto databází je složitá. Služba [Elastic Jobs](sql-database-elastic-jobs-overview.md) usnadňuje správu a řízení datové vrstvy SQL. Úlohy umožňují bezpečně a spolehlivě spuštění skriptů jazyka Transact-SQL jako úloh pro skupinu databází klienta. Úkoly jsou nezávislé na vstupu nebo interakci uživatelů. Tato metoda slouží k nasazení změny schématu nebo běžné referenční data v rámci všech klientů v aplikaci. Elastické úlohy lze také udržovat zlaté šablony kopii databáze. Šablona se používá k vytvoření nové klienty, vždy zajistíte nejnovější schéma a referenční data jsou používány.

![obrazovka](media/saas-multitenantdb-schema-management/schema-management.png)

## <a name="elastic-jobs-limited-preview"></a>Elastic Jobs verze Limited Preview

Není k dispozici nová verze elastické úlohy, který je teď integrované funkce databáze SQL Azure. Pomocí integrovaného jsme znamená, že se nevyžaduje žádné další služby ani součásti. Tato nová verze služby Elastic Jobs je v současnosti ve verzi Limited Preview. Omezené preview aktuálně podporuje prostředí PowerShell k vytvoření úlohy účty a T-SQL k vytvoření a Správa úloh.

## <a name="get-the-wingtip-tickets-saas-multi-tenant-database-application-source-code-and-scripts"></a>Zdrojový kód Wingtip lístky SaaS víceklientské databáze aplikace a skripty

Adresář Wingtip lístky SaaS víceklientské databázové skripty a zdrojový kód aplikace, které jsou k dispozici v [WingtipTicketsSaaS MultitenantDB](https://github.com/microsoft/WingtipTicketsSaaS-MultiTenantDB) úložišti na Githubu. Najdete v článku [obecné pokyny](saas-tenancy-wingtip-app-guidance-tips.md) kroky, jak stáhnout a odblokování skripty Wingtip lístky SaaS. 

## <a name="create-a-job-account-database-and-new-job-account"></a>Vytvoření databáze účtu úlohy a nového účtu úlohy

Tento kurz vyžaduje vytvoření databáze účtů úlohy a úlohy účtu pomocí prostředí PowerShell. Jako databázi MSDB používá Agent serveru SQL používá elastické úlohy Azure SQL database k uložení definice úlohy, stav úlohy a historie. Po vytvoření účtu úlohy, můžete vytvořit a monitorujte úlohy, okamžitě.

1. V **prostředí PowerShell ISE**, otevřete *... \\Učení moduly\\Správa schématu\\ukázku SchemaManagement.ps1*.
2. Stisknutím klávesy **F5** spusťte skript.

*Ukázku SchemaManagement.ps1* skript volání *nasadit SchemaManagement.ps1* skript pro vytvoření vrstvy *S2* databáze s názvem **jobaccount** na serveru katalogu. Skript pak vytvoří účet úlohy, předávání databázi jobaccount jako parametr pro volání vytvoření účtu úlohy.

## <a name="create-a-job-to-deploy-new-reference-data-to-all-tenants"></a>Vytvoření úlohy, která u všech tenantů nasadí nová referenční data

#### <a name="prepare"></a>Příprava

Každá databáze klientů zahrnuje sadu typy místo v **VenueTypes** tabulky. Typy místo definování typu události, které jsou hostované na místo. V tomto cvičení nasadit aktualizace pro všechny databáze přidat dva typy další místo: *Motorky Racing* a *křížovou kartou plaveckých*. Tyto typy míst odpovídají obrázkům na pozadí zobrazeným v aplikaci událostí tenanta.

Před nasazením nové referenční data, poznamenejte si počet typů místo, které již existují, což může být 10. Poznámka: proveďte kliknutím na následující odkaz na adresář Wingtip webového uživatelského rozhraní, a kliknutím **místo typu** rozevírací nabídky:
- http://events.Wingtip-mt.<USER>. trafficmanager.net

Nyní můžete počítat počet původní místo typy. Konkrétně, Všimněte si, že ani *Motorky Racing* ani *křížovou kartou plaveckých* dosud neexistují.

#### <a name="steps"></a>Kroky

Teď vytvoříte úlohu, která aktualizuje **VenueTypes** tabulky v databázi jednotlivých klientů, přidáním dva nové typy místo.

Pokud chcete vytvořit novou úlohu, použijete sadu úloh systémové uložené procedury, které byly vytvořeny *jobaccount* databáze. Postupy byly vytvořeny při vytvoření účtu úlohy.

1. V aplikaci SSMS, připojení k serveru klienta: tenants1-mt -\<uživatele\>. database.windows.net

2. Vyhledejte *tenants1* databáze na *tenants1-mt –\<uživatele\>. database.windows.net* serveru.

3. Dotaz *VenueTypes* tabulka, která se potvrďte, že *Motorky Racing* a *křížovou kartou plaveckých* nejsou ještě v seznamu výsledků.

4. Připojení k serveru katalogu, což je *katalogu-mt -\<uživatele\>. database.windows.net*.

5. Připojení k *jobaccount* databáze na serveru katalogu.

6. V aplikaci SSMS, otevřete soubor *... \\Učení moduly\\Správa schématu\\DeployReferenceData.sql*.

7. Upravit příkaz: nastavte @User = &lt;uživatele&gt; a nahraďte hodnotu uživatele použít při nasazení aplikace Wingtip lístky SaaS víceklientské databáze.

8. Stisknutím klávesy **F5** spusťte skript.

#### <a name="observe"></a>Pozorovat

Zkontrolujte následující položky *DeployReferenceData.sql* skriptu:

- **SP\_přidat\_cíl\_skupiny** vytvoří cílová skupina *DemoServerGroup*, a přidá cílový členy do skupiny.

- **SP\_přidat\_cíl\_skupiny\_člen** přidá následující položky:
    - A *server* cíle typ člena.
        - Toto je *tenants1-mt -&lt;uživatele&gt;*  serveru, který obsahuje klienty databáze.
        - Proto všechny databáze na serveru jsou zahrnuty v úloze když provede úlohu.
    - A *databáze* cíle typ člena pro databázi zlaté (*basetenantdb*), kde je umístěná *katalogu-mt –&lt;uživatele&gt;*  serveru
    - A *databáze* cíle typ člena zahrnout *adhocreporting* databázi, která se používá novější kurzu.

- **SP\_přidat\_úlohy** vytvoří úlohu názvem *referenční Data nasazení*.

- **SP\_přidat\_krok úlohy** vytvoří krok úlohy obsahující text příkazů T-SQL aktualizovat na referenční tabulku VenueTypes.

- Zbývající pohledy ve skriptu zobrazují existující objekty a monitorují provádění úlohy. Ke kontrole hodnota stavu v použijte tyto dotazy **životního cyklu** sloupec k určení, kdy má úloha úspěšně dokončena. Úloha aktualizace databáze klientů a aktualizuje dva další databáze, které obsahují na referenční tabulku.

V aplikaci SSMS, přejděte do databáze klienta na *tenants1-mt –&lt;uživatele&gt;*  serveru. Dotaz *VenueTypes* tabulka, která se potvrďte, že *Motorky Racing* a *křížovou kartou plaveckých* jsou nyní přidat do tabulky. Celkový počet místo typy by měl mít zvýšila dvakrát.

## <a name="create-a-job-to-manage-the-reference-table-index"></a>Vytvoření úlohy pro správu indexu referenční tabulky

Tento postup je podobný předchozím cvičení. Tento postup vytvoří úlohu znovu sestavit index referenční primární klíč tabulky. Operace správy typické databáze, které správce může spustit po zatížení velkých objemů dat do tabulky, je opětovné sestavení indexu ke zlepšení výkonu.

1. V aplikaci SSMS, připojte k *jobaccount* databáze v *katalogu-mt -&lt;uživatele&gt;. database.windows.net* serveru.

2. V aplikaci SSMS, otevřete *... \\Učení moduly\\Správa schématu\\OnlineReindex.sql*.

3. Stisknutím klávesy **F5** spusťte skript.

#### <a name="observe"></a>Pozorovat

Zkontrolujte následující položky *OnlineReindex.sql* skriptu:

* **SP\_přidat\_úlohy** vytvoří novou úlohu názvem *Online přeindexování Primárníklíč\_\_VenueTyp\_\_265E44FD7FD4C885*.

* **SP\_přidat\_krok úlohy** vytvoří krok úlohy obsahující text T-SQL příkaz k aktualizaci indexu.

* Zbývající zobrazení ve skriptu monitorování provádění úlohy. Použít tyto dotazy ke kontrole hodnota stavu v **životního cyklu** k určení, když úloha úspěšně dokončil na všechny členy skupiny cílový sloupec.

## <a name="additional-resources"></a>Další zdroje informací:

<!-- TODO: Additional tutorials that build upon the Wingtip Tickets SaaS Multi-tenant Database application deployment (*Tutorial link to come*)
(saas-multitenantdb-wingtip-app-overview.md#sql-database-wingtip-saas-tutorials)
-->
* [Správa cloudových databází s horizontálním navýšením kapacity](sql-database-elastic-jobs-overview.md)
* [Vytvoření a správa databází s horizontálním navýšením kapacity](sql-database-elastic-jobs-create-and-manage.md)

## <a name="next-steps"></a>Další postup

V tomto kurzu jste se naučili:

> [!div class="checklist"]
> - Vytvořte účet úlohy k dotazování mezi několik klientů.
> - Aktualizujte data v všechny databáze klienta.
> - Vytvoření indexu pro tabulku v všechny databáze klienta.

Zkuste [Ad-hoc reporting kurzu](saas-multitenantdb-adhoc-reporting.md).

