---
title: "Nasazení a zkoumání SaaS aplikace využívající službu Azure SQL Database | Dokumentace Microsoftu"
description: "Nasazení a prozkoumání ukázkové aplikace Wingtip Tickets (WTP) využívající službu Azure SQL Database"
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
ms.openlocfilehash: 04859fb8a9f4a8bf04d92e7139d9202885c52503
ms.contentlocale: cs-cz
ms.lasthandoff: 05/12/2017


---
# <a name="deploy-and-explore-a-multi-tenant-saas-application-that-uses-azure-sql-database"></a>Nasazení a zkoumání SaaS aplikace s více tenanty využívající službu Azure SQL Database

V tomto kurzu nasadíte a prozkoumáte SaaS aplikaci WTP (Wingtip Tickets Platform) a seznámíte se s ní. Aplikace používá k poskytování služeb více tenantům strukturu SaaS aplikací, tj. s jednou databází pro každého tenanta. Aplikace je navržená tak, aby ukazovala funkce služby Azure SQL Database, které umožňují scénáře SaaS, a vzorce návrhu a správy SaaS aplikací.

Pět minut po kliknutí na tlačítko *Nasadit do Azure* zobrazené níže budete mít k dispozici SaaS aplikaci pro více klientů využívající službu SQL Database, která bude běžet v cloudu. Aplikace se nasazuje se třemi ukázkovými klienty, přičemž každý má vlastní databází a všichni jsou nasazení do elastického fondu SQL. Aplikace se nasazuje do vašeho předplatného Azure, díky čemuž máte úplný přístup ke kontrole jednotlivých komponent aplikace a práci s nimi.

Skripty a zdrojový kód aplikace jsou k dispozici v úložišti GitHubu [WingtipSaaS](https://github.com/Microsoft/WingtipSaaS).

V tomto kurzu se dozvíte:

> [!div class="checklist"]

> * Jak nasadit aplikaci WTP
> * O serverech, fondech a databázích, které aplikaci tvoří
> * Tenanti jsou namapovaní na svá data pomocí *katalogu*
> * Jak zřídit nové tenanty
> * Jak zobrazit využití fondu k monitorování aktivity tenanta
> * Jak odstranit ukázkové prostředky k zastavení souvisejícího účtování

K prozkoumání různých vzorů návrhu a správy SaaS aplikace máte k dispozici [řadu souvisejících kurzů](sql-database-wtp-overview.md), které vycházejí z tohoto počátečního nasazení. Při procházení těmito kurzy se důkladně podívejte na uvedené skripty a vyzkoušejte si, jak jsou implementované různé vzory SaaS. Projděte si krok po kroku skripty v každém kurzu, abyste detailně porozuměli způsobu implementace mnoha funkcí služby SQL Database, které zjednodušují vývoj SaaS aplikací.

Předpokladem dokončení tohoto kurzu je splnění následujících požadavků:

* Prostředí Azure PowerShell je nainstalované. Podrobnosti najdete v článku [Začínáme s prostředím Azure PowerShell](https://docs.microsoft.com/powershell/azure/get-started-azureps).

## <a name="deploy-the-wingtip-tickets-wtp-saas-application"></a>Nasazení SaaS aplikace WTP (Wingtip Tickets Platform)

Nasazení aplikace Wingtip Tickets Platform za méně než pět minut:

1. Klikněte pro nasazení:

   <a href="http://aka.ms/deploywtpapp" target="_blank"><img src="http://azuredeploy.net/deploybutton.png"/></a>

1. Zadejte hodnoty parametrů požadované pro vaše nasazení:

    > [!IMPORTANT]
    > Některá ověřování a brány firewall serveru jsou pro demonstrační účely záměrně nezabezpečené. **Vytvořte si novou skupinu prostředků** a nepoužívejte stávající skupiny prostředků, servery ani fondy a nepoužívejte tuto aplikaci ani žádné prostředky, které vytvoří, v produkčním prostředí. Po ukončení práce s aplikací tuto skupinu prostředků odstraňte, aby se zastavilo související účtování.

    * **Skupina prostředků** – Vyberte **Vytvořit novou** a zadejte **Název** a **Umístění**.
    * **Uživatel** – Některé prostředky vyžadují názvy, které jsou jedinečné v rámci všech předplatných Azure. Abyste tuto jedinečnost zajistili, zadejte hodnotu odlišující prostředky, které vytvoříte, od prostředků vytvořených jinými uživateli nasazujícími aplikaci Wingtip. Doporučuje se použít krátké **uživatel**ské jméno, třeba vaše iniciály, a číslo (například *bg1*) a to potom používat v názvu skupiny prostředků (například *wingtip-bg1*). Parametr **uživatel** může obsahovat jenom písmena, čísla a spojovníky. První a poslední znak musí být písmeno nebo číslo (doporučuje se používat všechna písmena malá).

     ![šablona](./media/sql-database-saas-tutorial/template.png)

1. **Nasaďte aplikaci**.

    * Pokud souhlasíte s podmínkami a ujednáními, zaškrtněte příslušné políčko.
    * Zaškrtněte **Připnout na řídicí panel**.
    * Klikněte na **Koupit**.

1. Kliknutím na **Oznámení** (ikona zvonku napravo od vyhledávacího pole) můžete monitorovat stav nasazení. Nasazení aplikace WTP trvá přibližně čtyři minuty.

   ![nasazení bylo úspěšné](media/sql-database-saas-tutorial/succeeded.png)

## <a name="explore-the-application"></a>Zkoumání aplikace

Aplikace prezentuje místa, například koncertní sály, jazzové kluby a sportovní kluby, ve kterých se konají akce. Místa se registrují jako zákazníci (neboli tenanti) pro WTP, aby bylo možné snadno vypisovat akce a prodávat lístky. Každé místo dostane vlastní webovou aplikaci pro správu a výpis svých akcí a prodej lístků, nezávislou a izolovanou od ostatních tenantů. Na pozadí každý tenant dostane databázi SQL nasazenou do elastického fondu SQL.

Centrální **Centrum akcí** obsahuje seznam adres URL tenantů specifických pro vaše nasazení. Všechny adresy URL tenantů zahrnují konkrétní hodnotu *uživatel* a mají tento formát: http://events.wtp.&lt;UŽIVATEL&gt;.trafficmanager.net/*fabrikamjazzclub*. 

1. Otevřete _Centrum akcí_: http://events.wtp.&lt;UŽIVATEL&gt;.trafficmanager.net (nahraďte názvem svého uživatele):

    ![centrum akcí](media/sql-database-saas-tutorial/events-hub.png)

1. V *Centru akcí* klikněte na **Fabrikam Jazz Club**.

   ![Akce](./media/sql-database-saas-tutorial/fabrikam.png)

1. Klikněte na **Lístky** a prozkoumejte nakupování lístků na akci.

Aplikace WTP používá k řízení distribuce příchozích přenosů [*Azure Traffic Manager*](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-overview). Stránky akcí, které jsou specifické pro každého tenanta, vyžadují, aby byly názvy tenantů součástí adres URL. Aplikace akcí analyzuje název tenanta z adresy URL a z něj vytvoří klíč pro přístup ke katalogu pomocí [*správy mapování horizontálního dělení*](https://docs.microsoft.com/azure/sql-database/sql-database-elastic-scale-shard-map-management). Katalog mapuje klíč na umístění databáze tenanta. **Centrum akcí** pomocí rozšířených metadat v katalogu načítá název klienta, který je spojený s každou databázi.

V produkčním prostředí byste obvykle vytvořili záznam CNAME DNS k [*nasměrování internetové domény společnosti*](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-point-internet-domain) na profil Traffic Manageru.

## <a name="get-the-wingtip-application-scripts"></a>Získání skriptů aplikace Wingtip

Skripty a zdrojový kód aplikace Wingtip Tickets jsou k dispozici v úložišti GitHubu [WingtipSaaS](https://github.com/Microsoft/WingtipSaaS). Soubory se skripty jsou ve [složce Learning Modules](https://github.com/Microsoft/WingtipSaaS/tree/master/Learning%20Modules). Stáhněte si složku **Learning Modules** do svého místního počítače. Dejte pozor, abyste zachovali strukturu složky.

## <a name="provision-a-new-tenant"></a>Zřízení nového tenanta

Při počátečním nasazení se vytvoří tři ukázkoví tenanti, ale aby byl efekt kurzu optimální, je potřeba jich vytvořit více. V tomto kroku rychle vytvoříte nového tenanta. Později se můžete zabývat zřizováním nových tenantů podrobněji v [kurzu zřízení nových tenantů a jejich registrace v katalogu](sql-database-saas-tutorial-provision-and-catalog.md), kde se dozvíte, jak jednoduše by šla do aplikace implementovat registrační komponenta a automaticky zřizovat tenanty při registraci zákazníků.

### <a name="initialize-the-user-config-file-for-your-deployment"></a>Inicializace uživatelského konfiguračního souboru pro nasazení

Protože se jedná o první skript, který chcete spustit po počátečním nasazení aplikace WTP, je nutné aktualizovat uživatelský konfigurační soubor. Aktualizujte proměnné o konkrétní hodnoty z vašeho nasazení.

   1. V *prostředí PowerShell ISE* otevřete ...\\Learning Modules\\*UserConfig.psm1*.
   1. Upravte _$userConfig.ResourceGroupName_ na _skupinu prostředků_, kterou jste nastavili při nasazování aplikace.
   1. Upravte _userConfig.Name_ na _uživatel_ské jméno, které jste nastavili při nasazování aplikace.

### <a name="provision-the-new-tenant"></a>Zřízení nového tenanta

1. V *prostředí PowerShell ISE* otevřete ...\\Learning Modules\Provision and Catalog\\*Demo-ProvisionAndCatalog.ps1*.
1. Stisknutím **F5** spusťte skript (zatím nechejte nastavené výchozí hodnoty skriptu).

Do katalogu se zaregistruje nový tenant. Vytvoří se jeho databáze a přidá se do elastického fondu SQL. Po úspěšném zřízení se ve vašem prohlížeči zobrazí web pro prodej lístků tenanta:

![Nový tenant](./media/sql-database-saas-tutorial/red-maple-racing.png)

Aktualizujte Centrum akcí a ověřte, že se nový tenant objevil v seznamu.

## <a name="start-generating-load-on-the-tenant-databases"></a>Spuštění generování zatížení databází tenantů

Když teď máme několik databází tenantů, můžeme je zapojit do práce. Máme tady skript PowerShellu, který simuluje úlohu spuštěnou na všech databázích tenantů. Ve výchozím nastavení zatížení běží po dobu 60 minut. Wingtip Tickets je SaaS aplikace a reálné zatížení těchto aplikací je obvykle sporadické a nepředvídatelné. Abychom takovýto scénář nasimulovali, vytváří generátor zatížení náhodné zatížení rozdělené mezi všechny tenanty. Je potřeba několik minut, aby se jeho vzor projevil, proto nechte generátor zatížení běžet asi 5 až 10 minut, než v následujících částech začnete zkoušet zatížení monitorovat.

1. V **prostředí PowerShell ISE** otevřete... \\Learning Modules\\Utilities\\*Demo-LoadGenerator.psm1*.
1. Stisknutím **F5** spusťte skript s generátorem zatížení (zatím nechejte nastavené výchozí hodnoty parametrů).

> [!IMPORTANT]
> Generátor zatížení spouští v místní relaci PowerShellu řadu úloh, proto relaci PowerShellu nezavírejte! Pokud byste to udělali nebo zavřeli kartu, ze které byl spuštěn generátor zatížení, nebo pozastavili svůj počítač, úlohy by se zastavily.


## <a name="explore-the-servers-pools-and-tenant-databases"></a>Zkoumání serverů, fondů a databází tenantů

Teď, když jste zjistili, jak aplikace funguje, zřídili nového tenanta a spustili zátěž kolekce tenantů, podíváme se na některé z nasazených prostředků.

1. Na portálu [Azure Portal](http://portal.azure.com) otevřete server **catalog-&lt;UŽIVATEL&gt;**. Na serveru katalogu jsou dvě databáze. **Tenantcatalog** a **basetenantdb** (prázdná *zlatá* databáze, která se kopíruje při vytváření nových tenantů).

   ![databáze](./media/sql-database-saas-tutorial/databases.png)

1. Otevřete server**tenants1-&lt;UŽIVATEL&gt;**, na kterém jsou databáze tenantů. Všimněte si, že všechny databáze tenantů jsou _elastické databáze úrovně Standard_ ve fondu s 50 eDTU úrovně Standard. Všimněte si také, že tady je i databáze tenanta _Red Maple Racing_, kterou jste zřídili dříve.

   ![server](./media/sql-database-saas-tutorial/server.png)

## <a name="monitor-the-pool"></a>Monitorování fondu

Pokud generátor zatížení běžel několik minut, mělo by být k dispozici dostatek dat, abyste mohli začít sledovat některé možnosti monitorování vestavěné do fondů a databází.

1. Přejděte na server *tenants1-&lt;UŽIVATEL&gt;* a klikněte na **Pool1**. Zobrazí se využití prostředků fondu:

   ![monitorování fondu](./media/sql-database-saas-tutorial/monitor-pool.png)

Na těchto dvou grafech je krásně vidět, jak dobře se elastické fondy a databáze SQL hodí pro úlohy SaaS aplikací. Čtyři databáze, všechny s nárazovým zatížením až 40 eDTU, snadno zvládá fond s 50 eDTU. Kdyby byla každá z nich zřízená jako samostatná databáze, musela by každá mít úroveň S2 (50 DTU), aby podporovala nárazové zatížení, ale cena 4 samostatných databází S2 by byla téměř trojnásobná oproti ceně za fond. A to má fond pořád dost velkou rezervu pro mnoho dalších databází. V reálných situacích zákazníci aktuálně používají až 500 databází ve fondech s 200 eDTU. Další informace najdete v [kurzu monitorování výkonu](sql-database-saas-tutorial-performance-monitoring.md).


## <a name="deleting-the-resources-created-with-this-tutorial"></a>Odstranění prostředků vytvořených v tomto kurzu

Když skončíte zkoumání a práci s aplikací WTP, přejděte na portálu ke skupině prostředků aplikace a odstraňte ji, abyste zastavili veškeré účtování spojené s jejím nasazením. Pokud jste použili některý z doprovodných kurzů, společně s aplikací se odstraní i všechny prostředky, které se při nich vytvořily.


## <a name="next-steps"></a>Další kroky

V tomto kurzu jste se dozvěděli:

> [!div class="checklist"]

> * Jak nasadit aplikaci WTP
> * O serverech, fondech a databázích, které aplikaci tvoří
> * Tenanti jsou namapovaní na svá data pomocí *katalogu*
> * Jak zřídit nové tenanty
> * Jak zobrazit využití fondu k monitorování aktivity tenanta
> * Jak odstranit ukázkové prostředky k zastavení souvisejícího účtování

Teď si vyzkoušejte [kurz zřízení nových tenantů a jejich registrace v katalogu](sql-database-saas-tutorial-provision-and-catalog.md).



## <a name="additional-resources"></a>Další zdroje

* [Další kurzy, které vycházejí z původně nasazené aplikace WTP (Wingtip Tickets Platform)](sql-database-wtp-overview.md#sql-database-wtp-saas-tutorials)
* Další informace o elastických fondech najdete v tématu [*Co je elastický fond Azure*](https://docs.microsoft.com/azure/sql-database/sql-database-elastic-pool).
* Další informace o elastických úlohách najdete v tématu [*Správa cloudových databází s horizontálním navýšením kapacity*](https://docs.microsoft.com/azure/sql-database/sql-database-elastic-jobs-overview).
* Další informace o SaaS aplikacích s více tenanty najdete v tématu [*Vzory návrhu pro SaaS aplikace s více tenanty*](https://docs.microsoft.com/azure/sql-database/sql-database-design-patterns-multi-tenancy-saas-applications).

