---
title: "Kdy je vhodné používat elastický fond?"
description: "Elastický fond je kolekce dostupných prostředků, které sdílí skupina elastických databází. Tento dokument obsahuje pokyny, které vám pomohou vyhodnotit vhodnost použití elastického fondu pro skupinu databází."
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: 3d3941d5-276c-4fd2-9cc1-9fe8b1e4c96c
ms.service: sql-database
ms.custom: multiple databases
ms.devlang: NA
ms.date: 12/19/2016
ms.author: sstein;carlrab
ms.workload: data-management
ms.topic: get-started-article
ms.tgt_pltfrm: NA
translationtype: Human Translation
ms.sourcegitcommit: 6c8420a154d998aa95c0220049ee54b3039a872b
ms.openlocfilehash: a79b78a4e8e683afe5b41a41911e7d5f020eff88


---
# <a name="when-should-an-elastic-pool-be-used"></a>Kdy je vhodné používat elastický fond?
Na základě způsobů využití databází a cenových rozdílů mezi elastickým fondem a samostatnými databázemi vyhodnoťte, jestli se použití elastického fondu vyplatí. Poskytujeme také další pokyny, které vám pomůžou určit aktuální požadovanou velikost fondu pro stávající sadu databází SQL.  

* Přehled fondů najdete v tématu [Elastické fondy služby SQL Database](sql-database-elastic-pool.md).

> [!NOTE]
> Elastické fondy jsou obecně dostupné ve všech oblastech Azure s výjimkou oblasti Západní Indie, kde jsou aktuálně ve verzi Preview. Obecná dostupnost elastických fondů v této oblasti bude zajištěna co nejdříve.
>
>

## <a name="elastic-pools"></a>Elastické fondy
Vývojáři SaaS sestavují aplikace nad datovými úrovněmi velkého rozsahu, které se skládají z několika databází. Běžným aplikačním postupem je zřídit pro každého zákazníka izolovanou databázi. Ale různí zákazníci mají často proměnlivé a nepředvídatelné vzorce využití a je těžké odhadnout požadavky jednotlivých databázových uživatelů na prostředky. Proto vývojáři mohou za cenu značných nákladů prostředky naddimenzovat a zajistit tak příznivou propustnost a dobu odezvy pro všechny databáze. Nebo mohou utratit méně a riskovat, že jejich zákazníky ovlivní nízký výkon. Další informace o návrhových schématech aplikací SaaS využívajících elastické fondy najdete v tématu [Návrhová schémata pro víceklientské aplikace SaaS využívající službu Azure SQL Database](sql-database-design-patterns-multi-tenancy-saas-applications.md).

Elastické fondy v Azure SQL Database umožňují vývojářům SaaS optimalizovat poměr cena/výkon pro skupinu databází v rámci předem daného rozpočtu a pro všechny databáze přitom zajistit elasticitu výkonu. Fondy umožňují vývojářům zakoupit pro fond sdílený více databázemi jednotky eDTU (elastic Database Transaction Unit), které umožňují zajistit nepředvídatelná období využití ze strany jednotlivých databází. Požadavek fondu na eDTU je určený agregačním využitím jeho databází. Rozpočet vývojáře určuje počet jednotek eDTU, které má fond k dispozici. Fondy umožňují vývojářům u jednotlivých fondů snadno porovnat vliv rozpočtu na výkon a naopak. Vývojář jednoduše do fondu přidá databáze, nastaví minimální a maximální počet jednotek eDTU pro tyto databáze a potom na základě svého rozpočtu nastaví eDTU fondu. Vývojáři mohou fondy využít k tomu, aby zajistili elegantní růst svých služeb od úsporného startupu až po zralé podnikání, a to ve stále se zvětšujícím měřítku.  

## <a name="when-to-consider-a-pool"></a>Kdy zvažovat použití fondu
Fondy jsou vhodné pro velký počet databází s konkrétními vzory využití. Pro danou databázi je tento vzor charakterizován nízkou mírou průměrného využití s relativně málo častými nárůsty využití.

Čím více databází je možné do fondu přidat, tím větší budou vaše úspory. V závislosti na vzorech využití aplikací je možné dosáhnout úspor už u pouhých dvou databází S3.  

Následující části vás seznámí s postupy, pomocí kterých můžete vyhodnotit, jestli pro vaši konkrétní kolekci databází bude použití fondu přínosné. V příkladech se používají fondy Standard, ale stejné principy platí také pro fondy Basic a Premium.

### <a name="assessing-database-utilization-patterns"></a>Vyhodnocení vzorů využití databáze
Na následujícím obrázku je příklad databáze, která je většinu doby nečinná, ale má také pravidelné špičky aktivity. Tento vzor využití se pro fond skvěle hodí:

   ![izolovaná databáze vhodná pro fond](./media/sql-database-elastic-pool-guidance/one-database.png)

Po uvedená pětiminutová období DB1 využívá až 90 jednotek DTU, ale celkové průměrné využití nedosahuje ani pěti jednotek DTU. Ke spuštění této úlohy v izolované databázi se vyžaduje úroveň výkonu S3. To ale znamená, že v období nízké aktivity je většina prostředků nevyužitá.

Fond umožňuje sdílet tyto nevyužité jednotky DTU napříč několika databázemi a snižuje tak počet potřebných jednotek DTU a celkové náklady.

Využijeme předchozí příklad a budeme předpokládat, že existují další databáze s podobnými vzory využití jako DB1. Na následujících dvou obrázcích je využití čtyř databází a 20 databází zakresleno do stejného grafu. Vidíte tak, že se jejich využití v čase nepřekrývá:

   ![Čtyři databáze se vzorem využití, který je vhodný pro fond](./media/sql-database-elastic-pool-guidance/four-databases.png)

  ![Dvacet databází se vzorem využití, který je vhodný pro fond](./media/sql-database-elastic-pool-guidance/twenty-databases.png)

Agregované využití DTU napříč všemi 20 databázemi je na předchozím obrázku znázorněné černou čárou. Ukazuje se, že agregované využití DTU nikdy nepřekračuje 100 jednotek DTU. Znamená to, že těchto 20 databází může v průběhu tohoto časového období sdílet 100 jednotek eDTU. Výsledkem je 20krát nižší počet DTU a 13krát nižší cena ve srovnání se zařazením jednotlivých databází do úrovní výkonu S3 pro izolované databáze.

Tento příklad je ideální z následujících důvodů:

* Ukazuje velké rozdíly mezi využitím ve špičce a průměrným využitím jednotlivých databází.  
* Špičky využití pro jednotlivé databáze nastávají v různých časových okamžicích.
* Jednotky eDTU jsou sdílené mezi mnoha databázemi.

Cena za fond závisí na jednotkách eDTU fondu. Přestože je cena ze jednotku eDTU pro fond 1,5krát vyšší než cena za jednotku DTU pro izolovanou databázi, **jednotky eDTU fondu může sdílet velký počet databází, a proto stačí menší celkový počet jednotek eDTU**. Tyto rozdíly v cenách a sdílení jednotek eDTU jsou základem potenciálních úspor, které fondy mohou nabídnout.  

Následující hrubé odhady související s počtem databází a jejich využitím pomáhají zajistit, že fond ve srovnání s použitím úrovní výkonu pro izolované databáze poskytuje snížení nákladů.

### <a name="minimum-number-of-databases"></a>Maximální počet databází
Pokud je součet jednotek DTU úrovní výkonu pro izolované databáze větší než 1,5násobek počtu jednotek eDTU potřebných pro fond, je elastický fond cenově výhodnější. Dostupné velikosti najdete v tématu [Omezení úložiště a eDTU pro elastické fondy a elastické databáze](sql-database-elastic-pool.md#edtu-and-storage-limits-for-elastic-pools).

***Příklad***<br>
K tomu, aby fond se 100 jednotkami eDTU byl cenově výhodnější než použití úrovní výkonu pro izolované databáze, jsou potřeba nejméně dvě databáze S3 nebo nejméně 15 databází S0.

### <a name="maximum-number-of-concurrently-peaking-databases"></a>Maximální počet databází se souběžnými špičkami
Vzhledem k tomu, že jednotky eDTU se sdílejí, nemohou je současně využít všechny databáze ve fondu limit až po limit dostupný při použití úrovní výkonu pro izolované databáze. Čím méně databází má současně špičku, tím nižší počet jednotek eDTU ve fondu je možné nastavit a tím výhodnější fond bude. Obecně platí, že svůj limit eDTU by mělo současně využívat nejvýše 2/3 (nebo 67 %) databází ve fondu.

***Příklad***<br>
Aby bylo možné snížit náklady pro tři databáze S3 ve fondu s 200 jednotkami eDTU, mohou nejvýše dvě z těchto databází dosahovat špičky svého využití současně. Pokud současně dosahují špičky více než dvě z těchto čtyř databází S3, bylo by nutné velikost fondu nastavit na více než 200 jednotek eDTU. Pokud se velikost fondu nastaví na více než 200 jednotek eDTU, bude nutné do fondu přidat další databáze S3, jinak náklady nebudou nižší než při použití úrovní výkonu pro izolované databáze.

Všimněte si, tento příklad nebere v úvahu využití ostatních databází ve fondu. Pokud se v libovolném konkrétním časovém okamžiku do určité míry využívají všechny databáze, může méně než 2/3 (nebo 67 %) z nich dosahovat špičky současně.

### <a name="dtu-utilization-per-database"></a>Využití DTU na databázi
Velký rozdíl mezi maximálním a průměrným využitím databáze ukazuje na delší doby nízkého využití a krátká období vysokého využití. Tento vzor využití je ideální pro sdílení prostředků mezi databázemi. Použití fondu pro databázi byste měli zvážit, pokud je její využití ve špičce přibližně 1,5krát větší než průměrné využití.

***Příklad***<br>
Databáze S3, která ve špičce využívá 100 DTU a průměrně využívá 67 DTU nebo méně, je vhodným kandidátem pro sdílení jednotek eDTU ve fondu. Databáze S1, která ve špičce využívá 20 DTU a průměrně využívá 13 DTU nebo méně, je vhodným kandidátem pro fond.

## <a name="sizing-an-elastic-pool"></a>Nastavení velikosti elastického fondu
Ideální velikost fondu závisí na agregovaných jednotkách eDTU a prostředcích úložiště potřebných pro všechny databáze ve fondu. To znamená, že je potřeba určit větší z následujících dvou hodnot:

* Maximální počet jednotek DTU využitých všemi databázemi ve fondu
* Maximální počet bajtů úložiště využitých všemi databázemi ve fondu

Dostupné velikosti najdete v tématu [Omezení úložiště a eDTU pro elastické fondy a elastické databáze](sql-database-elastic-pool.md#edtu-and-storage-limits-for-elastic-pools).

SQL Database automaticky vyhodnotí historické údaje používání prostředků databází na existujícím serveru SQL Database a doporučí odpovídající konfigurace fondu na webu Azure Portal. Kromě těchto doporučení integrované prostředí odhaduje využití eDTU pro vlastní skupinu databází na serveru. Interaktivním přidáváním databází do fondu a jejich odebíráním díky tomu můžete prostřednictvím citlivostních analýz získat analýzy využití prostředků a rady týkající se požadované velikosti dřív, než potvrdíte požadované změny. Postupy najdete v tématu [Monitorování, správa a nastavení velikosti elastického fondu](sql-database-elastic-pool-manage-portal.md).

Flexibilnější vyhodnocení využití prostředků, které umožňuje odhady velikosti ad hoc pro servery dřívější než V12 a odhady velikosti pro databáze na různých serverech, najdete v tématu [Skript prostředí PowerShell pro určení databází vhodných pro elastický fond](sql-database-elastic-pool-database-assessment-powershell.md).

| Schopnost | Prostředí portálu | Skript PowerShellu |
|:--- |:--- |:--- |
| Členitost |15 sekund |15 sekund |
| Zvažuje cenové rozdíly mezi fondem a úrovněmi výkonu pro izolované databáze |Ano |Ne |
| Umožňuje přizpůsobení seznamu analyzovaných databází |Ano |Ano |
| Umožňuje přizpůsobení časového období použitého při analýze |Ne |Ano |
| Umožňuje přizpůsobení seznamu databází, které se analyzují napříč různými servery |Ne |Ano |
| Umožňuje přizpůsobení seznamu databází, které se analyzují na serverech v11 |Ne |Ano |

V případech, kdy nejde používat nástroje, vám při odhadování, jestli je fond cenově výhodnější než izolované databáze, pomůže následující postup:

1. Následujícím způsobem odhadněte počet jednotek eDTU potřebných pro fond:

   MAX(<*celkový počet databází* X *průměrné využití DTU na databázi*>,<br>
   <*počet databází se souběžnou špičkou* X *využití DTU ve špičce na databázi*)
2. Odhadněte potřebnou velikost úložiště pro fond (sečtěte počet bajtů potřebných pro všechny databáze ve fondu). Potom určete velikost fondu v jednotkách eDTU, která toto úložiště poskytuje. Omezení úložiště fondů na základě velikosti fondu v jednotkách eDTU najdete v tématu [Omezení úložiště a eDTU pro elastické fondy a elastické databáze](sql-database-elastic-pool.md#edtu-and-storage-limits-for-elastic-pools).
3. Použijte větší z odhadovaného počtu eDTU z kroku 1 a kroku 2.
4. Prohlédněte si [stránku s cenami SQL Database](https://azure.microsoft.com/pricing/details/sql-database/) a najděte nejmenší velikost fondu v jednotkách eDTU, která je větší než odhad z kroku 3.
5. Porovnejte cenu fondu z kroku 5 s cenou při použití odpovídajících úrovní výkonu pro izolované databáze.

## <a name="summary"></a>Souhrn
Ne všechny izolované databáze jsou optimálními kandidáty pro fondy. Vynikajícími kandidáty jsou databáze se vzorcem využití, pro který je charakteristické nízké průměrné využití a poměrně málo četné špičky využití. Vzorce využití aplikací jsou dynamické vzorce, proto informace a nástroje popsané v tomto článku využijte k úvodnímu posouzení toho, jestli pro některé nebo všechny vaše databáze je fond dobrou volbou. Tento článek je jenom výchozím bodem, který usnadní vaše rozhodování, jestli použití elastického fondu je nebo není vhodné. Nezapomeňte, že byste měli průběžně monitorovat historické využití prostředků a neustále znovu vyhodnocovat úrovně výkonu všech vašich databází. Mějte na paměti, že databáze můžete snadno přesouvat do a z elastických fondů. Pokud máte velký počet databází, může mít několik fondů různých velikostí, mezi které můžete svoje databáze rozdělit.

## <a name="next-steps"></a>Další kroky
* [Vytvoření elastického fondu](sql-database-elastic-pool-create-portal.md)
* [Monitorování, správa a nastavení velikosti elastického fondu](sql-database-elastic-pool-manage-portal.md)
* [Výkon a možnosti služby SQL Database: Co je k dispozici v jednotlivých úrovních služeb](sql-database-service-tiers.md)
* [Skript prostředí PowerShell pro určení databází vhodných pro elastický fond](sql-database-elastic-pool-database-assessment-powershell.md)



<!--HONumber=Jan17_HO1-->


