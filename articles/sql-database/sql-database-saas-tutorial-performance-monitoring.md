---
title: "Monitorování výkonu SaaS aplikace využívající službu SQL Database | Dokumentace Microsoftu"
description: "Monitorování a správa výkonu ukázkové aplikace Wingtip Tickets (WTP) využívající službu Azure SQL Database"
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
ms.openlocfilehash: af9511978718af10c97bee6af3a2835c9d2c1ff4
ms.contentlocale: cs-cz
ms.lasthandoff: 05/12/2017


---
# <a name="monitor-performance-of-the-wtp-sample-saas-application"></a>Monitorování výkonu ukázkové SaaS aplikace WTP

Tento kurz představuje integrované funkce monitorování a upozorňování služby SQL Database a elastické fondy a dále se věnuje několika důležitým scénářům řízení výkonu používaných v aplikacích SaaS.

Aplikace Wingtip Tickets používá model dat s jedním tenantem, přičemž každé místo (tenant) má svou vlastní databázi. Stejně jako u většiny aplikací SaaS je předpokládaný vzorek úloh tenanta nepředvídatelný a sporadický. Jinými slovy to znamená, že prodej lístků může probíhat kdykoli. Abyste mohli využít výhod tohoto typického vzoru používání databáze, databáze tenantů se nasazují do elastických databázových fondů. Elastické fondy optimalizují náklady na řešení prostřednictvím sdílení prostředků mezi mnoha databázemi. S tímto typem vzorců je důležité monitorovat využití databáze a prostředků fondu k zajištění, že jsou přiměřeně vyvážená přetížení mezi jednotlivými fondy. Je také potřeba zajistit, že jednotlivé databáze mají adekvátní prostředky a že fondy nedosahují limitů [eDTU](sql-database-what-is-a-dtu.md). Tento kurz se věnuje způsobům monitorování a správy databází a fondů a uvádí, jak se provádějí nápravné akce v reakci na variace v úloze.

Co se v tomto kurzu naučíte:

> [!div class="checklist"]

> * Simulace použití v databázích tenantů pomocí dodaného generátoru zatížení
> * Monitorování databází tenantů při reakci na zvýšení zatížení
> * Vertikální navýšení kapacity elastického fondu v reakci na zvýšení zatížení databáze
> * Zřízení druhého elastického fondu pro vyrovnání zatížení činnosti databáze


Předpokladem dokončení tohoto kurzu je splnění následujících požadavků:

* Je nasazená aplikace WTP. Informace o nasazení, které netrvá ani pět minut, najdete v článku [Nasazení SaaS aplikace WTP a seznámení s ní](sql-database-saas-tutorial.md).
* Prostředí Azure PowerShell je nainstalované. Podrobnosti najdete v článku [Začínáme s prostředím Azure PowerShell](https://docs.microsoft.com/powershell/azure/get-started-azureps).

## <a name="introduction-to-saas-performance-management-patterns"></a>Úvod do principu správy výkonu SaaS

Správa výkonu databáze sestává z kompilování a analýz dat výkonu a následného reagování na tato data prostřednictvím úpravy parametrů pro řízení přijatelné doby odezvy na aplikaci. Při hostování více tenantů představují fondy elastické databáze nenákladný způsob zajištění a správy prostředků pro skupinu databází s nepředvídatelnými úlohami. Při určitých vzorcích úloh může být správa ve fondu užitečná pro pouhé dvě databáze S3. Fond nejen sdílí náklady na prostředky, ale může také eliminovat potřebu konstantně monitorovat a sledovat jednotlivé databáze.

![média](./media/sql-database-saas-tutorial-performance-monitoring/app-diagram.png)

Nadále je potřeba monitorovat fondy a databáze, které jsou ve fondech obsažené, aby se zajistilo jejich setrvání v přijatelných rozsazích výkonu. Vylaďte konfiguraci fondu podle potřeb agregované úlohy. Tím zajistíte, že jsou jednotky eDTU fondu vhodné pro celkovou úlohu. Upravte maximální a minimální hodnoty eDTU jednotlivých databází na vhodné hodnoty pro vaše konkrétní aplikační požadavky.

### <a name="performance-management-strategies"></a>Strategie výkonu aplikací

* Abyste nemuseli výkon monitorovat ručně, je nejvhodnějším způsobem **nastavit výstrahy, které se aktivují, když se databáze nebo fondy odchýlí od normálního rozsahu** .
* V reakci na krátkodobé kolísání v agregované úrovni výkonu fondu je možné **vertikálně navýšit nebo snížit kapacitu úrovně eDTU fondu**. Pokud k tomuto kolísání dochází pravidelně nebo je předvídatelné, **je možné naplánovat automatické škálování fondu**. Pokud například víte, že je úloha malého rozsahu, třeba přes noc nebo o víkendech, můžete vertikálně snížit kapacitu.
* Pokud chcete reagovat na dlouhodobější kolísání nebo změny počtu databází, **je možné přesunout jednotlivé databáze do jiných fondů**.
* Pokud potřebujete reagovat na krátkodobé navýšení zatížení *jednotlivých* databází, je možné vyjmout **jednotlivé databáze z fondu a přiřadit jim samostatnou úroveň výkonu** na dané období. Po snížení zatížení je možné databázi vrátit do fondu. Tam, kde to víte předem, je možné databáze přesouvat preventivně, aby se zajistilo, že databáze vždy obsahuje potřebné prostředky a aby se zabránilo vlivu na ostatní databáze ve fondu. Pokud je tento požadavek předvídatelný, například v místě, kde se předpokládá navýšení prodeje lístků na oblíbenou akci, je možné toto chování správy začlenit do aplikace.

[Azure Portal](https://portal.azure.com) poskytuje integrované monitorování a upozorňování pro většinu prostředků. Ve službě SQL Database je monitorování a upozorňování k dispozici v databázích a fondech. Toto integrované monitorování a upozorňování je specifické pro jednotlivé prostředky, takže je pohodlné používat ho u malého počtu prostředků, ale není příliš pohodlné při práci s mnoha prostředky.

U scénářů s vysokými objemy je možné používat Log Analytics (známou také jako OMS). To je samostatná služba Azure, která poskytuje analýzy přes vydávané diagnostické protokoly a telemetrii získanou v pracovním prostoru Log Analytics, které můžou shromažďovat telemetrii z mnoha služeb a dají se používat k dotazování a nastavování výstrah.

## <a name="get-the-wingtip-application-scripts"></a>Získání skriptů aplikace Wingtip

Skripty a zdrojový kód aplikace Wingtip Tickets jsou k dispozici v úložišti GitHubu [WingtipSaaS](https://github.com/Microsoft/WingtipSaaS). Soubory se skripty jsou ve [složce Learning Modules](https://github.com/Microsoft/WingtipSaaS/tree/master/Learning%20Modules). Stáhněte si složku **Learning Modules** do svého místního počítače. Dejte pozor, abyste zachovali strukturu složky.

## <a name="provision-additional-tenants"></a>Zřízení dalších tenantů

I když fondy můžou být nákladově efektivní jenom se dvěma databázemi S3, platí, že čím více databází fond obsahuje, tím efektivnější z hlediska nákladů začne být efekt zprůměrování. Pro zajištění správného porozumění fungování monitorování a správy výkonu na škále vyžaduje tento kurz, abyste měli nasazených nejméně 20 databází.

Pokud jste v předchozím kurzu zřídili dávku tenantů, můžete přejít do části [Simulace použití ve všech databázích tenantů](#simulate-usage-on-all-tenant-databases).

1. Otevřete skript …\\Learning Modules\\Provision and Catalog\\*Demo-PerformanceMonitoringAndManagement.ps1* v části **Integrované skriptovací prostředí (ISE) v prostředí PowerShell**. Tento skript nechte otevřený, protože během tohoto kurzu budete spouštět několik scénářů.
1. Nastavte **$DemoScenario** = **1**, **Zřízení dávky tenantů**
1. Stisknutím klávesy **F5** spusťte skript.

Skript nasadí 17 tenantů za méně než pět minut.

Skript *New-TenantBatch* používá vnořenou nebo propojenou sadu šablon [Resource Manageru](../azure-resource-manager/index.md), které vytvářejí dávku tenantů. Ta ve výchozím nastavení zkopíruje databázi **baseTenantDb** na katalogový server k vytvoření nových databází tenantů, následně je zaregistruje v katalogu a nakonec dokončí s názvem tenanta a typem místa. To je konzistentní se způsobem, jakým aplikace WTP zřizuje nového tenanta. Jakékoli změny provedené ve skriptu *baseTenantDB* se následně použijí u všech nově zřízených tenantů. Podívejte se do [kurzu Správa schématu](sql-database-saas-tutorial-schema-management.md) na postup provedení změn schématu v *existujících* databázích tenantů (včetně *zlaté* databáze).

## <a name="simulate-different-usage-patterns-by-generating-different-load-types"></a>Simulace různých vzorců použití generováním různých typů zatížení

Skript *Demo-PerformanceMonitoringAndManagement.ps1* spouští generátor zatížení pomocí některého z dostupných *typů zatížení*:

| Ukázka | Scénář |
|:--|:--|
| 2 | Generování normální intenzity zatížení (asi 40 DTU) |
| 3 | Generování zatížení s delšími a častějšími nárůsty zatížení na databázi|
| 4 | Generování zatížení s vyššími nárůsty DTU na databázi (přibližně 80 DTU)|
| 5 | Generování normálního a vysokého zatížení v jednom tenantovi (přibližně 95 DTU)|
| 6 | Generování nevyváženého zatížení mezi více fondy|

## <a name="simulate-usage-on-all-tenant-databases"></a>Simulace využití ve všech databázích tenantů

Generátor zatížení použije *syntetické* zatížení jenom pro CPU na každé databázi tenantů. Generátor spustí úlohu pro každou databázi tenantů, která pravidelně volá uloženou proceduru generující zatížení. Úrovně zatížení (v eDTU), doba trvání a intervaly jsou napříč všemi databázemi různé, což simuluje nepředvídanou činnost tenanta.

1. Nastavte **$DemoScenario** = **2**, *Generování zatížení normální intenzity*.
1. Stisknutím klávesy **F5** použijte zatížení u všech databází tenantů.

Kvůli sporadické povaze zatížení nechte generátor spuštěný 10–20 minut, aby aktivita dosáhla stabilního stavu a usadila se na vyhovujícím vzorci.

> [!IMPORTANT]
> Generátor zatížení běží jako řada úloh v místní relaci prostředí PowerShell. Nechte otevřenou kartu *Demo-PerformanceMonitoringAndManagement.ps1*. Pokud tuto kartu zavřete nebo pozastavíte počítač, generátor zatížení se zastaví.

## <a name="monitor-resource-usage-using-the-portal"></a>Monitorování využití prostředků pomocí portálu

Pokud chcete monitorovat využití prostředků, které vyplývá z použití zatížení, otevřete portál na fondu, který obsahuje databáze tenantů.

1. Otevřete [Azure Portal](https://portal.azure.com) a přejděte na server tenants1-&lt;USER&gt;.
1. Měli byste vidět seznam databází tenantů, včetně nové dávky databází.
1. Přejděte dolů na elastické fondy a klikněte na **Pool1**. Tento fond obsahuje všechny dosud vytvořené databáze tenantů.
1. Rozbalte okno fondu, které se otevře, a sledujte graf využití fondu a graf využití hlavní databáze.

Využití fondu je v podstatě agregace využití databáze u všech databází ve fondu. Graf využití databáze zobrazuje 5 nejvyužívanějších databází:

![](./media/sql-database-saas-tutorial-performance-monitoring/pool1.png)

Protože kromě 5 hlavních databází fond obsahuje další databáze, jeho využití ukazuje aktivitu, která se neprojevuje v grafu pěti hlavních databází. Pokud se chcete podívat na podrobnější údaje, klikněte na **Využití databázových prostředků**:

![](./media/sql-database-saas-tutorial-performance-monitoring/database-utilization.png)


## <a name="set-performance-alerts-on-the-pool"></a>Nastavení výstrah výkonu ve fondu

Nastavte ve fondu výstrahu, která se spustí při \>75% využití po dobu 5 minut, následujícím způsobem:

1. Otevřete *Pool1* (na serveru *tenants1-\<user\>*) na portálu [Azure Portal](https://portal.azure.com).
1. Klikněte na **Pravidla výstrah** a potom na **+ Přidat výstrahu**:

   ![přidání výstrahy](media/sql-database-saas-tutorial-performance-monitoring/add-alert.png)

1. Zadejte název, například **High DTU**, 
1. Nastavte následující hodnoty:
   * **Metrika = procento eDTU**
   * **Podmínka = je větší než**.
   * **Prahová hodnota = 75**.
   * **Období = více než posledních 30 minut**.

   ![nastavení upozornění](media/sql-database-saas-tutorial-performance-monitoring/alert-rule.png)

Můžete si nechat zasílat oznámení na e-mail účtu Azure a volitelně další e-maily (doporučuje se tuto možnost nenastavovat, pokud nepoužíváte vlastní předplatné).

> [!NOTE]
> Protože výstraha zazní pouze v případě, kdy je prahová hodnota překročená na posledních 30 minut, musí být generátor zatížení spuštěný více než 30 minut, aby výstrahu otestoval.


## <a name="scale-up-a-busy-pool"></a>Vertikální navýšení kapacity zaneprázdněného fondu

Pokud se agregovaná úroveň zatížení ve fondu zvýší na bod, která fond navyšuje na maximální hodnotu a dosáhne 100% využití eDTU, má to vliv na individuální výkon databáze a potenciálně vykazuje pomalejší doby odezvy pro všechny databáze ve fondu.

Z krátkodobého hlediska zvažte navýšení kapacity fondu tak, aby poskytoval další prostředky, nebo odeberte databáze z fondu (přemístěním do jiných fondů nebo mimo fond do samostatné servisní vrstvy).

Z dlouhodobého hlediska uvažujte o optimalizaci využití dotazů nebo indexu pro účely vylepšení výkonu databáze. Podle toho, jak je aplikace citlivá na problémy s výkonem, se osvědčilo navýšit kapacitu fondu předtím, než dosáhne 100% využití eDTU. Použijte výstrahu, abyste byli předem upozornění.

Zaneprázdněný fond můžete simulovat tak, že zvýšíte zatížení vytvořené generátorem. Způsobí častější zvýšení provozu databáze a na delší dobu zvýší agregované zatížení ve fondu beze změny požadavků jednotlivých databází. Škálování fondu se snadno provádí na portálu nebo z prostředí PowerShell. Při tomto cvičení se používá portál.

1. Nastavte možnost *$DemoScenario* = **3**, _Generování zatížení s delšími a častějšími nárůsty zatížení na databázi_ na zvýšení intenzity agregovaného zatížení ve fondu beze změny vrcholového zatížení požadovaného jednotlivými databázemi.
1. Stisknutím klávesy **F5** použijte zatížení u všech databází tenantů.


1. **Otevřete okno fondu** **pro atributy tenants1/Pool1**.


1. Sledujte navýšené využití DTU fondu v grafu nahoře. Nové vyšší zatížení se rozjíždí delší dobu, ale měli byste si rychle všimnout, že fond začíná dosahovat 100% využití a s tím, jak se zatížení stabilizuje na novém vzorci, rychle fond přetíží.


1. Pokud chcete vertikálně navýšit kapacitu fondu, klikněte na **Konfigurovat fond**
1. Nastavte posuvník **eDTU fondu** na 100 (doporučuje se nepřekračovat limit nákladů). Všimněte si, že je agregované úložiště dostupné pro všechny databáze ve fondu označené jako **GB fondu** propojené s nastavením eDTU a také se zvýší. Změna eDTU fondu se nemění podle nastavení databáze (což je stále maximálně 50 eDTU na databázi). Na nastavení pro jednotlivé databáze se můžete podívat na pravé straně okna **Konfigurovat fond**.
1. Požadavek odešlete kliknutím na **Uložit**. Změna u standardního fondu obvykle trvá 3–5 minut.

Vraťte se k části **Pool1** > **Přehled** k zobrazení monitorovacích grafů. Monitorujte efekt zřizování fondu s více prostředky (i když s méně databázemi a náhodným zatížením ne vždy probíhá nezvratně). Při interpretaci grafů mějte na paměti, že 100 % v horním grafu teď reprezentuje 100 eDTU, zatímco v dolním grafu je 100 % vždy 50 eDTU, protože maximum na databázi je 50 eDTU.

Databáze je v průběhu procesu online a plně dostupná. A nakonec – protože je každá databáze připravená k aktivaci s eDTU nového fondu, bude každé aktivní propojení přerušeno. Kód aplikace by měl vždy být zapsán tak, aby se znovu pokoušel obnovit porušené propojení a tím pádem se znovu připojí k databázi ve fondu s navýšenou kapacitou.

## <a name="create-a-second-pool-and-load-balance-databases-to-handle-increased-aggregate-load"></a>Vytvoření druhého fondu a databází pro vyrovnávání zatížení ke zvládání zvýšeného agregovaného zatížení

Jako alternativu navýšení kapacity fondu vytvořte druhý fond a přemístěte do něho databáze, aby se vyrovnalo zatížení mezi dvěma fondy. K tomu je potřeba vytvořit tento nový fond na stejném serveru jako první.

1. Otevřete server **okno serveru pro customers1-&lt;USER&gt;**. Pokud jste v okně databáze nebo fondu, můžete procházet základní ovladač a vybrat název serveru jako zástupce.
1. Kliknutím na **+ Nový fond** vytvořte fond na aktuálním serveru
1. V šabloně fondu Nová elastická databáze:

    1. Nastavte **Název = Pool2**.
    1. Cenovou úroveň nechte jako **Fond Standard**.
    1. Klikněte na **Konfigurovat fond**
    1. V okně Konfigurovat fond, které se otevře, nastavte **eDTU fondu = 50 DTU**.
    1. Kliknutím na příkaz **Přidat databáze** zobrazte seznam databází na tomto serveru, které nejsou v aktuálním fondu.
    1. V seznamu **označte** polovinu databází (10 z 20) pro přesunutí těchto databází do nového fondu, a pak klikněte na **Vybrat**.
    1. Znovu klikněte na **Vybrat** pro přijetí změn konfigurace. Poznamenejte si odhad nákladů na jeden měsíc používání s vybranými možnostmi.
    1. Vyberte **OK** k vytvoření nového fondu s novou konfigurací a přesunutí databází.

Vytvoření fondu a přesunutí databází do něho trvá několik minut. Každá z přesouvaných databází zůstává online a plně přístupná až do posledního okamžiku, kdy se všechna otevřená připojení zavřou. Když klient opakuje připojení, připojí se k databázi v novém fondu.

Vytvořený fond se zobrazí v okně serveru customers1. Kliknutím na název fondu otevřete okno fondu a monitorujte jeho výkon.

Měli byste vidět, že využití prostředků v okně Pool1 pokleslo a že podobně zatížený je Pool2.

## <a name="manage-an-increased-load-on-a-single-database"></a>Správa zvýšeného zatížení v izolované databázi

Pokud má izolovaná databáze ve fondu stabilně vysoké zatížení, může mít v závislosti na konfiguraci fondu tendenci dominovat prostředkům ve fondu a ovlivňovat ostatní databáze. Pokud je pravděpodobné, že bude aktivita nějakou dobu pokračovat, je možné databázi dočasně přemístit mimo fond. To umožní poskytovat pro databázi více prostředků, než mají ostatní databáze ve fondu, a izolovat ji od ostatních databází. Toto cvičení simuluje vliv vysokého zatížení při prodeji lístků na populární koncert v Koncertním sále Contoso.

1. Ve skriptu …\\**Demo-PerformanceManagementAndMonitoring**.ps1
1. Nastavte **$DemoScenario = 5, Generování normálního a vysokého zatížení v jednom tenantovi (přibližně 95 DTU).**
1. Nastavte **$SingleTenantDatabaseName = contosoconcerthall**
1. Skript proveďte pomocí **F5**.
1. **Otevřete okno fondu** **pro Customers1/Pool1**.
1. Podívejte se na displej **Monitorování elastického fondu** v horní části okna a hledejte zvýšené využití DTU fondu. Po jedné až dvou minutách by mělo začít zvýšené zatížení a měli byste rychle zaznamenat, že fond dosáhl 100% využití.
1. Monitorujte také displej **Monitorování elastické databáze**, na kterém se zobrazují nejvytíženější databáze za uplynulou hodinu. Databáze contosoconcerthall by se měla brzy zobrazit jako jedna z 5 nejvytíženějších databází.
1. **Klikněte na graf** **monitorování elastické databáze**, který se otevře v okně **Využití databázových prostředků**, kde můžete selektivně monitorovat libovolnou databázi. Díky tomu můžete izolovat zobrazení databáze contosoconcerthall.
1. V seznamu databází **klikněte na contosoconcerthall** a otevře se její databázové okno.
1. Klikněte na **Cenová úroveň (DTU škálování)** v místní nabídce a otevřete okno **Konfigurovat výkon**, kde můžete nastavit izolovanou úroveň výkonu databáze.
1. Kliknutím na kartu **Standardní** otevřete možnosti škálování ve standardní vrstvě.
1. Posuňte **posuvník DTU** doprava a vyberte 100 DTU. Všimněte si, že to odpovídá cíli služby **S3,** který je uveden v závorkách mezi měřítkem DTU a velikostí úložiště.
1. Kliknutím na **Použít** přesuňte databázi mimo fond a nastavte ji jako standardní databázi S3.
1. Po dokončení nasazení sledujte vliv na databázi contosoconcerthall a fond, který byl odebrán z okna elastického fondu a databáze.

Když pak vyšší než normální zatížení databáze contosoconcerthall poklesne, měli byste ji ihned vrátit do fondu, aby se snížilo její zatížení. Jestli není jasné, kdy se to stane, měli byste nastavit výstrahu v databázi, která se aktivuje, jakmile využití DTU ve fondu poklesne pod maximum na databázi. Přesunutí databáze do fondu je popsáno v cvičení 5.

## <a name="other-performance-management-patterns"></a>Další vzorce správy výkonu

**Preventivní škálování** Ve cvičení 6, kde jste si vyzkoušeli škálování izolované databáze, jste zjistili, kterou databázi hledat. Pokud vedení Koncertního sálu Contoso informovalo WTP o nadcházejícím prodeji lístků, byla databáze pravděpodobně preventivně přemístěna z fondu. Jinak by to vyžadovalo výstrahu ve fondu nebo v databázi s cílem zjistit, co se stalo. Pravděpodobně byste to nechtěli zjistit proto, že by si ostatní tenanti ve fondu stěžovali na snížený výkon. A kdyby mohli tenanti předvídat, na jak dlouho by potřebovali další prostředky, můžete nastavit runbook Azure Automation na přesunutí databáze mimo fond a potom zpět podle předem definovaného plánu.

**Samoobslužné škálování tenanta**  Protože škálování je úkol, který se snadno volá prostřednictvím rozhraní API pro škálu, můžete snadno vytvořit možnost škálovat databáze tenantů do aplikace směřující k tenantovi a nabízet ji jako funkci služby SaaS. Můžete například umožnit tenantům samoobslužné vertikální navýšení a snížení kapacity navázané přímo na fakturaci.

### <a name="scaling-a-pool-up-and-down-on-a-schedule-to-match-usage-patterns"></a>Vertikální navýšení a snížení kapacity podle plánu v souladu se vzorci využití

Tam, kde agregované využití tenanta probíhá podle předvídatelných vzorců použití, můžete pomocí Azure Automation podle plánu vertikálně zvyšovat nebo snižovat kapacitu fondu. Můžete například snížit kapacitu fondu po 6. hodině večer a navýšit před 6. hodinou ráno ve dnech, kdy víte, že existuje pokles požadavků na prostředky.



## <a name="next-steps"></a>Další kroky

Co se v tomto kurzu naučíte:

> [!div class="checklist"]
> * Simulace použití v databázích tenantů pomocí dodaného generátoru zatížení
> * Monitorování databází tenantů při reakci na zvýšení zatížení
> * Vertikální navýšení kapacity elastického fondu v reakci na zvýšení zatížení databáze
> * Zřízení druhého elastického fondu pro vyrovnání zatížení činnosti databáze

[Kurz Obnovení jednoho tenanta](sql-database-saas-tutorial-restore-single-tenant.md)


## <a name="additional-resources"></a>Další zdroje

* [Další kurzy, které vycházejí z původně nasazené aplikace WTP (Wingtip Tickets Platform)](sql-database-wtp-overview.md#sql-database-wtp-saas-tutorials)
* [Elastické fondy SQL](sql-database-elastic-pool.md)
* [Azure Automation](../automation/automation-intro.md)
* [Log Analytics](sql-database-saas-tutorial-log-analytics.md) – kurz Nastavení a používání Log Analytics
