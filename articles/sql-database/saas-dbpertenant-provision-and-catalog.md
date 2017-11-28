---
title: "Zřídit nové klienty ve víceklientské aplikaci, která používá Azure SQL Database | Microsoft Docs"
description: "Zjistěte, jak zřídit a nové klienty v aplikaci SaaS víceklientské Azure SQL Database v katalogu"
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
ms.date: 08/11/2017
ms.author: sstein
ms.openlocfilehash: b82623f63681daff502f1e23d052da7480dda942
ms.sourcegitcommit: f847fcbf7f89405c1e2d327702cbd3f2399c4bc2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/28/2017
---
# <a name="learn-how-to-provision-new-tenants-and-register-them-in-the-catalog"></a>Zjistěte, jak zřídit nové klienty a zaregistrujte je v katalogu

V tomto kurzu informace o zřizování a katalog vzory SaaS a jak jsou implementované v databázi Wingtip lístky SaaS na aplikaci klienta. Vytvoření a inicializace nové databáze klienta a zaregistrujte je v katalogu klienta aplikace. Katalog je databáze, která zajišťuje mapování mezi aplikace SaaS velký počet klientů a jejich data. Katalog hraje důležitou roli odkazovat aplikace a správu žádosti o databázi správné.  

V tomto kurzu se naučíte:

> [!div class="checklist"]

> * Zřídit jednoho nového klienta, včetně procházení jak tato možnost je implementovaná
> * Zřídit dávku dalších tenantů.


Předpokladem dokončení tohoto kurzu je splnění následujících požadavků:

* Databáze Wingtip lístky SaaS za klienta aplikace se nasazuje. Nasazení za méně než pět minut najdete v tématu [nasazení a seznamte se s databázi Wingtip lístky SaaS na aplikaci klienta](saas-dbpertenant-get-started-deploy.md)
* Je nainstalované prostředí Azure PowerShell. Podrobnosti najdete v článku [Začínáme s prostředím Azure PowerShell](https://docs.microsoft.com/powershell/azure/get-started-azureps).

## <a name="introduction-to-the-saas-catalog-pattern"></a>Úvod do katalogu SaaS

V databázi zálohovanou víceklientské aplikaci SaaS je důležité vědět, se uloží informace pro každého klienta. Ve vzoru katalogu SaaS je databáze katalogu používané pro udržení mapování mezi každého klienta a databázi, ve kterém jsou uchovávána data. Databáze SaaS lístky Wingtip jednu aplikaci klienta používá jednoho klienta za model databáze, ale základní vzor ukládání mapování klienta do databáze v katalog platí vždy, když klient dat je distribuován do více databází, zda více klientů nebo databáze jednoho klienta se používá.

Každý klient je přiřazen klíč, který označuje je v katalogu a která se mapuje na umístění příslušné databáze. V aplikaci SaaS lístky Wingtip klíč je vytvořen z hodnoty hash názvu klienta. To umožňuje název část adresy URL aplikace, který se má použít k vytvoření klíče klienta. Může použít jiná schémata klíče klienta.  

Katalogu umožňuje název nebo umístění databáze se musí změnit s minimálním dopadem na aplikaci.  V databázi víceklientského modelu to rovněž umožňuje, 'Přesun' klienta mezi databázemi.  Katalog lze také označuje, zda klient nebo databáze do režimu offline za účelem údržby nebo jiné akce. To je prozkoumali v [obnovení jednoho klienta kurzu](saas-dbpertenant-restore-single-tenant.md).

Kromě toho katalog, který je v platnosti databáze správy pro aplikace SaaS, můžete ukládat další metadata klienta nebo databáze, například vrstvy nebo edici databáze, verzi schématu, plán služeb nebo SLA nabízených klientům a dalších informací, které umožňuje správu aplikací, na zákaznickou podporu nebo devops procesy.  

Nad rámec aplikace SaaS můžete povolit katalogu Nástroje databáze.  V databázi Wingtip lístky SaaS na klienta vzorek, katalogu slouží k povolení klienta mezi dotaz, prozkoumali v [ad hoc analytics kurzu](saas-tenancy-adhoc-analytics.md). Správa úloh mezidatabázové je prozkoumali v [Správa schématu](saas-tenancy-schema-management.md) a [klienta analytics](saas-tenancy-tenant-analytics.md) kurzy. 

V ukázkách Wingtip lístky SaaS katalogu je implementovaná pomocí funkcím pro správu horizontálního oddílu [elastické databáze klienta knihovny (EDCL)](sql-database-elastic-database-client-library.md). EDCL je k dispozici v jazyce Java a rozhraní .net Framework. EDCL umožňuje aplikaci vytvářet, spravovat a použít mapování horizontálních databáze zálohována. Mapování horizontálních obsahuje seznam horizontálních oddílů (databáze) a mapování mezi klíče (klienty) a horizontálních oddílů.  EDCL funkcí lze z aplikace nebo skripty prostředí PowerShell během zřizování vytvořit položky v mapě horizontálního oddílu klienta a z aplikace pro efektivní připojení k databázi správné. EDCL ukládá do mezipaměti informace o připojení a minimalizovat provoz do databáze katalogu zrychlit aplikace.  

> [!IMPORTANT]
> Data o mapování jsou přístupná v katalogové databázi, ale *neupravujte je*! K jejich úpravě používejte jen rozhraní API klientské knihovny Elastic Database. Pokud budete manipulovat přímo s daty mapování, riskujete poškození katalogu, protože takový postup není podporovaný.


## <a name="introduction-to-the-saas-provisioning-pattern"></a>Úvod do vzoru zřizování SaaS

Při připojování nového klienta v aplikaci SaaS, která používá model databáze jednoho klienta novou databázi klienta musí být zřízená.  Musí být vytvořený v příslušné umístění a úroveň služby, inicializovat pomocí příslušného schématu a referenčních dat a poté zaregistrovány v katalogu pod klíčem odpovídající klienta.  

Různý přístup umožňuje databáze zřizování, které můžou zahrnovat provádění skripty SQL, nasazení souboru bacpac nebo kopírování šablony databáze.  

Zřizování přístupů, které používáte musí comprehended součástí strategie pro správu celkové schéma, který musí zajistit, že nové databáze zřízený s nejnovější schématu.  To je prozkoumali v [schématu správu kurzu](saas-tenancy-schema-management.md).  

Databázi Wingtip lístky SaaS jednu aplikaci klienta zřídí nové klienty pomocí kopírování šablony databáze s názvem _basetenantdb_, nasazené na serveru katalogu.  Zřizování může být integrovaná do aplikace jako součást registrace prostředí a podporované v režimu offline pomocí skriptů. V tomto kurzu jsou zde popsány zřizování pomocí prostředí PowerShell. Zřizování skripty zkopírujte databázi basetenantdb vytvořit novou databázi klienta v elastickém fondu, pak provést jeho inicializaci s informacemi o konkrétního klienta a její registrace v mapě horizontálního oddílu katalogu.  V Wingtip lístky SaaS databáze na aplikaci klienta klienta databází mají názvy na základě názvu klienta, ale nejedná se o důležitou součástí vzoru – použití katalogu umožňuje libovolný název přiřazení klienta databází. + 


## <a name="get-the-wingtip-tickets-saas-database-per-tenant-application-scripts"></a>Získat aplikační skripty Wingtip lístky SaaS databáze za klienta

Adresář Wingtip lístky SaaS víceklientské databázové skripty a zdrojový kód aplikace, které jsou k dispozici v [WingtipTicketsSaaS DbPerTenant](https://github.com/Microsoft/WingtipTicketsSaaS-DbPerTenant) úložiště GitHub. Podívejte se [obecné pokyny](saas-tenancy-wingtip-app-guidance-tips.md) kroky, jak stáhnout a odblokování skripty Wingtip lístky SaaS.


## <a name="provision-and-catalog-detailed-walkthrough"></a>Podrobný postup zřizování a přidání tenantů do katalogu

Abyste pochopili, jak aplikace Wingtip implementuje nového klienta zřizování, přidejte zarážek a krok v pracovním postupu při zřizování klienta:

1. V _prostředí PowerShell ISE_, otevřete... \\Learning moduly\\ProvisionAndCatalog\\_ukázku ProvisionAndCatalog.ps1_ a nastavit následující parametry:
   * **$TenantName** = název nového místa (například *Bushwillow Blues*).
   * **$VenueType** = jeden z typů předdefinovaného místo: *modré*, classicalmusic, tance, jazz, judo, motorracing víceúčelových, opera, rockmusic, fotbalový.
   * **$DemoScenario** = **1**do *zřídit jednoho klienta*.

1. Přidejte zarážku umístěním kurzoru kdekoli v řádku 48, řádek, která uvádí, že: *nového klienta,*a stiskněte klávesu **F9**.

   ![přerušení](media/saas-dbpertenant-provision-and-catalog/breakpoint.png)

1. Ke spuštění skriptu stiskněte **F5**.

1. Po spuštění skriptu se zastaví u zarážky, stiskněte klávesu **F11** ke kroku do kódu.

   ![Ladění](media/saas-dbpertenant-provision-and-catalog/debug.png)



Trasování spuštění skriptu na pomocí **ladění** možnosti nabídky - **F10** a **F11** krok přes nebo do volaných funkcí. Další informace o ladění skriptů prostředí PowerShell najdete v tématu [tipy k ladění skriptů prostředí PowerShell a práce s](https://msdn.microsoft.com/powershell/scripting/core-powershell/ise/how-to-debug-scripts-in-windows-powershell-ise).


Následující nejsou explicitně provést kroky, ale vysvětlení, které jednotlivé kroky při ladění skriptu pracovního postupu:

1. **Importuje modul SubscriptionManagement.psm1**, který obsahuje funkce pro přihlášení do Azure a výběr předplatného Azure, se kterým pracujete.
1. **Importuje modul CatalogAndDatabaseManagement.psm1**, který nabízí obecný přehled funkcí pro [správu horizontálních oddílů (Shard Management)](sql-database-elastic-scale-shard-map-management.md) na úrovni katalogu a tenanta. Jde o důležitý modul, který ukrývá většinu funkcí katalogu, a proto určitě stojí za prozkoumání.
1. **Získejte podrobnosti o konfiguraci**. Krok do Get-konfigurace (s F11), abyste viděli, jak je zadána konfigurace aplikace. Jsou zde definované názvy prostředků a jiné hodnoty konkrétní aplikace, ale neměňte je, dokud se s těmito skripty neseznámíte.
1. **Získejte katalog objektů**. Krok do Get-katalogu, která vytvoří a vrátí objekt katalog, který se používá ve skriptu vyšší úrovně.  Tato funkce využívá funkce správy horizontálního oddílu, které jsou importovány z **AzureShardManagement.psm1**. Objekt katalogu se skládá z následujících akcí:
   * $catalogServerFullyQualifiedName je vytvořený ze standardního kmene, ke kterému je přidané vaše uživatelské jméno: _catalog-\<user\>.database.windows.net_.
   * $catalogDatabaseName se načte z této konfigurace: *tenantcatalog*.
   * Objekt $shardMapManager se inicializuje z databáze katalogu.
   * Objekt $shardMap se inicializuje z mapy horizontálního dělení *tenantcatalog* v databázi katalogu.
   Objekt katalogu se sestaví, vrátí a použije ve skriptu vyšší úrovně.
1. **Vypočítejte klíč nového tenanta**. K vytvoření klíče tenanta z jeho jména se používá funkce hash.
1. **Zkontrolujte, jestli klíč tenanta ještě neexistuje**. Pokud chcete ověřit dostupnost klíče, zkontrolujte katalog.
1. **Ke zřízení databáze tenanta se použije New-TenantDatabase.** Použití **F11** krok v a v tématu jak databáze je zřízený pomocí [šablony Azure Resource Manageru](../azure-resource-manager/resource-manager-template-walkthrough.md).

    Název databáze je sestavený z názvu tenanta, aby bylo jasné, jakému tenantovi příslušný horizontální oddíl patří. (Můžete také jednoduše použít jiný způsob pojmenování databáze.) Šablony Resource Manageru se používá k vytvoření databáze klienta zkopírováním šablony databáze (baseTenantDB) na serveru katalogu. Alternativní způsob může být vytvořit prázdnou databázi a potom inicializujte importováním souboru bacpac, nebo pro spuštění skriptu inicializaci ze známých umístění.  

    Šablona Resource Manager je ve složce Modules\Common\ ...\Learning: *tenantdatabasecopytemplate.json*

1. Po vytvoření databáze klienta je pak další **inicializován s názvem místo (klientů) a typ místo**. Zde také můžete provést další inicializaci.

1. **Databáze klienta je zaregistrován v katalogu** s *přidat TenantDatabaseToCatalog* pomocí klíče klienta. Ke vstupu do podrobností použijte funkční klávesu **F11**.

    * Databáze katalogu se přidá do mapy horizontálního dělení (tj. do seznamu známých databází).
    * Vytvoří se mapování, které propojí hodnotu klíče s horizontálním oddílem.
    * Další metadata o klientovi (název místo) se přidá do tabulky klienty v katalogu.  Tabulka klienty není součástí ShardManagement schéma a není nainstalovaná v rámci EDCL.  Tato tabulka ukazuje, jak může být databáze katalogu rozšířené k podpoře další data specifické pro aplikaci.   


Po zřízení dokončení zpracování vrátí do původního *ukázku ProvisionAndCatalog* skript, který se otevře **události** stránky pro nového klienta v prohlížeči:

   ![stránka events](media/saas-dbpertenant-provision-and-catalog/new-tenant.png)


## <a name="provision-a-batch-of-tenants"></a>Zřídit dávky klientů

Toto cvičení zřídí dávky 17 klientů. Doporučuje se, že před spuštěním další adresář Wingtip lístky SaaS databáze za klienta kurzy, takže víc než několik databází pro práci s zřídíte tuto dávku klientů.

1. V *prostředí PowerShell ISE*, otevřete... \\Learning moduly\\ProvisionAndCatalog\\*ukázku ProvisionAndCatalog.ps1* a změňte *$DemoScenario* parametru 3:
   * **$DemoScenario** = **3**do *zřídit dávky klienty*.
1. Stisknutím klávesy **F5** spusťte skript.

Skript nasadí dávku dalších tenantů. Použije [šablonu Azure Resource Manageru](../azure-resource-manager/resource-manager-template-walkthrough.md), která dávku řídí a deleguje zřizování každé databáze na propojenou šablonu. Tento způsob použití šablon umožňuje Azure Resource Manageru zprostředkovat skriptu proces zřizování. Šablony zřizují databáze souběžně na možných místech. V případě potřeby ošetří opakované pokusy a optimalizují celkový proces. Skript je idempotent, pokud selže nebo zastaví z jakéhokoli důvodu, potom ho spusťte znovu.

### <a name="verify-the-batch-of-tenants-successfully-deployed"></a>Ověření úspěšného nasazení dávky tenantů

* V [portál Azure](https://portal.azure.com), přejděte do seznamu serverů a otevřete *tenants1* serveru. Klikněte na tlačítko **databází SQL** a zkontrolujte dávku 17 další databáze, jsou nyní v seznamu:

   ![seznam databází](media/saas-dbpertenant-provision-and-catalog/database-list.png)



## <a name="other-provisioning-patterns"></a>Další způsoby zřizování

K dalším způsobům zřizování, které nejsou zahrnuty do tohoto kurzu, patří:

**Předběžné zřizování databází.** Předem zřizování vzor zneužije skutečnost, že databáze v elastickém fondu nepřidávejte zpoplatněné. Fakturuje se pro elastický fond není databáze, a nečinnosti databáze využívat žádné prostředky. Předem zřizování databází ve fondu a přidělování je podle potřeby, může výrazně snížit čas registrace klienta. Počet databází předem zřizovat lze upravit podle potřeby zachovat vhodný pro předpokládaného zřizování rychlost vyrovnávací paměti.

**Automatické zřizování.** Ve vzoru automatické zřizování zřizování služby zřídí servery, fondy a databáze automaticky podle potřeby – včetně předem zřizování databází v elastické fondy, pokud potřeby. A pokud jsou databáze zrušte uvedena do provozu a odstranit, mezery v elastické fondy může být vyplněny zřizování služby. Tato služba může být jednoduché nebo komplexní – například zpracování zřizování napříč několika zeměpisných oblastí a může nastavit geografická replikace pro zotavení po havárii. Pomocí vzoru automatické zřizování klientská aplikace nebo skriptu odešle žádost o zřizování do fronty ke zpracování pomocí služby zřizování a pak cyklicky dotazuje službu, kterou chcete určit dokončení. Pokud se používá předem zřizování, by požadavky zpracovávány rychle službou zřizování nahrazení databáze na pozadí.



## <a name="next-steps"></a>Další kroky

V tomto kurzu jste se naučili:

> [!div class="checklist"]

> * Zřídit jednoho nového tenanta.
> * Zřídit dávku dalších tenantů.
> * Krok na podrobné informace o zřizování klientů a jejich registrace do katalogu

Zkuste [kurzu monitorování výkonu](saas-dbpertenant-performance-monitoring.md).

## <a name="additional-resources"></a>Další zdroje

* Další [návodů, které stavět na adresář Wingtip lístky SaaS databáze na aplikaci klienta](saas-dbpertenant-wingtip-app-overview.md#sql-database-wingtip-saas-tutorials)
* [Klientská knihovna Elastic Database](sql-database-elastic-database-client-library.md)
* [Ladění skriptů v integrovaném skriptovacím prostředí (ISE) v prostředí Windows PowerShell](https://msdn.microsoft.com/powershell/scripting/core-powershell/ise/how-to-debug-scripts-in-windows-powershell-ise)
