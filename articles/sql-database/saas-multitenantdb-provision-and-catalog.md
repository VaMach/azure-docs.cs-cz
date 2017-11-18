---
title: "Zřídit a nové klienty v aplikaci SaaS pomocí databáze Azure SQL Database SQL víceklientské katalogu | Microsoft Docs"
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
ms.date: 11/17/2017
ms.author: billgib
ms.openlocfilehash: f6707b85cc80178da663d7e2b95eeb5c9550789c
ms.sourcegitcommit: 933af6219266cc685d0c9009f533ca1be03aa5e9
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/18/2017
---
# <a name="provision-and-catalog-new-tenants-in-a-saas-application-using-a-multi-tenant-sql-database"></a>Zřizování a katalog novým tenantům v aplikaci SaaS použití databáze systému SQL více klientů

V tomto kurzu informace o vzory pro zřizování a katalogizaci klienty při práci s modelem horizontálně dělené víceklientské databáze. 

Schéma víceklientské umožňuje data z více klientů k uložení do jedné databáze. K podpoře velkého počtu klientů, distribuci dat klienta mezi víc horizontálních oddílů nebo databáze. Data pro všechny jednoho klienta je vždy zcela zahrnutá v jedné databáze.  Katalog se používá k ukládání mapování klientům databáze.   

Můžete také naplnit některé databáze s pouze jednoho klienta. Databáze, které mají víc klientů upřednostnit s nižšími náklady na každého klienta za cenu izolaci klientů.  Databáze, které obsahují pouze jednoho klienta upřednostnit izolace nad náklady.  Databáze s více klienty a jeden klienty můžete kombinovat v jedné aplikace SaaS optimalizovat náklady nebo izolace pro každého klienta. Klienty můžete mít vlastní databáze při zřízení nebo lze přesunout do své vlastní databáze později.

   ![Horizontálně dělené databáze víceklientské aplikace pomocí katalogu klienta](media/saas-multitenantdb-provision-and-catalog/MultiTenantCatalog.png)


## <a name="tenant-catalog-pattern"></a>Vzor katalogu klienta

Pokud klient dat je distribuován do více databází, je důležité vědět, které jsou uložená data každého klienta. Databáze katalogu obsahuje mapování mezi klientem a databáze, která ukládá data.

Každý klient je identifikována klíč v katalogu. V aplikacích Wingtip lístky SaaS je klíč tenanta vytvořen z hodnota hash název klienta. To umožňuje aplikaci rozbalte název klienta z adresu URL webové stránky a použijte ji pro připojení k příslušné databázi. Můžete také použít jiná schémata klíče klienta.

Použití katalog umožňuje název nebo umístění databáze klienta se musí změnit po zřízení bez přerušení aplikace.  V databázi víceklientského modelu toto nastavení, odpovídá přesun klienta mezi databází.  Katalogu mohou sloužit také k aplikaci indikující, zda klient je offline z důvodu údržby nebo jiné akce.

Katalog lze rozšířit ukládat další metadata klienta nebo databáze, jako je například vrstvy nebo edici databáze, verze schématu databáze, název klienta a plán služeb nebo SLA a další informace můžete povolit správu aplikací, zákaznickou podporu nebo devops procesy.  

Katalog lze použít také k povolení správy schématu sestav, mezi klienta a extrahování dat pro účely analýzy. 

### <a name="elastic-database-client-library"></a>Klientská knihovna Elastic Database 
V aplikacích Wingtip lístky SaaS, katalogu je implementována ve *tenantcatalog* databáze pomocí funkcím pro správu horizontálního oddílu [elastické databáze klienta knihovny (EDCL)](sql-database-elastic-database-client-library.md). Umožňuje aplikaci vytvářet, spravovat a použít databázi zálohovanou knihovny 'horizontálního oddílu mapy'. Mapování horizontálních obsahuje seznam horizontálních oddílů (databáze) a mapování mezi klíče (klienty) a horizontálních oddílů.  Funkce EDCL můžete použít z aplikace nebo skripty prostředí PowerShell během klienta zřizování vytvořit položky v mapě horizontálního oddílu a novějším, pro připojení k databázi správné. Knihovny ukládá do mezipaměti informace o připojení a minimalizovat provoz na databázi katalogu urychlení připojení. 

> [!IMPORTANT]
> Data o mapování jsou přístupná v katalogové databázi, ale *neupravujte je*! K jejich úpravě používejte jen rozhraní API klientské knihovny Elastic Database. Pokud budete manipulovat přímo s daty mapování, riskujete poškození katalogu, protože takový postup není podporovaný.


## <a name="tenant-provisioning-pattern"></a>Vzor zřizování klienta

Při zřizování nového klienta v horizontálně dělené databáze víceklientského modelu, musí nejprve být určen Pokud klienta je možné zřídit do sdílené databáze nebo zadané svou vlastní databázi. Pokud sdílenou databázi, je třeba stanovit, pokud je místa v existující databázi nebo je potřeba novou databázi. V případě potřeby novou databázi musí být zřízená v příslušné umístění a úroveň služby, inicializován s odpovídající schématu a referenčních dat a poté zaregistrovány v katalogu. Nakonec klient mapování lze přidat tak, aby odkazovaly odpovídající ID horizontálního oddílu.

Zřizování databáze lze dosáhnout provádění skripty SQL, nasazení souboru bacpac nebo kopírování šablony databáze. Aplikace SaaS lístky Wingtip zkopírujte databázi šablony k vytvoření nové databáze klienta.

Databázi zřizování přístupu musí být comprehended v celkové strategie správy schématu, která potřebuje, aby se nejnovější schématem jsou zřizovat nové databáze.  To je prozkoumali další v [schématu správu kurzu](saas-tenancy-schema-management.md).  

Zřizování klienta skripty v tomto kurzu obsahovat zřizování klienta do existující databáze víceklientské i vytvoření nové databáze klienta. Klienta data se pak inicializovat a pak zaregistrována v mapě horizontálního oddílu katalogu. V ukázkové aplikace databáze, které obsahují jednoho klienta se zadaným názvy na základě názvu klienta. Databáze, které obsahují víc klientů mají obecný _tenantsN_ název při databáze s pouze jedním klienta jsou uvedeny název klienta. Konkrétní zásady vytváření názvů používá v ukázce nejsou důležitou součástí vzor, podle použití katalogu umožňuje libovolný název a přiřadit k databázi.  

## <a name="provision-and-catalog-tutorial"></a>Zřizování a katalog kurzu

V tomto kurzu se naučíte:

> [!div class="checklist"]

> * Přidělení klienta do databáze více klientů
> * Přidělení klienta do jednoho klienta databáze
> * Zřídit dávky klienty do databáze více klientů a jednoho klienta
> * Registrovat databáze a klienta mapování v katalogu

Předpokladem dokončení tohoto kurzu je splnění následujících požadavků:

* Nasazení aplikace Wingtip lístky SaaS víceklientské databáze. Nasazení za méně než pět minut najdete v tématu [nasazení a seznamte se s aplikací Wingtip lístky SaaS víceklientské databáze](saas-multitenantdb-get-started-deploy.md)
* Je nainstalované prostředí Azure PowerShell. Podrobnosti najdete v článku [Začínáme s prostředím Azure PowerShell](https://docs.microsoft.com/powershell/azure/get-started-azureps).

## <a name="get-the-wingtip-tickets-management-scripts"></a>Získat skripty správy Wingtip lístky

Skripty pro správu a zdrojový kód aplikace, které jsou k dispozici v [WingtipTicketsSaaS MultiTenantDB](https://github.com/Microsoft/WingtipTicketsSaaS-MultiTenantDB) úložiště GitHub. <!--See [Steps to download the Wingtip SaaS scripts](saas-tenancy-wingtip-app-guidance-tips.md#download-and-unblock-the-wingtip-saas-scripts).-->


## <a name="provision-tenant-walkthrough-1"></a>Zřizování klienta návod #1

Abyste pochopili, jak aplikace Wingtip lístky implementuje nového klienta zřizování s více klienty databáze, přidejte zarážek a krok v pracovním postupu při zřizování klienta v databázi sdílené s jinými klienty:

1. V _prostředí PowerShell ISE_, otevřete... \\Learning moduly\\ProvisionAndCatalog\\_ukázku ProvisionAndCatalog.ps1_ a nastavit následující parametry:
   * **$TenantName** = **Bushwillow modré**, název nové místo.
   * **$VenueType** = **modré**, jeden z typů předdefinovaného místo: modré, classicalmusic, tance, jazz, judo, motorracing víceúčelových, opera, rockmusic, fotbalový (malá písmena, bez mezer).
   * **$DemoScenario** = **1**do *zřídit klienta v databázi sdílené s jinými klienty*.

1. Přidejte zarážku umístěním kurzoru kdekoli v řádku 38, řádek, která uvádí, že: *nového klienta,*a stiskněte klávesu **F9**.

   ![přerušení](media/saas-multitenantdb-provision-and-catalog/breakpoint.png)

1. Ke spuštění skriptu stiskněte **F5**.

1. Po spuštění skriptu se zastaví u zarážky, stiskněte klávesu **F11** ke kroku do kódu.

   ![Ladění](media/saas-multitenantdb-provision-and-catalog/debug.png)

Trasování spuštění skriptu na pomocí **ladění** možnosti nabídky - **F10** a **F11** krok přes nebo do volaných funkcí. Další informace o ladění skriptů prostředí PowerShell najdete v tématu [tipy k ladění skriptů prostředí PowerShell a práce s](https://msdn.microsoft.com/powershell/scripting/core-powershell/ise/how-to-debug-scripts-in-windows-powershell-ise).


Toto jsou klíčové prvky, které jednotlivé kroky při trasování skriptu pracovního postupu:

* **Vypočítejte klíč nového tenanta**. K vytvoření klíče tenanta z jeho jména se používá funkce hash.
* **Zkontrolujte, jestli klíč tenanta ještě neexistuje**. Katalog je zkontrolovat, že klíč již zaregistrován.
* **Inicializace klienta v databázi klienta výchozí**. Databáze klienta se aktualizuje a přidat nové informace o klientovi.  
* **Registrace klienta v katalogu** mapování mezi nový klíč klienta a stávající databázi tenants1 je přidaných do katalogu. 
* **Název klienta se přidá do katalogu**. Název místo je přidat do tabulky klienty v katalogu.  Ukazuje to, jak může být databáze katalogu rozšířené k podpoře další data specifické pro aplikaci.
* **Otevřete stránku události pro nového klienta**. *Bushwillow modré* události stránka se otevře v prohlížeči:

   ![stránka events](media/saas-multitenantdb-provision-and-catalog/bushwillow.png)


## <a name="provision-tenant-walkthrough-2"></a>Zřizování klienta návod #2

Nyní návod proces, při vytváření klienta ve vlastní databázi:

1. Pořád ještě v... \\Learning moduly\\ProvisionAndCatalog\\_ukázku ProvisionAndCatalog.ps1_ nastavit následující parametry:
   * **$TenantName** = **sequoia fotbalový**, název nové místo.
   * **$VenueType** = **fotbalový**, jeden z typů předdefinovaného místo: modré, classicalmusic, tance, jazz, judo, motorracing víceúčelových, opera, rockmusic, fotbalový (malá písmena, bez mezer).
   * **$DemoScenario** = **2**do *zřídit klienta v databázi sdílené s jinými klienty*.

1. Přidejte novou zarážku umístěním kurzoru kdekoli v řádku 57, řádek, která uvádí, že:  *& &nbsp;$PSScriptRoot\New-TenantAndDatabase '*a stiskněte klávesu **F9**.

   ![přerušení](media/saas-multitenantdb-provision-and-catalog/breakpoint2.png)

1. Stisknutím klávesy **F5** spusťte skript.

1. Po spuštění skriptu se zastaví u zarážky, stiskněte klávesu **F11** ke kroku do kódu.  Použití **F10** a **F11** krok přes a zanořte se do funkce pro sledování spuštění.

Toto jsou klíčové prvky, které jednotlivé kroky při trasování skriptu pracovního postupu:

* **Vypočítejte klíč nového tenanta**. K vytvoření klíče tenanta z jeho jména se používá funkce hash.
* **Zkontrolujte, jestli klíč tenanta ještě neexistuje**. Katalog je zkontrolovat, že klíč již zaregistrován.
* **Vytvoření nové databáze klienta**. Vytvoření databáze tak, že zkopírujete *basetenantdb* databáze pomocí šablony Resource Manageru.  Název databáze je založen na názvu klienta.
* **Přidat databáze do katalogu**. Databáze klienta se zaregistruje jako horizontálního oddílu v katalogu.
* **Inicializace klienta v databázi klienta výchozí**. Databáze klienta se aktualizuje a přidat nové informace o klientovi.  
* **Registrace klienta v katalogu** mapování mezi nový klíč klienta a *sequoiasoccer* databáze je přidaných do katalogu.
* **Název klienta se přidá do katalogu**. Název místo je přidat do tabulky klienty v katalogu.
* **Otevřete stránku události pro nového klienta**. *Sequoia fotbalový* události stránka se otevře v prohlížeči:

   ![stránka events](media/saas-multitenantdb-provision-and-catalog/sequoiasoccer.png)


## <a name="provision-a-batch-of-tenants"></a>Zřídit dávky klientů

Toto cvičení rychle zřídí dávky 17 klientů. Doporučuje se, že zřídit tuto dávku klienty před zahájením dalších kurzech Wingtip lístků, je více než několik databází pro práci s.

1. V *prostředí PowerShell ISE*, otevřete... \\Learning moduly\\ProvisionAndCatalog\\*ukázku ProvisionAndCatalog.ps1* a změňte *$DemoScenario* parametru 3:
   * **$DemoScenario** = **3**do *zřídit dávky klienty do sdílené databáze*.
1. Stiskněte **F5** a spusťte skript.


### <a name="verify-the-deployed-set-of-tenants"></a>Ověřte sadu nasazených klientů 
V této fázi mít směs klienty nasadit do sdílené databáze a klienty nasazený do vlastní databáze. Portál Azure slouží ke kontrole databáze vytvořené:  

* V [portál Azure](https://portal.azure.com), otevřete **tenants1-mt -\<uživatele\>**  procházením seznamu serverů SQL server.  **Databází SQL** seznamu by měla obsahovat sdílený **tenants1** databáze a databáze pro klienty, kteří jsou v jejich vlastní databázi:

   ![seznam databází](media/saas-multitenantdb-provision-and-catalog/databases.png)

A portálu Azure se zobrazí klienta databáze, ho můžete Podíváme se, klienti uvnitř sdílenou databázi. Úplný seznam klientů, které si můžete prohlédnout ve stránce centra událostí Wingtip lístků.   

* Otevřete Centrum událostí stránku v prohlížeči (http:events.wingtip-mt.\<uživatele\>. trafficmanager.net)  

Úplný seznam klientů a jejich odpovídající databáze je k dispozici v katalogu. Zobrazení SQL najdete v databázi tenantcatalog, které připojí název klienta, které jsou uložené v tabulce klienty k názvu databáze v tabulkách správu horizontálního oddílu. Toto zobrazení ukazuje vhodně hodnotu rozšíření metadat uložené v katalogu.

* V *SQL Server Management Studio (SSMS)* připojit k serveru klienty na **tenants1 strojový překladů.\<uživatele\>. database.windows.net**, s přihlášením: **vývojáře** , Heslo:**P@ssword1**

    ![Dialogové okno připojení aplikace SSMS](media/saas-multitenantdb-provision-and-catalog/SSMSConnection.png)

* V *Průzkumník objektů*, přejděte do zobrazení *tenantcatalog* databáze.
* Klikněte pravým tlačítkem na *ExtendedTenants* a zvolte **řádky vyberte Top 1000** a poznamenejte si mapování mezi název klienta a databáze pro jiné klienty.

    ![Zobrazení ExtendedTenants v aplikaci SSMS](media/saas-multitenantdb-provision-and-catalog/extendedtenantsview.png)
      
## <a name="other-provisioning-patterns"></a>Další způsoby zřizování

K dalším způsobům zřizování, které nejsou zahrnuty do tohoto kurzu, patří:

**Předem zřizování databází v elastické fondy.** Předem zřizování vzor zneužije skutečnost, že databáze v elastickém fondu nepřidávejte zpoplatněné. Fakturuje se pro elastický fond není databáze, a nečinnosti databáze využívat žádné prostředky. Předem zřizování databází ve fondu a přidělování je podle potřeby, může výrazně snížit čas potřebný k zařadit klienty do své vlastní databáze. Počet předběžně zřizovaných databází můžete podle potřeby upravit, abyste měli dostatečnou rezervu, která odpovídá předpokládanému tempu zřizování tenantů.

**Automatické zřizování.** Ve vzoru automatické zřizování vyhrazené zřizování služby slouží ke zřízení servery, fondy a databáze automaticky podle potřeby – včetně předem zřizování databází v elastické fondy, v případě potřeby. Pokud jsou databáze stažené z provozu a odstraněné, může mezery v elastických fondech podle potřeby zaplnit zřizovací služba. Tato služba může být jednoduché nebo komplexní – například zpracování zřizování napříč několika zeměpisných oblastí a může nastavit geografická replikace pro zotavení po havárii. Pomocí vzoru automatické zřizování klientská aplikace nebo skriptu by odeslat žádost o zřizování do fronty ke zpracování zřizování službou a by pak dotazování k určení dokončení. Pokud se používá předem zřizování, by požadavky zpracovávány rychle se službou Správa zřizování nahrazení databáze spuštěná na pozadí.



## <a name="next-steps"></a>Další kroky

V tomto kurzu jste se naučili:

> [!div class="checklist"]

> * Zřídit jednoho nového tenanta.
> * Zřídit dávku dalších tenantů.
> * Krok na podrobné informace o zřizování klientů a jejich registrace do katalogu

Zkuste [kurzu monitorování výkonu](saas-multitenantdb-performance-monitoring.md).

## <a name="additional-resources"></a>Další zdroje

<!--* Additional [tutorials that build upon the Wingtip SaaS application](saas-dbpertenant-wingtip-app-overview.md#sql-database-wingtip-saas-tutorials)-->
* [Klientská knihovna Elastic Database](sql-database-elastic-database-client-library.md)
* [Ladění skriptů v integrovaném skriptovacím prostředí (ISE) v prostředí Windows PowerShell](https://msdn.microsoft.com/powershell/scripting/core-powershell/ise/how-to-debug-scripts-in-windows-powershell-ise)
