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
ms.openlocfilehash: eda330a7202de8a325d645b37a0d05ef8df8985b
ms.sourcegitcommit: e5355615d11d69fc8d3101ca97067b3ebb3a45ef
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/31/2017
---
# <a name="learn-how-to-provision-and-catalog-new-tenants-and-register-them-in-the-catalog"></a>Zjistěte, jak zřídit a nové klienty katalogu a zaregistrujte je v katalogu

V tomto kurzu můžete další informace o zřizování a katalog vzory SaaS a jak jsou implementované v aplikaci Wingtip SaaS. Vytvoření a inicializace nové databáze klienta a zaregistrujte je v katalogu klienta aplikace. Katalog je databáze, která zajišťuje mapování mezi aplikace SaaS velký počet klientů a jejich data. Katalog hraje důležitou roli odkazovat žádosti o aplikace ke správné databázi.  

V tomto kurzu se naučíte:

> [!div class="checklist"]

> * Zřídit jednoho nového klienta, včetně procházení jak tato možnost je implementovaná
> * Zřídit dávku dalších tenantů.


Předpokladem dokončení tohoto kurzu je splnění následujících požadavků:

* Adresář Wingtip SaaS aplikace je nasazená. Nasazení za méně než pět minut najdete v tématu [nasazení a seznamte se s Wingtip SaaS aplikace](sql-database-saas-tutorial.md)
* Je nainstalované prostředí Azure PowerShell. Podrobnosti najdete v článku [Začínáme s prostředím Azure PowerShell](https://docs.microsoft.com/powershell/azure/get-started-azureps).

## <a name="introduction-to-the-saas-catalog-pattern"></a>Úvod do katalogu SaaS

V databázi zálohovanou víceklientské aplikaci SaaS je důležité vědět, se uloží informace pro každého klienta. Ve vzoru katalogu SaaS, je databáze katalogu používané pro udržení mapování mezi každého klienta a kde jsou uchovávána data. Aplikace Wingtip SaaS používá jednoho klienta za architektura databáze, ale základní vzor ukládání mapování klienta do databáze v katalogu platí, jestli se používá databázi víceklientské nebo jednoho klienta.

Každý klient je přiřazen klíč, který označuje je v katalogu a která se mapuje na umístění příslušné databáze. V aplikaci Wingtip SaaS klíč je vytvořen z hodnota hash název klienta. To umožňuje název část adresy URL aplikace, který se má použít k vytvoření klíče klienta. Může použít jiná schémata klíče klienta.  

Katalogu umožňuje název nebo umístění databáze se musí změnit s minimálním dopadem na aplikaci.  V databázi víceklientského modelu to rovněž umožňuje, 'Přesun' klienta mezi databázemi.  Katalog lze také označuje, zda klient nebo databáze do režimu offline za účelem údržby nebo jiné akce. To je prozkoumali v [obnovení jednoho klienta kurzu](sql-database-saas-tutorial-restore-single-tenant.md).

Kromě toho katalog, který je v platnosti databáze správy pro aplikace SaaS, můžete ukládat další metadata klienta nebo databáze, například vrstvy nebo edici databáze, verzi schématu, plán služeb nebo SLA nabízených klientům a dalších informací, které umožňuje správu aplikací, na zákaznickou podporu nebo devops procesy.  

Nad rámec aplikace SaaS můžete povolit katalogu Nástroje databáze.  V ukázce Wingtip SaaS katalogu slouží k povolení klienta mezi dotaz, prozkoumali v [ad hoc analytics kurzu](sql-database-saas-tutorial-adhoc-analytics.md). Správa úloh mezidatabázové je prozkoumali v [Správa schématu](sql-database-saas-tutorial-schema-management.md) a [klienta analytics](sql-database-saas-tutorial-tenant-analytics.md) kurzy. 

V aplikaci Wingtip SaaS katalogu je implementovaná pomocí funkcím pro správu horizontálního oddílu [elastické databáze klienta knihovny (EDCL)](sql-database-elastic-database-client-library.md). EDCL umožňuje aplikaci vytvářet, spravovat a použít mapování horizontálních databáze zálohována. Mapování horizontálních obsahuje seznam horizontálních oddílů (databáze) a mapování mezi klíče (klienty) a databázemi.  EDCL funkcí lze z aplikace nebo skripty prostředí PowerShell během zřizování vytvořit položky v mapě horizontálního oddílu klienta a z aplikace pro efektivní připojení k databázi správné. EDCL ukládá do mezipaměti informace o připojení a minimalizovat provoz do databáze katalogu zrychlit aplikace.  

> [!IMPORTANT]
> Data o mapování jsou přístupná v katalogové databázi, ale *neupravujte je*! K jejich úpravě používejte jen rozhraní API klientské knihovny Elastic Database. Pokud budete manipulovat přímo s daty mapování, riskujete poškození katalogu, protože takový postup není podporovaný.


## <a name="introduction-to-the-saas-provisioning-pattern"></a>Úvod do vzoru zřizování SaaS

Při připojování nového klienta v aplikaci SaaS, která používá model databáze jednoho klienta novou databázi klienta musí být zřízená.  Musí být vytvořený v příslušné umístění a úroveň služby, inicializovat pomocí příslušného schématu a referenčních dat a poté zaregistrovány v katalogu pod klíčem odpovídající klienta.  

Různý přístup umožňuje databáze zřizování, které můžou zahrnovat provádění skripty SQL, nasazení souboru bacpac nebo kopírování databáze "zlatá" šablony.  

Zřizování přístupů, které používáte musí comprehended součástí strategie pro správu celkové schéma, který musí zajistit, že nové databáze zřízený s nejnovější schématu.  To je prozkoumali v [schématu správu kurzu](sql-database-saas-tutorial-schema-management.md).  

Adresář Wingtip SaaS aplikace zřizuje nové klienty tak, že zkopírujete zlaté databáze s názvem basetenantdb, nasadit na server katalogu.  Zřizování může být integrovaná do aplikace jako součást registrace prostředí a podporované v režimu offline pomocí skriptů. V tomto kurzu bude prozkoumejte zřizování pomocí prostředí PowerShell. Zřizování skripty zkopírujte basetenantdb vytvořit novou databázi klienta v elastickém fondu, pak provést jeho inicializaci s informacemi o konkrétního klienta a její registrace v mapě horizontálního oddílu katalogu.  V ukázkové aplikace, databáze mají názvy na základě názvu klienta, ale nejedná se o důležitou součástí vzoru – použití katalogu umožňuje libovolný název a přiřadit k databázi. + 


## <a name="get-the-wingtip-application-scripts"></a>Získání skriptů aplikace Wingtip

Adresář Wingtip SaaS skripty a zdrojový kód aplikace, které jsou k dispozici v [WingtipSaaS](https://github.com/Microsoft/WingtipSaaS) úložiště github. [Postup stažení skripty Wingtip SaaS](sql-database-wtp-overview.md#download-and-unblock-the-wingtip-saas-scripts).


## <a name="provision-and-catalog-detailed-walkthrough"></a>Podrobný postup zřizování a přidání tenantů do katalogu

Abyste pochopili, jak aplikace Wingtip implementuje nového klienta zřizování, přidejte zarážek a krok v pracovním postupu při zřizování klienta:

1. Otevřete... \\Learning moduly\\ProvisionAndCatalog\\_ukázku ProvisionAndCatalog.ps1_ a nastavit následující parametry:
   * **$TenantName** = název nového místa (například *Bushwillow Blues*).
   * **$VenueType** = jeden z typů předdefinovaného místo: *modré*, classicalmusic, tance, jazz, judo, motorracing víceúčelových, opera, rockmusic, fotbalový.
   * **$DemoScenario** = **1**, nastavte na **1** k *zřídit jednoho klienta*.

1. Přidejte zarážku umístěním kurzoru kdekoli v řádku 48, řádek, která uvádí, že: *nového klienta,*a stiskněte klávesu **F9**.

   ![přerušení](media/sql-database-saas-tutorial-provision-and-catalog/breakpoint.png)

1. Ke spuštění skriptu stiskněte **F5**.

1. Po spuštění skriptu se zastaví u zarážky, stiskněte klávesu **F11** ke kroku do kódu.

   ![přerušení](media/sql-database-saas-tutorial-provision-and-catalog/debug.png)



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

Název databáze je sestavený z názvu tenanta, aby bylo jasné, jakému tenantovi příslušný horizontální oddíl patří. (Další strategie pro pojmenování databáze by mohl snadno použít.) + Šablony Resource Manageru se používá k vytvoření databáze klienta zkopírováním zlaté databáze (baseTenantDB) na serveru katalogu. Alternativní způsob může být vytvořit prázdnou databázi a potom inicializujte importováním souboru bacpac, nebo pro spuštění skriptu inicializaci ze známých umístění.  

Šablona Resource Manager je ve složce Modules\Common\ ...\Learning: *tenantdatabasecopytemplate.json*

Po vytvoření databáze klienta je pak další **inicializován s názvem místo (klientů) a typ místo**. Zde také můžete provést další inicializaci.

**Databáze klienta je zaregistrován v katalogu** s *přidat TenantDatabaseToCatalog* pomocí klíče klienta. Ke vstupu do podrobností použijte funkční klávesu **F11**.

* Databáze katalogu se přidá do mapy horizontálního dělení (tj. do seznamu známých databází).
* Vytvoří se mapování, které propojí hodnotu klíče s horizontálním oddílem.
* Další metadata (název místo) o klienta se přidá do tabulky klienty v katalogu.  Tabulka klienty není součástí ShardManagement schéma a není nainstalovaná v rámci EDCL.  Tato tabulka ukazuje, jak může být databáze katalogu rozšířené k podpoře další data specifické pro aplikaci.   


Po zřízení dokončení zpracování vrátí do původního *ukázku ProvisionAndCatalog* skript, který se otevře **události** stránky pro nového klienta v prohlížeči:

   ![stránka events](media/sql-database-saas-tutorial-provision-and-catalog/new-tenant.png)


## <a name="provision-a-batch-of-tenants"></a>Zřídit dávky klientů

Toto cvičení zřídí dávky 17 klientů. Doporučuje se, že zřídit tuto dávku klienty před zahájením dalších kurzech Wingtip SaaS, je více než několik databází pro práci s.

1. Otevřete... \\Learning moduly\\ProvisionAndCatalog\\*ukázku ProvisionAndCatalog.ps1* v *prostředí PowerShell ISE* a změňte *$ DemoScenario* parametru 3:
   * **$DemoScenario** = **3**, změňte **3** k *zřídit dávky klienty*.
1. Stiskněte **F5** a spusťte skript.

Skript nasadí dávku dalších tenantů. Použije [šablonu Azure Resource Manageru](../azure-resource-manager/resource-manager-template-walkthrough.md), která dávku řídí a deleguje zřizování každé databáze na propojenou šablonu. Tento způsob použití šablon umožňuje Azure Resource Manageru zprostředkovat skriptu proces zřizování. Šablony zřizují databáze souběžně na možných místech. V případě potřeby ošetří opakované pokusy a optimalizují celkový proces. Skript je idempotent, pokud selže nebo zastaví z jakéhokoli důvodu, potom ho spusťte znovu.

### <a name="verify-the-batch-of-tenants-successfully-deployed"></a>Ověření úspěšného nasazení dávky tenantů

* Otevřete *tenants1* server tak, že přejde do seznamu serverů v [portál Azure](https://portal.azure.com), klikněte na tlačítko **databází SQL**a ověřte dávku 17 další databáze jsou teď ve seznam:

   ![seznam databází](media/sql-database-saas-tutorial-provision-and-catalog/database-list.png)



## <a name="other-provisioning-patterns"></a>Další způsoby zřizování

K dalším způsobům zřizování, které nejsou zahrnuty do tohoto kurzu, patří:

**Předběžné zřizování databází.** Předem zřizování vzor zneužije skutečnost, že databáze v elastickém fondu nepřidávejte zpoplatněné. Fakturuje se pro elastický fond není databáze, a nečinnosti databáze využívat žádné prostředky. Předem zřizování databází ve fondu a přidělování je podle potřeby, může výrazně snížit čas registrace klienta. Počet předběžně zřizovaných databází můžete podle potřeby upravit, abyste měli dostatečnou rezervu, která odpovídá předpokládanému tempu zřizování tenantů.

**Automatické zřizování.** Ve vzoru automatické zřizování vyhrazené zřizování služby slouží ke zřízení servery, fondy a databáze automaticky podle potřeby – včetně předem zřizování databází v elastické fondy, v případě potřeby. Pokud jsou databáze stažené z provozu a odstraněné, může mezery v elastických fondech podle potřeby zaplnit zřizovací služba. Tato služba může být jednoduché nebo komplexní – například zpracování zřizování napříč několika zeměpisných oblastí a může nastavit geografická replikace automaticky pokud této strategie se používá pro zotavení po havárii. Při automatickém zřizování odešle klientská aplikace nebo skript žádost o zřízení do fronty, kterou zpracovává zřizovací služba, a pak se této služby zeptá na dokončení. Při použití předběžného zřizování požadavky rychle zpracovává služba, která spravuje zřizování náhradní databáze spuštěné na pozadí.



## <a name="next-steps"></a>Další kroky

V tomto kurzu jste se naučili:

> [!div class="checklist"]

> * Zřídit jednoho nového tenanta.
> * Zřídit dávku dalších tenantů.
> * Krok na podrobné informace o zřizování klientů a jejich registrace do katalogu

Zkuste [kurzu monitorování výkonu](sql-database-saas-tutorial-performance-monitoring.md).

## <a name="additional-resources"></a>Další zdroje

* Další [návodů, které stavět na adresář Wingtip SaaS aplikace](sql-database-wtp-overview.md#sql-database-wingtip-saas-tutorials)
* [Klientská knihovna Elastic Database](https://docs.microsoft.com/azure/sql-database/sql-database-elastic-database-client-library)
* [Ladění skriptů v integrovaném skriptovacím prostředí (ISE) v prostředí Windows PowerShell](https://msdn.microsoft.com/powershell/scripting/core-powershell/ise/how-to-debug-scripts-in-windows-powershell-ise)
