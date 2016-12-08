---
title: "Co je SQL Database? Představení služby SQL Database | Dokumentace Microsoftu"
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
ms.date: 11/08/2016
ms.author: shkurhek
translationtype: Human Translation
ms.sourcegitcommit: 1c603d37735bbbfdfaaf4a191e2ad1ce6ff5b2b7
ms.openlocfilehash: 67f3e923680a9a2f399c0839d2ec11ef4615da00


---
# <a name="what-is-sql-database-introduction-to-sql-database"></a>Co je SQL Database? Úvod do služby SQL Database
SQL Database je cloudová služba poskytující relační databáze založené na předním databázovém stroji Microsoft SQL Server a schopné obsloužit i kriticky důležité aplikace. SQL Database nabízí předvídatelný výkon, škálovatelnost bez přerušení provozu, kontinuitu podnikových procesů a ochranu dat – to vše téměř bez nutnosti jakékoli správy. Můžete se zaměřit na rychlý vývoj aplikací a zkrácení doby dodání produktu na trh, namísto správy virtuálních počítačů a infrastruktury. Protože je služba SQL Database založena na databázovém stroji [SQL Server](https://msdn.microsoft.com/library/bb545450.aspx), podporuje stávající nástroje, knihovny a rozhraní API což usnadňuje přesunutí a rozšíření do cloudu.

Tento článek je úvodem do základních koncepcí služby SQL Database a funkcí týkajících se výkonu, škálovatelnosti a možností správy, nabízí ale také odkazy na podrobnější informace. Až budete připraveni, můžete během několika minut [vytvořit svoji první databázi SQL](sql-database-get-started.md) nebo [vytvořit fond elastické databáze](sql-database-elastic-pool-create-portal.md). Máte-li zájem o podrobnější prohlídku, podívejte se na toto 30minutové video.

> [!VIDEO https://channel9.msdn.com/Events/Microsoft-Azure/AzureCon-2015/ACON326/player]
> 
> 

## <a name="adjust-performance-and-scale-without-downtime"></a>Úprava výkonu a škálování bez výpadků
Databáze SQL jsou dostupné v *úrovních služeb* Basic, Standard a Premium. Každá úroveň služeb nabízí [různé úrovně výkonu a možnosti](sql-database-service-tiers.md) pro podporu zátěží všech typů – od nejlehčích k těm nejnáročnějším. Za pár peněz na měsíc můžete sestavit svoji první aplikaci s malou databází a později ručně nebo z programu [změnit úroveň služby](sql-database-scale-up.md), jakmile se vaše aplikace virálně rozšíří do světa. Změny úrovně si vaši zákazníci ani nevšimnou, provoz nijak nenaruší.

Mnoha firmám a aplikacím stačí vytváření databází a nastavování výkonu izolovaných databází na vyžádání, zejména v případě, že jsou vzorce používání relativně předvídatelné. Ale pokud vaše vzorce používání předvídatelné nejsou, může být správa nákladů a údržba obchodního modelu velmi těžká.

[Elastické fondy](sql-database-elastic-pool.md) ve službě SQL Database řeší právě tento problém. Princip je jednoduchý. Přidělíte výkon fondu a platíte za celkový výkon poskytovaný všem databázím ve fondu společně, namísto za výkon jednotlivých databází. Výkon jednotlivých databází už nemusíte navyšovat ani omezovat podle aktuálních požadavků. Databáze ve fondu, označované jako *elastické databáze*, se budou automaticky škálovat podle potřeby v reálném čase. Elastické databáze výkon spotřebovávají, ale nepřekračují omezení fondu, takže vaše náklady budou předvídatelné, i když využívání databáze odhadnutelné nebude. A navíc můžete [přidávat a odebírat databáze ve fondu](sql-database-elastic-pool-manage-portal.md), škálovat aplikace od několik databází k tisícům a přitom mít rozpočet stále pod kontrolou. Další informace o návrhových schématech aplikací SaaS využívajících elastické fondy najdete v tématu [Návrhová schémata pro víceklientské aplikace SaaS využívající službu Azure SQL Database](sql-database-design-patterns-multi-tenancy-saas-applications.md).

Ať už si vyberete kteroukoli cestu – jednotlivé nebo elastické databáze – nejste k ní odsouzeni na věčné časy. Můžete kombinovat izolované databáze s fondy elastické databáze a rychle a snadno měnit úrovně služeb izolovaných databází i celých fondů a přizpůsobit je tak pro vaše konkrétní potřeby. Kromě toho s výkonem a dostupností, které Azure nabízí, můžete se službou SQL Database kombinovat další služby Azure podle konkrétních potřeb vašich aplikací pro zvýšení ekonomičnosti provozu a efektivity prostředků a otevírat tak zcela nové obchodní příležitosti.

Jak ale můžeme srovnávat relativní výkon databází a jejich fondů? Jak poznáme správnou hodnotu nastavení při přidávání nebo ubírání výkonu? Odpověď najdete pomocí integrovaných nástrojů pro sledování výkonu a upozorňování v kombinaci s hodnoceními výkonu na základě jednotek DTU (Database Transaction Unit) pro izolované databáze a elastických DTU (eDTU) pro elastické databáze a fondy databáze. Společně vám pomohou rychle vyhodnotit dopad vertikálního navýšení nebo snížení kapacity v závislosti na vašich aktuálních nebo plánovaných požadavcích na výkon. Podrobnosti viz téma [Výkon a možnosti služby SQL Database: Co je k dispozici v jednotlivých úrovních služeb](sql-database-service-tiers.md).

## <a name="keep-your-app-and-business-running"></a>Udržujte své aplikace a podnikáni v chodu
Dostupnost služby Azure se smlouvou o úrovní služeb [(SLA)](http://azure.microsoft.com/support/legal/sla/) dosahuje špičkové hodnoty 99,99 %, protože staví na globální síti Microsoftem spravovaných datových center. Může tedy udržet vaše aplikace v nepřetržitém provozu každý den po celý rok. S každou službou SQL Database využíváte integrované zabezpečení, odolnost proti chybám a ochranu dat, které byste jinak museli kupovat nebo navrhovat, sestavovat a spravovat. I tak si v závislosti na požadavcích vaší firmy můžete potřebovat další vrstvy ochrany a zajistit, aby se vaše aplikace a firma mohly rychle zotavit v případě katastrofy, chyby nebo čehokoli jiného. SQL Database nabízí v každé úrovni služby komplexní sadu funkcí a možností pro zajištění kontinuity podnikových procesů, které můžete využít k zahájení a udržení provozu. Můžete využít obnovení databáze do určitého bodu v čase, a to až 35 dnů zpět. Kromě toho, pokud dojde k výpadku datového centra hostujícího vaše databáze, můžete databáze obnovit z geograficky redundantních kopií nebo nedávných záloh, případně mohou službu převzít repliky databáze v jiné oblasti. Repliky můžete použít také k upgradu nebo přemístění do jiných oblastí.

![Geografická replikace služby SQL Database](./media/sql-database-technical-overview/azure_sqldb_map.png)

V tématu [Kontinuita podnikových procesů](sql-database-business-continuity.md) najdete podrobnosti o různých možnostech, které jsou v jednotlivých úrovních služby dostupné.

## <a name="secure-your-data"></a>Zabezpečení dat
SQL Server má tradici solidního zabezpečení dat, kterou SQL Database zachovává díky funkcím omezujícím přístup, ochraňujícím data a pomáhajícím s monitorováním aktivit souvisejících s databází. Viz téma [Zabezpečení databáze SQL](sql-database-security.md), kde najdete rychlý přehled možností zabezpečení, které služba SQL Database nabízí. Úplnější přehled funkcí zabezpečení obsahuje téma [Centrum zabezpečení pro databázový stroj SQL Server a SQL Database](https://msdn.microsoft.com/library/bb510589). A nezapomeňte navštívit také [Azure Security Center](https://azure.microsoft.com/support/trust-center/security/), kde najdete informace o bezpečnosti samotné platformy Azure.

## <a name="next-steps"></a>Další kroky
Nyní, když jste si přečetli úvod do služby SQL Database a znáte odpověď na otázku „Co je SQL Database?“, jste připraveni na následující články:

* Na [stránce s cenami](https://azure.microsoft.com/pricing/details/sql-database/) najdete srovnání cen za izolované databáze i fondy a kalkulačky pro výpočet nákladů.
* Další informace [elastických fondech](sql-database-elastic-pool.md).
* Začněte [vytvářet první databáze](sql-database-get-started.md).
* [Připojení a dotazování pomocí SSMS](sql-database-connect-query-ssms.md)
* Začněte vytvářet vaši první aplikaci v jazyce C#, Java, Node.js, PHP, Python nebo Ruby: [Knihovny pro připojení ke službě SQL Database a serveru SQL Server](sql-database-libraries.md).



<!--HONumber=Nov16_HO5-->


