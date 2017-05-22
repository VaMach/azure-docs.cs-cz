---
title: "Zřízení nových tenantů v aplikaci s více tenanty využívající službu Azure SQL Database | Dokumentace Microsoftu"
description: "Zřízení a katalogizace nových tenantů v ukázkové SaaS aplikaci Wingtip Tickets (WTP) využívající službu SQL Database"
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
ms.openlocfilehash: aae5d85a18f93b7821a6ef8fc7161dd9a6ebe533
ms.contentlocale: cs-cz
ms.lasthandoff: 05/12/2017


---
# <a name="provision-new-tenants-and-register-them-in-the-catalog"></a>Zřízení nových tenantů a jejich registrace v katalogu

V tomto kurzu zřídíte nové tenanty v SaaS aplikaci WTP (Wingtip Tickets Platform). Vytvoříte tenanty, databáze tenantů a zaregistrujete tenanty do katalogu. *Katalog* je databáze, kde se uchovává mapování různých tenantů aplikací SaaS a jejich dat. S těmito skripty můžete prozkoumat použitý způsob zřizování tenantů a jejich přidání do katalogu. Můžete také prozkoumat, jak je implementovaná registrace nových tenantů do katalogu. Katalog hraje důležitou roli při přesměrování požadavků aplikace na správné databáze.

V tomto kurzu se naučíte:

> [!div class="checklist"]

> * Zřídit jednoho nového tenanta.
> * Zřídit dávku dalších tenantů.
> * Vstoupit do podrobností o zřizovaní nových tenantů a zaregistrovat je do katalogu.


Předpokladem dokončení tohoto kurzu je splnění následujících požadavků:

* Je nasazená aplikace WTP. Informace o nasazení, které netrvá ani pět minut, najdete v článku [Nasazení SaaS aplikace WTP a seznámení s ní](sql-database-saas-tutorial.md).
* Prostředí Azure PowerShell je nainstalované. Podrobnosti najdete v článku [Začínáme s prostředím Azure PowerShell](https://docs.microsoft.com/powershell/azure/get-started-azureps).

## <a name="introduction-to-the-saas-catalog-pattern"></a>Úvod do katalogu SaaS

V aplikaci SaaS, která má databázi a více tenantů, je důležité vědět, kde jsou uložené informace každého tenanta. V katalogovém systému SaaS se k uchovávání mapování tenantů na jejich uložená data používá databáze katalogu. Aplikace WTP používá databázovou architekturu jednoho tenanta, ale základní vzorec ukládání mapování tenantů na databáze do katalogu platí bez ohledu na to, jestli je použitá databáze pro více tenantů nebo jen pro jednoho tenanta.

Každý tenant má přiřazený klíč, který odlišuje jeho data v katalogu. V aplikaci WTP je tento klíč tvořený hodnotou hash názvu tenanta. Tímto způsobem můžete použít část názvu tenanta z URL aplikace, sestavit z něj klíč a načíst připojení konkrétního tenanta. Použití dalších ID schémat nemá vliv na celkový systém.

K implementaci katalogu v aplikaci WTP se používá technologie Shard Management v [klientské knihovně Elastic Database (EDCL)](sql-database-elastic-database-client-library.md). EDCL zodpovídá za vytvoření a správu databázového _katalogu_, který uchovává _mapu horizontálního dělení_. Katalog obsahuje mapování klíčů (tenantů) na jejich databáze (horizontální oddíly).

> [!IMPORTANT]
> Data o mapování jsou přístupná v katalogové databázi, ale *neupravujte je*! K jejich úpravě používejte jen rozhraní API klientské knihovny Elastic Database. Pokud budete manipulovat přímo s daty mapování, riskujete poškození katalogu, protože takový postup není podporovaný.

SaaS aplikace Wingtip zřizuje nové tenanty tím, že zkopíruje *zlatou* databázi.

## <a name="get-the-wingtip-application-scripts"></a>Získání skriptů aplikace Wingtip

Skripty a zdrojový kód aplikace Wingtip Tickets jsou k dispozici v úložišti GitHubu [WingtipSaaS](https://github.com/Microsoft/WingtipSaaS). Soubory se skripty jsou ve [složce Learning Modules](https://github.com/Microsoft/WingtipSaaS/tree/master/Learning%20Modules). Stáhněte si složku **Learning Modules** do svého místního počítače. Dejte pozor, abyste zachovali strukturu složky.

## <a name="provision-a-new-tenant"></a>Zřízení nového tenanta

Pokud jste už tenanta vytvořili v prvním kurzu WTP, můžete přejít k další části, kterou je [zřízení dávky tenantů](#provision-a-batch-of-tenants).

Spusťte skript *Demo-ProvisionAndCatalog*, kterým rychle vytvoříte tenanta a zaregistrujete ho v katalogu:

1. V integrovaném skriptovacím prostředí (ISE) v prostředí Windows PowerShell otevřete skript **Demo-ProvisionAndCatalog.ps1** a nastavte následující hodnoty:
   * **$TenantName** = název nového místa (například *Bushwillow Blues*). 
   * **$VenueType** = jeden z předdefinovaných typů místa: blues, classicalmusic, dance, jazz, judo, motorracing, multipurpose, opera, rockmusic, soccer.
   * **$DemoScenario** = 1. Při **zřizování jednoho tenanta** nechte nastavenou hodnotu _1_.

1. Stiskněte **F5** a spusťte skript.

Jakmile skript skončí, je nový tenant zřízený a aplikace *Events* se otevře v prohlížeči:

![Nový tenant](./media/sql-database-saas-tutorial-provision-and-catalog/new-tenant.png)


## <a name="provision-a-batch-of-tenants"></a>Zřízení dávky tenantů

V tomto cvičení zřídíte dávku dalších tenantů. Doporučujeme, abyste si napřed udělali toto cvičení a teprve potom dokončili další kurzy WTP.

1. V *integrovaném skriptovacím prostředí (ISE) v prostředí Windows PowerShell* otevřete skript ...\\Learning Modules\\Utilities\\*Demo-ProvisionAndCatalog.ps1* a nastavte následující hodnotu:
   * **$DemoScenario** = **3** nastavte na hodnotu **3**, abyste **zřídili dávku tenantů**.
1. Stiskněte **F5** a spusťte skript.

Skript nasadí dávku dalších tenantů. Použije [šablonu Azure Resource Manageru](../azure-resource-manager/resource-manager-template-walkthrough.md), která dávku řídí a deleguje zřizování každé databáze na propojenou šablonu. Tento způsob použití šablon umožňuje Azure Resource Manageru zprostředkovat skriptu proces zřizování. Šablony zřizují databáze souběžně na možných místech. V případě potřeby ošetří opakované pokusy a optimalizují celkový proces. Skript je idempotentní. Pokud se přeruší, spusťte ho znovu.

### <a name="verify-the-batch-of-tenants-successfully-deployed"></a>Ověření úspěšného nasazení dávky tenantů

* Otevřete server *tenants1* na portálu [Azure Portal](https://portal.azure.com) a klikněte na **SQL databases** (Databáze SQL):

   ![seznam databází](media/sql-database-saas-tutorial-provision-and-catalog/database-list.png)


## <a name="provision-and-catalog-detailed-walkthrough"></a>Podrobný postup zřizování a přidání tenantů do katalogu

Abyste dokázali lépe pochopit, jak je v aplikaci Wingtip implementované zřizování nového tenanta, znovu spusťte skript *Demo-ProvisionAndCatalog*, abyste zřídili dalšího tenanta. Tentokrát ale přidejte přerušení a projděte si pracovní postup:

1. Otevřete skript ...\\Learning Modules\Utilities\_Demo-ProvisionAndCatalog.ps1_ a nastavte následující parametry na hodnoty nového tenanta, které v aktuálním katalogu neexistují:
   * **$TenantName** = nastavte nový název (například *Hackberry Hitters*).
   * **$VenueType** = použijte jedno z předdefinovaných typů míst (například *judo*).
   * **$DemoScenario** = 1. Při **zřizování jednoho tenanta** nechte nastavenou hodnotu **1**.

1. Přidejte přerušení: Dejte kurzor někam na řádek *New-Tenant `* a stiskněte **F9**.

   ![přerušení](media/sql-database-saas-tutorial-provision-and-catalog/breakpoint.png)

1. Stisknutím klávesy **F5** spusťte skript. Jakmile dosáhnete přerušení, stiskněte **F11** pro vstup. Sledujte provádění skriptu a funkčními klávesami **F10** a **F11** přeskočte volané funkce nebo do nich vstupte. [Tipy pro práci se skripty PowerShellu a jejich ladění](https://msdn.microsoft.com/powershell/scripting/core-powershell/ise/how-to-debug-scripts-in-windows-powershell-ise)

### <a name="examine-the-provision-and-catalog-implementation-in-detail-by-stepping-through-the-script"></a>Podrobná kontrola implementace zřizování tenantů a jejich přidání do katalogu procházením skriptu

Při zřizování nových tenantů a jejich přidávání do katalogu skript provádí tyto kroky:

1. **Importuje modul SubscriptionManagement.psm1**, který obsahuje funkce pro přihlášení do Azure a výběr předplatného Azure, se kterým pracujete.
1. **Importuje modul CatalogAndDatabaseManagement.psm1**, který nabízí obecný přehled funkcí pro [správu horizontálních oddílů (Shard Management)](sql-database-elastic-scale-shard-map-management.md) na úrovni katalogu a tenanta. Jde o důležitý modul, který ukrývá většinu funkcí katalogu, a proto určitě stojí za prozkoumání.
1. **Získejte podrobnosti o konfiguraci**. Vstupte do _Get-Configuration_ (funkční klávesou **F11**) a podívejte se, jak je zadaná konfigurace aplikace. Jsou zde definované názvy prostředků a jiné hodnoty konkrétní aplikace, ale neměňte je, dokud se s těmito skripty neseznámíte.
1. **Získejte katalog objektů**. Vstupte do *Get-Catalog* a podívejte se na inicializaci katalogu s použitím funkcí správy horizontálních oddílů (Shard Management) importovaných z **AzureShardManagement.psm1**. Katalog se skládá z následujících objektů:
   * $catalogServerFullyQualifiedName je vytvořený ze standardního kmene, ke kterému je přidané vaše uživatelské jméno: _catalog-\<user\>.database.windows.net_.
   * $catalogDatabaseName se načte z této konfigurace: *tenantcatalog*.
   * Objekt $shardMapManager se inicializuje z databáze katalogu.
   * Objekt $shardMap se inicializuje z mapy horizontálního dělení *tenantcatalog* v databázi katalogu.
   Objekt katalogu se sestaví, vrátí a použije ve skriptu vyšší úrovně.
1. **Vypočítejte klíč nového tenanta**. K vytvoření klíče tenanta z jeho jména se používá funkce hash.
1. **Zkontrolujte, jestli klíč tenanta ještě neexistuje**. Pokud chcete ověřit dostupnost klíče, zkontrolujte katalog.
1. **Ke zřízení databáze tenanta se použije New-TenantDatabase.** Pokud chcete do procesu vstoupit a podívat se, jak se zřizuje databáze pomocí šablony Resource Manageru, použijte funkční klávesu **F11**.
    
Název databáze je sestavený z názvu tenanta, aby bylo jasné, jakému tenantovi příslušný horizontální oddíl patří. (Můžete také jednoduše použít jiný způsob pojmenování databáze.)

Šablona Resource Manageru se použije k **vytvoření databáze tenanta zkopírováním *zlaté* databáze** (baseTenantDB) na server katalogu.  Alternativní postup spočívá ve vytvoření prázdné databáze, kterou následně inicializujete importem souboru bacpac.

Šablona Resource Manageru je ve složce …\\Learning Modules\\Common\\:  *tenantdatabasecopytemplate.json*

Po vytvoření databáze tenanta proběhne další inicializace, kdy se doplní název a typ místa (tenanta). Zde také můžete provést další inicializaci.

Databáze tenanta se zaregistruje do katalogu příkazem *Add-TenantDatabaseToCatalog* pod klíčem tenanta. Ke vstupu do podrobností použijte funkční klávesu **F11**.

* Databáze katalogu se přidá do mapy horizontálního dělení (tj. do seznamu známých databází).
* Vytvoří se mapování, které propojí hodnotu klíče s horizontálním oddílem.
* A přidají se další metadata (název místa) o tenantovi.

Jakmile je zřizování hotové, spuštění se vrátí k původnímu skriptu *Demo-ProvisionAndCatalog* a v prohlížeči se otevře stránka **events** nového tenanta:

   ![stránka events](media/sql-database-saas-tutorial-provision-and-catalog/new-tenant2.png)


## <a name="other-provisioning-patterns"></a>Další způsoby zřizování

K dalším způsobům zřizování, které nejsou zahrnuty do tohoto kurzu, patří:

**Předběžné zřizování databází.** Tento způsob využívá skutečnost, že databáze v elastickém fondu nepřidávají další náklady (účtování probíhá za elastický fond, ne za databáze), protože nečinné databáze nespotřebovávají žádné prostředky. Předběžným zřizováním databází ve fondu a jejich přidělením v případě potřeby výrazně zkrátíte čas potřebný ke zprovoznění tenanta. Počet předběžně zřizovaných databází můžete podle potřeby upravit, abyste měli dostatečnou rezervu, která odpovídá předpokládanému tempu zřizování tenantů.

**Automatické zřizování.** U tohoto způsobu se k automatickému zřizování serverů, fondů a databází, včetně předem zřizovaných databází v elastických fondech (pokud jsou potřeba), používá vyhrazená zřizovací služba. Pokud jsou databáze stažené z provozu a odstraněné, může mezery v elastických fondech podle potřeby zaplnit zřizovací služba. Tato služba může být jednoduchá nebo složitá, třeba zajištění zřizování v různých zeměpisných oblastech a automatické nastavení geografické replikace, pokud tuto strategii používáte k redukci dat (DR). Při automatickém zřizování odešle klientská aplikace nebo skript žádost o zřízení do fronty, kterou zpracovává zřizovací služba, a pak se této služby zeptá na dokončení. Při použití předběžného zřizování požadavky rychle zpracovává služba, která spravuje zřizování náhradní databáze spuštěné na pozadí.


## <a name="stopping-wingtip-saas-application-related-billing"></a>Zastavení účtování spojeného s SaaS aplikací Wingtip

Pokud nechcete pokračovat dalším kurzem, doporučujeme odstranit všechny prostředky, abyste pozastavili účtování. Odstraňte skupinu prostředků, na které jste nasadili aplikaci WTP. Tím odstraníte všechny její prostředky.

* Na portálu přejděte ke skupině prostředků aplikace a odstraňte ji, abyste zastavili veškeré účtování spojené s nasazením aplikace WTP.

## <a name="tips"></a>Tipy

* Knihovna EDCL dále nabízí důležité funkce, které klientským aplikacím umožňují připojit se ke katalogu a manipulovat s ním. EDCL také můžete použít k načtení připojení ADO.NET pro danou hodnotu klíče, abyste aplikaci umožnili připojit se ke správné databázi. Klient ukládá tyto informace o připojení do mezipaměti, aby minimalizoval přenosy do databáze katalogu a zrychlil aplikaci.



## <a name="next-steps"></a>Další kroky

V tomto kurzu jste se naučili:

> [!div class="checklist"]

> * Zřídit jednoho nového tenanta.
> * Zřídit dávku dalších tenantů.
> * Vstoupit do podrobností o zřizovaní nových tenantů a zaregistrovat je do katalogu.

[Kurz o monitorování výkonu](sql-database-saas-tutorial-performance-monitoring.md)

## <a name="additional-resources"></a>Další zdroje

* [Další kurzy, které vycházejí z původně nasazené aplikace WTP (Wingtip Tickets Platform)](sql-database-wtp-overview.md#sql-database-wtp-saas-tutorials)
* [Klientská knihovna Elastic Database](https://docs.microsoft.com/azure/sql-database/sql-database-elastic-database-client-library)
* [Ladění skriptů v integrovaném skriptovacím prostředí (ISE) v prostředí Windows PowerShell](https://msdn.microsoft.com/powershell/scripting/core-powershell/ise/how-to-debug-scripts-in-windows-powershell-ise)

