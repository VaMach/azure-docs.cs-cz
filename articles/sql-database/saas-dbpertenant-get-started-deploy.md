---
title: "Kurz SaaS víceklientské – Azure SQL Database | Microsoft Docs"
description: "Nasazení a prozkoumejte víceklientské aplikace Wingtip SaaS, která demonstruje SaaS vzory pro používání Azure SQL Database."
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
ms.date: 11/10/2017
ms.author: sstein
ms.openlocfilehash: 6270656ba172f1e85df557d59173ecdcf7d6cbcb
ms.sourcegitcommit: 732e5df390dea94c363fc99b9d781e64cb75e220
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/14/2017
---
# <a name="deploy-and-explore-a-multi-tenant-application-that-uses-azure-sql-database---wingtip-saas"></a>Nasazení a prozkoumejte víceklientské aplikace, která používá Azure SQL Database – Wingtip SaaS

V tomto kurzu nasazení a prozkoumejte Wingtip SaaS aplikace. Aplikace používá k poskytování služeb více tenantům strukturu SaaS aplikací, tj. s jednou databází pro každého tenanta. Aplikace je navržená ke prezentují funkce databáze SQL Azure, které zjednodušují povolení SaaS scénáře.

Pět minut po kliknutí na tlačítko *Nasadit do Azure* zobrazené níže budete mít k dispozici SaaS aplikaci pro více klientů využívající službu SQL Database, která bude běžet v cloudu. Aplikace se nasazuje s tři ukázkové klientů, každý s svou vlastní databázi, všechny nasazené do elastický fond SQL. Aplikace se nasadí do vašeho předplatného Azure, která poskytuje úplný přístup k prozkoumávání a pracovat jednotlivé součásti aplikace. Zdrojový kód a správu skripty aplikace jsou k dispozici v úložišti WingtipSaaS GitHub.


V tomto kurzu se dozvíte:

> [!div class="checklist"]

> * Postup nasazení aplikace Wingtip SaaS
> * Kde lze získat zdrojovému kódu aplikace a skripty pro správu
> * O serverech, fondech a databázích, které aplikaci tvoří
> * Jak klienti jsou mapované na svoje data pomocí *katalogu*
> * Jak zřídit nového klienta
> * Postup sledování klientů aktivity v aplikaci

K prozkoumání různých vzorů návrhu a správy SaaS aplikace máte k dispozici [řadu souvisejících kurzů](saas-dbpertenant-wingtip-app-overview.md#sql-database-wingtip-saas-tutorials), které vycházejí z tohoto počátečního nasazení. Při procházení těmito kurzy se důkladně podívejte na uvedené skripty a vyzkoušejte si, jak jsou implementované různé vzory SaaS. Projděte si krok po kroku skripty v každém kurzu, abyste detailně porozuměli způsobu implementace mnoha funkcí služby SQL Database, které zjednodušují vývoj SaaS aplikací.

## <a name="prerequisites"></a>Požadavky

Předpokladem dokončení tohoto kurzu je splnění následujících požadavků:

* Prostředí Azure PowerShell je nainstalované. Podrobnosti najdete v článku [Začínáme s prostředím Azure PowerShell](https://docs.microsoft.com/powershell/azure/get-started-azureps).

## <a name="deploy-the-wingtip-saas-application"></a>Nasazení aplikace Wingtip SaaS

Nasazení adresář Wingtip aplikace SaaS:

1. Kliknutím **nasadit do Azure** tlačítko otevře portálu Azure do šablony nasazení Wingtip SaaS. Šablona vyžaduje dvě hodnoty parametrů; název pro novou skupinu prostředků a uživatelské jméno, která odlišuje od jiných nasazení aplikace Wingtip SaaS toto nasazení. Dalším krokem poskytuje podrobnosti pro tyto hodnoty nastavení.

   Zajistěte, aby si přesné hodnoty, které chcete použít, protože budete potřebovat později je zadat do konfiguračního souboru.

   <a href="http://aka.ms/deploywtpapp" target="_blank"><img src="http://azuredeploy.net/deploybutton.png"/></a>

1. Zadejte hodnoty parametrů požadované pro vaše nasazení:

    > [!IMPORTANT]
    > Některá ověřování a brány firewall serveru jsou pro demonstrační účely záměrně nezabezpečené. **Vytvořit novou skupinu prostředků**a nepoužívejte existující skupiny prostředků, serverům nebo fondů. Tato aplikace nebo všechny prostředky, který vytváří, nepoužívejte pro produkční prostředí. Po ukončení práce s aplikací tuto skupinu prostředků odstraňte, aby se zastavilo související účtování.

    * **Skupina prostředků** – vyberte **vytvořit nový** a poskytují **název** pro skupinu prostředků. Vyberte **umístění** z rozevíracího seznamu.
    * **Uživatel** -některé prostředky, vyžadují názvy, které jsou globálně jedinečné. Aby jedinečnosti pokaždé, když nasazujete aplikace zadat hodnotu k odlišení prostředků, které vytvoříte, z prostředků, které jsou vytvořené nasazení Wingtip aplikace. Doporučuje se použít krátké **uživatel**ské jméno, třeba vaše iniciály, a číslo (například *bg1*) a to potom používat v názvu skupiny prostředků (například *wingtip-bg1*). **Uživatele** parametru může obsahovat pouze písmena, číslice a pomlčky (bez mezer). První a poslední znak musí být písmeno nebo číslo (doporučuje se používat všechna písmena malá).


1. **Nasaďte aplikaci**.

    * Kliknutím na tlačítko přijmout podmínky a ujednání.
    * Klikněte na **Koupit**.

1. Kliknutím na **Oznámení** (ikona zvonku napravo od vyhledávacího pole) můžete monitorovat stav nasazení. Nasazení aplikace Wingtip SaaS trvá přibližně pět minut.

   ![nasazení bylo úspěšné](media/saas-dbpertenant-get-started-deploy/succeeded.png)

## <a name="download-and-unblock-the-wingtip-saas-scripts"></a>Stáhněte si a odblokování skripty Wingtip SaaS

Zatímco je nasazení aplikace, stáhněte si skripty zdrojový kód a správu.

> [!IMPORTANT]
> Spustitelný soubor obsah (skripty, knihovny DLL) mohou být blokovány Windows, když jsou soubory zip stažené z externího zdroje a rozbalené. Při extrahování skripty ze souboru zip, použijte následující postup odblokování před extrahování souboru ZIP. Tím se zajistí, že je povoleno spustit skripty.

1. Přejděte do [úložiště github Wingtip SaaS](https://github.com/Microsoft/WingtipSaaS).
1. Klikněte na tlačítko **klonovat nebo stáhnout**.
1. Klikněte na tlačítko **stáhnout ZIP** a soubor uložte.
1. Klikněte pravým tlačítkem myši **WingtipSaaS-master.zip** soubor a vyberte **vlastnosti**.
1. Na **Obecné** vyberte **Odblokovat**a klikněte na tlačítko **použít**.
1. Klikněte na **OK**.
1. Extrahujte soubory.

Skripty, které jsou umístěné v *... \\WingtipSaaS hlavní\\Learning moduly* složky.

## <a name="update-the-configuration-file-for-this-deployment"></a>Aktualizace konfiguračního souboru pro toto nasazení

Před spuštěním všech skriptů, nastavte *skupiny prostředků* a *uživatele* hodnoty v **UserConfig.psm1**. Nastavte tyto proměnné na hodnoty, které můžete zadat během nasazování.

1. V *prostředí PowerShell ISE* otevřete ...\\Learning Modules\\*UserConfig.psm1*.
1. Aktualizace *ResourceGroupName* a *název* s konkrétními hodnotami pro vaše nasazení (na řádky 10 a 11 pouze).
1. Uložte změny!

Toto nastavení tady jednoduše nebude nutné aktualizovat tyto hodnoty specifické pro nasazení v každé skriptu.

## <a name="run-the-application"></a>Spuštění aplikace

Aplikace prezentuje místa, například koncertní sály, jazzové kluby a sportovní kluby, ve kterých se konají akce. Místa zaregistrovat jako zákazníky (nebo klienty) na platformě Wingtip snadný způsob, jak zobrazit seznam událostí a prodej lístků. Každé místo dostane vlastní webovou aplikaci pro správu a výpis svých akcí a prodej lístků, nezávislou a izolovanou od ostatních tenantů. V pozadí každý klient získá databázi SQL, který je nasazený do elastický fond SQL.

Centrální **Centrum akcí** obsahuje seznam adres URL tenantů specifických pro vaše nasazení.

1. Otevřete _události rozbočovače_ ve webovém prohlížeči: http://events.wtp.&lt; Uživatel&gt;. trafficmanager.net (nahraďte názvem vašeho nasazení uživatelské jméno):

    ![centrum akcí](media/saas-dbpertenant-get-started-deploy/events-hub.png)

1. V *Centru akcí* klikněte na **Fabrikam Jazz Club**.

   ![Události](./media/saas-dbpertenant-get-started-deploy/fabrikam.png)


K řízení distribuce příchozí požadavky a používá aplikace [ *Azure Traffic Manager*](../traffic-manager/traffic-manager-overview.md). Stránky akcí, které jsou specifické pro každého tenanta, vyžadují, aby byly názvy tenantů součástí adres URL. Všechny adresy URL tenantů zahrnují konkrétní hodnotu *uživatel* a mají tento formát: http://events.wtp.&lt;UŽIVATEL&gt;.trafficmanager.net/*fabrikamjazzclub*. Aplikace akcí analyzuje název tenanta z adresy URL a z něj vytvoří klíč pro přístup ke katalogu pomocí [*správy mapování horizontálního dělení*](sql-database-elastic-scale-shard-map-management.md). Katalog mapuje klíč klienta umístění databáze. **Události rozbočovače** používá rozšířené metadata v katalogu načíst název klienta, které jsou spojené s každou databázi, aby zajistil seznam adres URL.

V produkčním prostředí byste obvykle vytvořili záznam CNAME DNS k [*nasměrování internetové domény společnosti*](../traffic-manager/traffic-manager-point-internet-domain.md) na profil Traffic Manageru.

## <a name="start-generating-load-on-the-tenant-databases"></a>Spuštění generování zatížení databází tenantů

Teď, když je aplikace nasazena, přidejme ho na spolupráci! *Ukázku LoadGenerator* zatížení spuštěným pro všechny databáze klienta spustí skript prostředí PowerShell. Skutečné zatížení velký počet aplikací SaaS je obvykle ojediněle a nepředvídatelným. Pro simulaci tento typ zatížení, vytvoří generátor zatížení distribuovaná do všech klientů s náhodnou shluky na každého klienta, ke kterým dochází v náhodnou intervalech. Z tohoto důvodu trvá několik minut pro vzor zatížení objeví proto je nejlepší umožníte generátor spustit alespoň tři nebo čtyři minut před monitorování zatížení.

1. V *prostředí PowerShell ISE*, otevřete... \\Learning moduly\\nástroje\\*ukázku LoadGenerator.ps1* skriptu.
1. Stisknutím **F5** spusťte skript s generátorem zatížení (zatím nechejte nastavené výchozí hodnoty parametrů).

> [!IMPORTANT]
> Pokud chcete spustit další skripty, otevřete nové okno prostředí PowerShell ISE. Generátor zatížení běží jako řada úloh v místní relaci prostředí PowerShell. *Ukázku LoadGenerator.ps1* skript se spustí na aktuální zatížení generátor skript, který se spouští jako úlohu popředí plus řadu úloh na pozadí generování zatížení. Pro každou databázi zaregistrovány v katalogu je volána úlohu generátor zatížení. Úlohy jsou spuštěny v místní relace prostředí PowerShell, takže ukončení relace prostředí PowerShell zastaví všechny úlohy. Pokud přepnete na váš počítač, generování zatížení je pozastavený a bude pokračovat, až budete probouzet svůj počítač.

Jakmile generátor zatížení vyvolá zatížení generování úlohy pro každého klienta, úkol popředí zůstane v vyvolání úlohy stavu, kdy začne další úlohy pro jakékoli nové klienty, které jsou následně zřízené. Můžete použít *Ctrl-C* nebo stiskněte klávesu *Zastavit* tlačítko Zastavit úlohu popředí, ale stávající úlohy na pozadí bude pokračovat generování zatížení na každou databázi. Pokud potřebujete monitorovat a řídit úlohy na pozadí, použijte *Get-Job*, *Receive-Job* a *úlohu zastavení*. Když je spuštěn úkol popředí nemůžete použít stejné relace prostředí PowerShell provést jiné skripty. Pokud chcete spustit další skripty, otevřete nové okno prostředí PowerShell ISE.

Pokud chcete restartovat relace generátor zatížení, například s odlišnými parametry, můžete zastavit úlohu popředí a poté znovu spusťte *ukázku LoadGenerator.ps1* skriptu. Opětovné spuštění *ukázku LoadGenerator.ps1* první zastaví všechny aktuálně spuštěné úlohy a potom restartuje generátor, který se spustí novou sadu úloh s parametry.

Nyní ponechte generátor zatížení spuštěna ve stavu vyvolání úlohy.


## <a name="provision-a-new-tenant"></a>Zřízení nového tenanta

Počáteční nasazení vytvoří tři ukázkové klienty, ale vytvoříme jiného klienta, pokud chcete zobrazit, jak to ovlivní nasazené aplikace. Pracovní postup zřizování klientů Wingtip SaaS je podrobně popsaná v [kurzu zřizování a katalog](saas-dbpertenant-provision-and-catalog.md). V tomto kroku rychle vytvořit nového klienta.

1. V *prostředí PowerShell ISE* otevřete ...\\Learning Modules\Provision and Catalog\\*Demo-ProvisionAndCatalog.ps1*.
1. Stiskněte klávesu **F5** pro spuštění skriptu (ponechte výchozí hodnoty pro nyní).

   > [!NOTE]
   > Použít mnoho skriptů Wingtip SaaS *$PSScriptRoot* povolit procházení složek, které chcete volat funkce v jiné skripty. Tato proměnná je Vyhodnocená jenom když skript se spustí, stisknutím klávesy **F5**.  Zvýraznění a spuštění výběr (**F8**) může vést k chybám, takže stiskněte **F5** při spouštění skriptů.

Novou databázi klienta je vytvořen elastický fond SQL, inicializovat a zaregistrovány v katalogu. Po úspěšném zřízení, je k novému klientovi prodávané ticket *události* server se zobrazí v prohlížeči:

![Nový tenant](./media/saas-dbpertenant-get-started-deploy/red-maple-racing.png)

Obnovit *události rozbočovače* a k novému klientovi se teď zobrazí v seznamu.


## <a name="explore-the-servers-pools-and-tenant-databases"></a>Zkoumání serverů, fondů a databází tenantů

Teď, když jste spuštění, spuštění zátěžového proti kolekci klientů, podíváme se na některé prostředky, které byly nasazeny:

1. V [portál Azure](http://portal.azure.com), přejděte do seznamu serverů SQL Server a otevřete **katalogu -&lt;uživatele&gt; ** serveru. Na serveru katalogu jsou dvě databáze. **Tenantcatalog**a **basetenantdb** (prázdnou *zlaté* nebo databáze šablony, která se zkopírují k vytvoření nové klienty).

   ![databáze](./media/saas-dbpertenant-get-started-deploy/databases.png)

1. Přejděte zpět do seznamu systému SQL Server a otevřete **tenants1 -&lt;uživatele&gt; ** serveru, který obsahuje databáze klienta. Každá databáze klienta je _elastické standardní_ databáze ve fondu standardní 50 eDTU. Také Všimněte si, že je _Red javor Racing_ databázi, databázi klienta můžete zřízené.

   ![server](./media/saas-dbpertenant-get-started-deploy/server.png)

## <a name="monitor-the-pool"></a>Monitorování fondu

Pokud generátor zatížení běžel několik minut, mělo by být k dispozici dostatek dat, abyste mohli začít sledovat některé možnosti monitorování vestavěné do fondů a databází.

1. Přejděte na server **tenants1 -&lt;uživatele&gt;**a klikněte na tlačítko **Pool1** zobrazení využití prostředků pro fond (generátor zatížení spustili jednu hodinu v následující grafy):

   ![monitorování fondu](./media/saas-dbpertenant-get-started-deploy/monitor-pool.png)

Na těchto dvou grafech je krásně vidět, jak dobře se elastické fondy a databáze SQL hodí pro úlohy SaaS aplikací. Čtyři databáze, všechny s nárazovým zatížením až 40 eDTU, snadno zvládá fond s 50 eDTU. Pokud byly zřízení jako samostatné databáze jejich by každý musí být S2 (50 DTU) pro podporu shluky. Náklady na 4 samostatné S2 databází by téměř 3krát cena fondu a fondu má stále dostatek prostoru pro mnoho více databází. V situacích, reálného databáze SQL zákazníků aktuálně používá až 500 databází ve fondech 200 eDTU. Další informace najdete v [kurzu monitorování výkonu](saas-dbpertenant-performance-monitoring.md).


## <a name="next-steps"></a>Další kroky

V tomto kurzu jste se dozvěděli:

> [!div class="checklist"]

> * Postup nasazení aplikace Wingtip SaaS
> * O serverech, fondech a databázích, které aplikaci tvoří
> * Tenanti jsou namapovaní na svá data pomocí *katalogu*
> * Jak zřídit nové tenanty
> * Jak zobrazit využití fondu k monitorování aktivity tenanta
> * Jak odstranit ukázkové prostředky k zastavení souvisejícího účtování

Nyní zkuste [kurzu zřizování a katalog](saas-dbpertenant-provision-and-catalog.md).



## <a name="additional-resources"></a>Další zdroje

* Další [návodů, které sestavení na adresář Wingtip SaaS aplikace](saas-dbpertenant-wingtip-app-overview.md#sql-database-wingtip-saas-tutorials)
* Další informace o elastických fondech najdete v tématu [*Co je elastický fond Azure*](sql-database-elastic-pool.md).
* Další informace o elastických úlohách najdete v tématu [*Správa cloudových databází s horizontálním navýšením kapacity*](sql-database-elastic-jobs-overview.md).
* Další informace o SaaS aplikacích s více tenanty najdete v tématu [*Vzory návrhu pro SaaS aplikace s více tenanty*](saas-tenancy-app-design-patterns.md).
