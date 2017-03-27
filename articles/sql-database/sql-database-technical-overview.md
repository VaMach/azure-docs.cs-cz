---
title: "Co je služba Azure SQL Database? | Dokumentace Microsoftu"
description: 'Get an introduction to SQL Database: technical details and capabilities of Microsoft''s relational database management system (RDBMS) in the cloud.'
keywords: "představení sql,úvod do sql,co je sql database"
services: sql-database
documentationcenter: 
author: shontnew
manager: jhubbard
editor: cgronlun
ms.assetid: c561f600-a292-4e3b-b1d4-8ab89b81db48
ms.service: sql-database
ms.custom: overview
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: data-management
ms.date: 03/17/2017
ms.author: shkurhek
translationtype: Human Translation
ms.sourcegitcommit: bb1ca3189e6c39b46eaa5151bf0c74dbf4a35228
ms.openlocfilehash: c505844cc2b7c745a1106b3c446833fb206ca98a
ms.lasthandoff: 03/17/2017

---
# <a name="what-is-sql-database-introduction-to-sql-database"></a>Co je SQL Database? Úvod do služby SQL Database
SQL Database je služba v cloudu Microsoftu poskytující relační databáze založené na předním databázovém stroji Microsoft SQL Server a schopné obsloužit i klíčové úlohy. SQL Database nabízí předvídatelný výkon na několika úrovních služby, dynamickou škálovatelnost bez přerušení provozu, integrovanou provozní kontinuitu a ochranu dat – to vše téměř bez nutnosti jakékoli správy. Díky těmto možnostem se můžete zaměřit na rychlý vývoj aplikací a zkrácení doby dodání produktu na trh, namísto vynakládání prostředků a drahocenného času na správu virtuálních počítačů a infrastruktury. Protože je služba SQL Database založena na stroji [SQL Serveru](https://msdn.microsoft.com/library/bb545450.aspx), podporuje stávající nástroje, knihovny a rozhraní API SQL Serveru. V důsledku toho můžete snadno vyvíjet nová řešení, přesouvat existující řešení SQL Serveru a rozšiřovat stávající řešení SQL Serveru do cloudu Microsoftu bez nutnosti učit se nové dovednosti.

Tento článek je úvodem do základních koncepcí služby SQL Database a funkcí týkajících se výkonu, škálovatelnosti a možností správy, nabízí ale také odkazy na podrobnější informace. Tyto rychlé starty vám pomůžou začít:
 - [Vytvoření databáze SQL na webu Azure Portal](sql-database-get-started-portal.md)  
 - [Vytvoření databáze SQL pomocí Azure CLI](sql-database-get-started-cli.md)
 - [Vytvoření databáze SQL pomocí PowerShellu](sql-database-get-started-powershell.md)

Řadu ukázek v Azure CLI a PowerShellu najdete tady:
 - [Ukázky v Azure CLI pro službu Azure SQL Database](sql-database-cli-samples.md)
 - [Ukázky v Azure PowerShellu pro službu Azure SQL Database](sql-database-powershell-samples.md)

## <a name="adjust-performance-and-scale-without-downtime"></a>Úprava výkonu a škálování bez výpadků
Služba SQL Database nabízí tři úrovně služeb: Basic, Standard a Premium. Každá úroveň služeb nabízí [různé úrovně výkonu a možnosti](sql-database-service-tiers.md) pro podporu zátěží všech typů – od nejlehčích k těm nejnáročnějším. Za pár peněz na měsíc můžete sestavit svou první aplikaci s malou databází a později ručně nebo programově [změnit úroveň služby](sql-database-service-tiers.md), aby splňovala požadavky vašeho řešení. To můžete provést bez výpadku aplikace a bez dopadu na vaše zákazníky. Dynamická škálovatelnost umožňuje databázím transparentně reagovat na rychle se měnící požadavky na prostředky a vy díky tomu platíte pouze za prostředky, které potřebujete, když je potřebujete.

## <a name="elastic-pools-to-maximize-resource-utilization"></a>Elastické fondy pro maximalizaci využití prostředků
Řadě firem a aplikací stačí, že může vytvářet izolované databáze a nastavovat větší nebo menší výkon na vyžádání, zejména při relativně předvídatelném způsobu používání. Ale pokud vaše vzorce používání předvídatelné nejsou, může být správa nákladů a údržba obchodního modelu velmi těžká. [Elastické fondy](sql-database-elastic-pool.md) jsou navržené k řešení tohoto problému. Princip je jednoduchý. Prostředky výkonu přidělujete fondu, nikoli jednotlivým databázím, a platíte za souhrnné prostředky výkonu fondu místo placení za výkon izolovaných databází. S elastickými fondy se nemusíte starat o zvyšování a snižování výkonu databáze s kolísajícími požadavky na prostředky. Databáze ve fondu spotřebovávají prostředky výkonu elastického fondu podle potřeby. Databáze ve fondu spotřebovávají výkon, ale nepřekračují omezení fondu, takže vaše náklady budou předvídatelné, i když využívání jednotlivých databází odhadnutelné nebude. A navíc můžete [přidávat a odebírat databáze ve fondu](sql-database-elastic-pool-manage-portal.md), škálovat aplikace od několik databází k tisícům a přitom mít rozpočet stále pod kontrolou. Nakonec můžete také řídit minimální a maximální prostředky, které mají databáze ve fondu k dispozici, a tím zajistit, že žádná databáze ve fondu nebude využívat všechny prostředky fondu a pro každou databázi ve fondu bude garantováno minimální množství prostředků. Další informace o návrhových schématech aplikací SaaS využívajících elastické fondy najdete v tématu [Návrhová schémata pro víceklientské aplikace SaaS využívající službu Azure SQL Database](sql-database-design-patterns-multi-tenancy-saas-applications.md).

## <a name="blend-single-databases-with-pooled-databases"></a>Kombinace izolovaných databází s databázemi ve fondu
Ať už si vyberete kteroukoli cestu – izolované databáze nebo elastické fondy – nejste k ní odsouzeni na věčné časy. Izolované databáze můžete kombinovat s elastickými fondy a snadno a rychle měnit úrovně služeb izolovaných databází a elastických fondů, abyste je přizpůsobili své situaci. Kromě toho s výkonem a dostupností, které Azure nabízí, můžete se službou SQL Database kombinovat další služby Azure podle konkrétních potřeb vašich aplikací pro zvýšení ekonomičnosti provozu a efektivity prostředků a otevírat tak zcela nové obchodní příležitosti.

## <a name="monitoring-and-alerting"></a>Monitorování a upozorňování
Jak ale můžeme srovnávat relativní výkon izolovaných databází a elastických fondů? Jak poznáme správnou hodnotu nastavení při přidávání nebo ubírání výkonu? Můžete použít [integrované nástroje pro monitorování výkonu](sql-database-performance.md) a [upozorňování](sql-database-insights-alerts-portal.md) v kombinaci s hodnocením výkonu na základě [jednotek DTU (Database Transaction Unit) pro izolované databáze a elastických DTU (eDTU) pro elastické fondy](sql-database-what-is-a-dtu.md). Pomocí těchto nástrojů můžete rychle posoudit dopad vertikálního navýšení nebo snížení kapacity v závislosti na stávajících nebo předpokládaných požadavcích. Podrobnosti viz téma [Výkon a možnosti služby SQL Database: Co je k dispozici v jednotlivých úrovních služeb](sql-database-service-tiers.md).

## <a name="keep-your-app-and-business-running"></a>Udržujte své aplikace a podnikáni v chodu
Dostupnost služby Azure se smlouvou o úrovní služeb [(SLA)](http://azure.microsoft.com/support/legal/sla/) dosahuje špičkové hodnoty 99,99 %, protože staví na globální síti Microsoftem spravovaných datových center. Může tedy udržet vaše aplikace v nepřetržitém provozu každý den po celý rok. S každou databází SQL využíváte integrované zabezpečení, odolnost proti chybám a [ochranu dat](sql-database-automated-backups.md), které byste jinak museli kupovat nebo navrhovat, sestavovat a spravovat. SQL Database nabízí v každé úrovni služby komplexní sadu funkcí a možností pro zajištění kontinuity podnikových procesů, které můžete využít k zahájení a udržení provozu. Můžete využít [obnovení databáze do určitého bodu v čase](sql-database-recovery-using-backups.md), a to až 35 dnů zpět. Můžete nakonfigurovat [dlouhodobé uchovávání záloh](sql-database-long-term-retention.md) a ukládat zálohy v bezpečném trezoru po dobu až 10 let. Kromě toho, pokud dojde k výpadku datového centra hostujícího vaše databáze, můžete databáze obnovit z [geograficky redundantních kopií nedávných záloh](sql-database-recovery-using-backups.md). V případě potřeby můžete také nakonfigurovat [geograficky redundantní repliky pro čtení](sql-database-geo-replication-overview.md) v jedné nebo několika oblastech pro rychlé převzetí služeb při selhání, pokud dojde k výpadku datového centra. Tyto repliky můžete také využít pro zvýšení výkonu při čtení v různých geografických oblastech nebo pro [upgrade aplikací bez výpadků](sql-database-manage-application-rolling-upgrade.md). 

![Geografická replikace služby SQL Database](./media/sql-database-technical-overview/azure_sqldb_map.png)

V tématu [Kontinuita podnikových procesů](sql-database-business-continuity.md) najdete podrobnosti o různých možnostech, které jsou v jednotlivých úrovních služby dostupné.

## <a name="secure-your-data"></a>Zabezpečení dat
SQL Server má tradici zabezpečení dat, kterou služba SQL Database zachovává díky funkcím omezujícím přístup, ochraňujícím data a pomáhajícím s monitorováním aktivity. Viz téma [Zabezpečení databáze SQL](sql-database-security-overview.md), kde najdete rychlý přehled možností zabezpečení, které služba SQL Database nabízí. Úplnější přehled funkcí zabezpečení obsahuje téma [Centrum zabezpečení pro databázový stroj SQL Server a SQL Database](https://msdn.microsoft.com/library/bb510589). A nezapomeňte navštívit také [Azure Security Center](https://azure.microsoft.com/support/trust-center/security/), kde najdete informace o bezpečnosti samotné platformy Azure.

## <a name="next-steps"></a>Další kroky
Nyní, když jste si přečetli úvod do služby SQL Database a znáte odpověď na otázku „Co je SQL Database?“, jste připraveni na následující články:

* Na [stránce s cenami](https://azure.microsoft.com/pricing/details/sql-database/) najdete cenové kalkulačky a srovnání cen izolovaných databází a elastických fondů.
* Další informace [elastických fondech](sql-database-elastic-pool.md).
* Začněte [vytvářet první databáze](sql-database-get-started.md).
* Začněte vytvářet vaši první aplikaci v jazyce C#, Java, Node.js, PHP, Python nebo Ruby: [Knihovny pro připojení ke službě SQL Database a serveru SQL Server](sql-database-libraries.md).

