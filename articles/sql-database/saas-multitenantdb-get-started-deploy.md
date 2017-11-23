---
title: "Nasaďte aplikaci SaaS horizontálně dělené víceklientské databáze, která používá Azure SQL Database | Microsoft Docs"
description: "Nasazení a prozkoumejte víceklientské databázovou aplikaci horizontálně dělené Wingtip lístky SaaS, která demonstruje SaaS vzory pomocí Azure SQL Database."
keywords: kurz k sql database
services: sql-database
documentationcenter: 
author: stevestein
manager: craigg
editor: billgib;MightyPen
ms.service: sql-database
ms.custom: scale out apps
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/13/2017
ms.author: sstein
ms.openlocfilehash: cb55bf1f1c7eeb0fc7608aca8d70818b5e3e06c0
ms.sourcegitcommit: 8aa014454fc7947f1ed54d380c63423500123b4a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/23/2017
---
# <a name="deploy-and-explore-a-sharded-multi-tenant-application-that-uses-azure-sql-database"></a>Nasazení a prozkoumejte horizontálně dělené víceklientské aplikace, která používá Azure SQL Database

V tomto kurzu nasazení a prozkoumat ukázkové aplikace víceklientské databáze SaaS, která je s názvem Wingtip lístků. Adresář Wingtip aplikace je navržená k prezentují funkce databáze SQL Azure, které usnadňují implementaci SaaS scénářů.

Tato implementace Wingtips využívá vzor horizontálně dělené víceklientské databáze. Identifikátor klienta, je horizontálního dělení. Data klienta je distribuován do konkrétní databázi podle hodnot identifikátoru klienta. Bez ohledu na to, kolik klientů obsahuje všechny danou databázi jsou všechny databáze více klientů v tom smyslu, že schémata tabulek obsahovat identifikátor klienta. 

Tento vzor databáze umožňuje ukládání jednoho nebo více klientů v každé horizontálního oddílu nebo databáze. Můžete optimalizovat pro nižší cenu tak, že každou databázi sdílet víc klientů. Nebo můžete optimalizovat pro izolaci tak, že každá databáze uložit pouze jednoho klienta. Zvoleného optimalizace můžete provést samostatně pro každou konkrétní klienta. Zvoleného mohou být provedeny, když klient nejdřív uložen, nebo můžete později změnit své rozhodnutí. Aplikace je určena pro dobře v obou případech.

#### <a name="app-deploys-quickly"></a>Rychle nasazuje aplikace

Následující části nasazení poskytuje **nasadit do Azure** tlačítko. Při stisknutí tlačítka nasazení aplikace Wingtip plně pět minut později. Adresář Wingtip aplikace běží v cloudu Azure a používá Azure SQL Database. Adresář Wingtip se nasadí do vašeho předplatného Azure. Máte plný přístup k práci s jednotlivé součásti aplikace.

Aplikace je nasazená s daty pro tři ukázkové klienty. Klienti jsou uloženy společně v jedné databáze více klientů.

Každý, kdo můžete stáhnout zdrojového kódu C# a prostředí PowerShell pro adresář Wingtip lístky z [našem úložišti Github][link-github-wingtip-multitenantdb-55g].

#### <a name="learn-in-this-tutorial"></a>Další informace v tomto kurzu

> [!div class="checklist"]

> - Postup nasazení aplikace Wingtip SaaS.
> - Kde lze získat zdrojovému kódu aplikace a skripty pro správu.
> - O servery a databáze, které tvoří aplikace.
> - Jak klienti jsou mapované na svoje data pomocí *katalogu*.
> - Jak zřídit nového klienta.
> - Postup sledování klientů aktivity v aplikaci.

Je k dispozici řadu související návodů, které stavět na toto počáteční nasazení. Kurzů k prozkoumejte rozsah SaaS návrhu a správu vzory. Při práci prostřednictvím kurzů k vám doporučujeme, aby se krok prostřednictvím zadané skriptů, které chcete zobrazit, jak jsou implementované různé vzorce SaaS.

## <a name="prerequisites"></a>Požadavky

Předpokladem dokončení tohoto kurzu je splnění následujících požadavků:

- Je nainstalována nejnovější prostředí Azure PowerShell. Podrobnosti najdete v tématu [Začínáme s Azure Powershellem][link-azure-get-started-powershell-41q].

## <a name="deploy-the-wingtip-tickets-app"></a>Nasazení adresář Wingtip lístky aplikace

1. Klikněte na následující **nasadit do Azure** tlačítko.
    - Otevře se na portálu Azure pomocí šablony nasazení Wingtip lístky SaaS. Šablona vyžaduje dvě hodnoty parametru: název pro novou skupinu prostředků a hodnotu "uživatelem", která odlišuje od jiných nasazení aplikace pro toto nasazení. Dalším krokem poskytuje podrobnosti pro nastavení hodnot těchto parametrů.
        - Ujistěte se, že Poznámka: přesné hodnoty, které chcete použít, protože je budete potřebovat později konfiguračního souboru.

    [![Tlačítko pro nasazení do Azure.][image-deploy-to-azure-blue-48d]][link-aka-ms-deploywtp-mtapp-52k]

2. Zadejte hodnoty parametrů požadovaných pro nasazení.

    > [!IMPORTANT]
    > Některé ověřování a nastavení brány firewall serveru, jsou záměrně nezabezpečené usnadňuje ukázky. Zvolte **vytvořit novou skupinu prostředků**a nepoužívejte existující skupiny prostředků, serverům nebo fondů. Tato aplikace nebo všechny prostředky, který vytváří, nepoužívejte pro produkční prostředí. Po ukončení práce s aplikací tuto skupinu prostředků odstraňte, aby se zastavilo související účtování.

    Je nejvhodnější použít jenom malá písmena, číslice a pomlčky v názvy prostředků.

    - Pro **skupiny prostředků** – vyberte **vytvořit nový**a potom zadejte **název** pro skupinu prostředků (malá a velká písmena).
        - Doporučujeme vám, že všechna písmena ve vašem název skupiny prostředků být malými písmeny.
        - Doporučujeme přidat pomlčkou, za nímž následuje vašimi iniciálami, za nímž následuje číslice: například *wingtip af1*.
        - Vyberte **umístění** z rozevíracího seznamu.
    - Pro **uživatele** -doporučujeme, abyste zvolili malou **uživatele** hodnotu, třeba vašimi iniciálami plus číslice: například *af1*.

3. **Nasaďte aplikaci**.

    - Kliknutím na tlačítko přijmout podmínky a ujednání.
    - Klikněte na **Koupit**.

4. Monitorování stavu nasazení kliknutím **oznámení**, který je na ikonu zvonku napravo od pole hledání. Nasazení aplikace Wingtip trvá přibližně pět minut.

   ![nasazení bylo úspěšné](media/saas-multitenantdb-get-started-deploy/succeeded.png)

## <a name="download-and-unblock-the-management-scripts"></a>Stáhněte si a odblokování skripty správy

Když je nasazení aplikace, stáhněte si skripty aplikace zdrojový kód a správu.

> [!IMPORTANT]
> Spustitelný soubor obsah (skripty, knihovny DLL) mohou být blokovány Windows, když jsou soubory zip stažené z externího zdroje a rozbalené. Při extrahování skripty ze souboru zip, použijte následující postup odblokování před extrahování souboru ZIP. Ve odblokování soubor .zip zajistíte, že je povoleno spustit skripty.

1. Přejděte do [úložiště github WingtipTicketsSaaS MultiTenantDb](https://github.com/Microsoft/WingtipTicketsSaaS-MultiTenantDb).
2. Klikněte na tlačítko **klonovat nebo stáhnout**.
3. Klikněte na tlačítko **stáhnout ZIP** a soubor uložte.
4. Klikněte pravým tlačítkem myši **WingtipTicketsSaaS-MultiTenantDb-master.zip** soubor a vyberte **vlastnosti**.
5. Na **Obecné** vyberte **Odblokovat**a klikněte na tlačítko **použít**.
6. Klikněte na **OK**.
7. Extrahujte soubory.

Skripty, které jsou umístěné v *... \\WingtipTicketsSaaS-MultiTenantDb-master\\Learning moduly\\*  složky.

## <a name="update-the-configuration-file-for-this-deployment"></a>Aktualizace konfiguračního souboru pro toto nasazení

Před spuštěním všech skriptů, nastavte *skupiny prostředků* a *uživatele* hodnoty v **UserConfig.psm1**. Nastavte tyto proměnné na stejné hodnoty, které nastavíte během nasazení.

1. V *prostředí PowerShell ISE* otevřete ...\\Learning Modules\\*UserConfig.psm1*.
2. Aktualizace *ResourceGroupName* a *název* s konkrétními hodnotami pro vaše nasazení (na řádky 10 a 11 pouze).
3. Uložte změny.

S hodnotami nastavenými v tomto souboru jsou používány všechny skripty, proto je důležité, zda že je přesné. Pokud aplikaci znovu nasaďte, ujistěte se, že vyberete hodnotu jiného prostředku skupiny a uživatele. Aktualizujte UserConfig s novými hodnotami.

## <a name="run-the-application"></a>Spuštění aplikace

Aplikace prezentuje místa, například koncertní sály, jazzové kluby a sportovní kluby, ve kterých se konají akce. Místa zaregistrovat jako zákazníky (nebo klienty) na platformě Wingtip snadný způsob, jak zobrazit seznam událostí a prodej lístků. Každé místo dostane vlastní webovou aplikaci pro správu a výpis svých akcí a prodej lístků, nezávislou a izolovanou od ostatních tenantů. V pozadí každý klient data uložena ve výchozím nastavení v horizontálně dělené víceklientské databáze.

Centrálního **události rozbočovače** obsahuje seznam odkazů na klienty v určitém nasazení.

1. Otevřete *události rozbočovače* ve webovém prohlížeči:
    - http://events.Wingtip-mt.&lt;uživatele&gt;. trafficmanager.net &nbsp; *(Nahraďte hodnotu uživatelského vaše nasazení.)*

    ![centrum akcí](media/saas-multitenantdb-get-started-deploy/events-hub.png)

2. V *Centru akcí* klikněte na **Fabrikam Jazz Club**.

   ![Události](./media/saas-multitenantdb-get-started-deploy/fabrikam.png)

K řízení distribuce příchozí požadavky a používá aplikace [Azure Traffic Manager](../traffic-manager/traffic-manager-overview.md). Na stránkách události, které jsou specifické pro klienta, zahrnují název klienta v adrese URL. Adresy URL také obsahovat vaše konkrétní hodnotu uživatele a mít tento formát:

- http://events.Wingtip-mt.&lt;uživatele&gt;.trafficmanager.net/*fabrikamjazzclub*
 
Analyzuje název klienta z adresy URL a rozdělí vytvoření klíče pro přístup k katalogu pomocí aplikace události [horizontálního oddílu mapy správu](sql-database-elastic-scale-shard-map-management.md). Katalog mapuje klíč klienta umístění databáze. **Události rozbočovače** obsahuje seznam všech klientů, které jsou zaregistrovány v katalogu. **Události rozbočovače** používá rozšířené metadata v katalogu načíst název klienta, které jsou spojené s každou mapování k vytvoření adresy URL.

V produkčním prostředí, obvykle vytvořili byste záznam CNAME DNS do [nasměrování internetové domény společnosti](../traffic-manager/traffic-manager-point-internet-domain.md) pro profil správce provozu.

## <a name="start-generating-load-on-the-tenant-databases"></a>Spuštění generování zatížení databází tenantů

Teď, když je aplikace nasazena, přidejme ho na spolupráci! *Ukázku LoadGenerator* zátěži pro každého klienta spustí skript prostředí PowerShell. Skutečné zatížení velký počet aplikací SaaS je obvykle ojediněle a nepředvídatelným. Pro simulaci tento typ zatížení, vytvoří generátor zatížení rozdělené mezi všechny klienty. Zatížení zahrnuje náhodnou shluky na každého klienta, ke kterým dochází v náhodnou intervalech. Trvá několik minut pro vzor zatížení objeví, proto je vhodné umožníte generátor spustit alespoň tři nebo čtyři minut před monitorování zatížení.

1. V *prostředí PowerShell ISE*, otevřete... \\Learning moduly\\nástroje\\*ukázku LoadGenerator.ps1* skriptu.
2. Stisknutím **F5** spusťte skript s generátorem zatížení (zatím nechejte nastavené výchozí hodnoty parametrů).

> [!IMPORTANT]
> *Ukázku LoadGenerator.ps1* skript se otevře jiná relace prostředí PowerShell, kde je spuštěna generátor zatížení. Generátor zatížení se spustí v této relaci jako popředí úlohu, která volá zatížení generování úlohy na pozadí, jednu pro každého klienta.

Po spuštění úlohy popředí, zůstane ve stavu vyvolání úlohy. Úloha spustí další úlohy pro jakékoli nové klienty, které jsou následně zřízené.

Ukončení relace prostředí PowerShell zastaví všechny úlohy.

Můžete chtít restartovat relace generátor zatížení použít jiné hodnoty parametru. Pokud ano, zavřete generování relace prostředí PowerShell a poté znovu spusťte *ukázku LoadGenerator.ps1*.

## <a name="provision-a-new-tenant-into-the-sharded-database"></a>Zřízení nového klienta do horizontálně dělené databáze

Počáteční nasazení obsahuje tři ukázkové klienty v *Tenants1* databáze. Vytvoříme jiného klienta, pokud chcete zobrazit, jak to ovlivní nasazené aplikace. V tomto kroku rychle vytvořit nového klienta.

1. Otevřete... \\Učení Modules\ProvisionTenants\\*ukázku ProvisionTenants.ps1* v *prostředí PowerShell ISE*.
2. Stiskněte klávesu **F5** pro spuštění skriptu (ponechte výchozí hodnoty pro nyní).

   > [!NOTE]
   > Použít mnoho skriptů Wingtip lístky SaaS *$PSScriptRoot* procházení složek, nebo volat jiné skripty nebo naimportovat moduly. Tato proměnná je vyhodnocena jenom v případě, že skript se spustí jako celek, stisknutím klávesy **F5**.  Zvýraznění a spuštění výběr (**F8**) může vést k chybám, takže stiskněte **F5** při spouštění skriptů.

K novému klientovi Red javor Racing je přidán do *Tenants1* databáze a zaregistrovány v katalogu. Je k novému klientovi prodávané ticket *události* lokality se otevře v prohlížeči:

![Nový tenant](./media/saas-multitenantdb-get-started-deploy/red-maple-racing.png)

Obnovit *události rozbočovače* a k novému klientovi se teď zobrazí v seznamu.

## <a name="provision-a-new-tenant-in-its-own-database"></a>Zřízení nového klienta ve vlastní databázi

Horizontálně dělené víceklientského modelu můžete zvolit, jestli ke zřízení nového klienta v databázi obsahující ostatních klientů, nebo pro zřízení klienta na databázi. Klient v jeho vlastní využívání s jeho data izolované od dat ostatních klientů. Izolaci umožňuje spravovat výkon tohoto klienta nezávisle na ostatních klientů. Navíc je snazší data obnovit předchozí stav pro izolované klienta. Můžete zvolit chápat jednotlivé databáze bez zkušební verze nebo běžné zákazníků v databázi více klientů a zákazníky premium. Pokud vytvoříte velký počet databází, že každý obsahovat pouze jednoho klienta, můžete je spravovat všechny souhrnně v elastickém fondu za účelem optimalizace prostředků náklady.  

Nyní jsme zřídit jiného klienta, tentokrát ve vlastní databázi.

1. V... \\Learning moduly\\ProvisionTenants\\*ukázku ProvisionTenants.ps1*, upravte *$TenantName* k **Salix Salsa**,  *$VenueType* k **tance** a *$Scenario* k **2**.

2. Stiskněte klávesu **F5** pro spuštění skriptu znovu.
    - Tato stisknutím klávesy F5 zřídí k novému klientovi v samostatné databáze. Databáze a klienta jsou zaregistrovány v katalogu. Potom prohlížeči se otevře na stránku událostí klienta.

   ![Stránka události Salix Salsa](./media/saas-multitenantdb-get-started-deploy/salix-salsa.png)

   - Přejděte do dolní části stránky. Existuje v informační zprávě zobrazí název databáze, ve kterém je uložený dat klienta.

3. Aktualizujte Centrum událostí a dva nové klienty se teď zobrazí v seznamu.



## <a name="explore-the-servers-and-tenant-databases"></a>Prozkoumejte serverů a klientů databáze

Nyní podíváme na některé prostředky, které byly nasazeny:

1. V [portál Azure](http://portal.azure.com), přejděte do seznamu skupin prostředků. Otevřete skupinu prostředků, kterou jste vytvořili při nasazení aplikace.

   ![skupina prostředků](./media/saas-multitenantdb-get-started-deploy/resource-group.png)

2. Klikněte na tlačítko **katalogu mt&lt;uživatele&gt;**  serveru. Na server katalogu obsahuje dvě databáze s názvem *tenantcatalog* a *basetenantdb*. *Basetenantdb* databáze je databáze prázdné šablony. Je zkopírován do vytvoření nového klienta databáze, jestli se používá pro velký počet klientů nebo jedním klienta.

   ![katalogový server](./media/saas-multitenantdb-get-started-deploy/catalog-server.png)

3. Přejděte zpět do skupiny prostředků a vyberte *tenants1-mt* serveru, který obsahuje databáze klienta.
    - Databázi tenants1 je víceklientské databáze, v níž jsou uloženy původní tři klienti plus první klient, který jste přidali. Je nakonfigurován jako 50 standardní DTU databáze.
    - **Salixsalsa** databáze obsahuje místo tance Salix Salsa jako jeho pouze klienta. Ve výchozím nastavení je nakonfigurován jako Standard edition databáze s 50 Dtu.

   ![Klienti serveru](./media/saas-multitenantdb-get-started-deploy/tenants-server.png)


## <a name="monitor-the-performance-of-the-database"></a>Monitorování výkonu databáze nástroje

Pokud generátor zatížení běží několik minut, dost telemetrických dat je k dispozici pro zahájení vyhledávání na některé možnosti, které jsou součástí portálu monitorování databáze.

1. Vyhledejte **tenants1-mt&lt;uživatele&gt;**  serveru a klikněte na **tenants1** zobrazení využití prostředků pro databázi, která se nachází čtyři klienty. Každý klient podléhá ojediněle intenzivně zatěžován z generátor zatížení:

   ![monitorování tenants1](./media/saas-multitenantdb-get-started-deploy/monitor-tenants1.png)

   Graf využití DTU vhodně ilustruje jak databázi víceklientské nepředvídatelným zatížení napříč velký počet klientů. V takovém případě je generátor zatížení použití ojediněle zatížení zhruba 30 Dtu každému klientovi. Tato zatížení rovná využití 60 %, 50 DTU databáze. Špičky delší než 60 % jsou výsledkem zatížení, které bylo použito pro víc klientů současně.

2. Vyhledejte **tenants1-mt&lt;uživatele&gt;**  serveru a klikněte na **salixsalsa** databáze k zobrazení využití prostředků u této databáze, který obsahuje pouze jeden klienta.

   ![salixsalsa databáze](./media/saas-multitenantdb-get-started-deploy/monitor-salix.png)

Generátor zatížení aplikuje podobné zatížení každému klientovi, bez ohledu na to, které databázové každého klienta je v. Pouze jeden klientovi v **salixsalsa** databáze, uvidíte, že databáze může tolerovat mnohem vyšší zatížení než databáze s několika klienty. 

> [!NOTE]
> Načítání vytvořený generátor zatížení jsou pouze pro ilustraci.  Prostředky přidělené databáze více klientů a jednoho klienta a počtu klientů, které mohou být uloženy v databázi více klientů, závisí na vzory skutečné úlohy v aplikaci.


## <a name="next-steps"></a>Další kroky

V tomto kurzu jste se dozvěděli:

> [!div class="checklist"]

> - Postup nasazení aplikace Wingtip lístky SaaS víceklientské databáze
> - O serverů a databází, které tvoří aplikace
> - Tenanti jsou namapovaní na svá data pomocí *katalogu*
> - Jak zřídit nové klienty do více klientů databáze a databáze jednoho klienta.
> - Jak zobrazit využití fondu k monitorování aktivity tenanta
> - Jak odstranit ukázkové prostředky k zastavení souvisejícího účtování

Nyní zkuste [kurzu zřizování klientů](sql-database-saas-tutorial-provision-and-catalog.md).



## <a name="additional-resources"></a>Další zdroje

- Další informace o SaaS aplikacích s více tenanty najdete v tématu [*Vzory návrhu pro SaaS aplikace s více tenanty*](https://docs.microsoft.com/azure/sql-database/sql-database-design-patterns-multi-tenancy-saas-applications).








<!--  Link references.

A [series of related tutorials] is available that build upon this initial deployment.
[link-wtp-overivew-jumpto-saas-tutorials-97j]: saas-multitenantdb-wingtip-app-overview.md#sql-database-wingtip-saas-tutorials

-->

[link-aka-ms-deploywtp-mtapp-52k]: http://aka.ms/deploywtp-mtapp


[link-azure-get-started-powershell-41q]: https://docs.microsoft.com/powershell/azure/get-started-azureps

[link-github-wingtip-multitenantdb-55g]: https://github.com/Microsoft/WingtipTicketsSaaS-MultiTenantDB/





<!--  Image references.

[image-deploy-to-azure-blue-48d]: http://aka.ms/deploywtp-mtapp "Button for Deploy to Azure."

-->

[image-deploy-to-azure-blue-48d]: media/saas-multitenantdb-get-started-deploy/deploy.png

