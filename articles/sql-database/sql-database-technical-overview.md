<properties
    pageTitle="Co je SQL Database? Představení služby SQL Database | Microsoft Azure"
    description="Přečtěte si základní informace o službě SQL Database – technické podrobnosti a možností cloudového systému pro správu relačních databází (RDBMS) společnosti Microsoft."
    keywords="introduction to sql,intro to sql,what is sql database"
    services="sql-database"
    documentationCenter=""
    authors="shontnew"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
   ms.service="sql-database"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="data-management"
   ms.date="05/23/2016"
   ms.author="shkurhek"/>

# Co je SQL Database? Úvod do služby SQL Database

SQL Database je cloudová služba poskytující relační databáze založené na předním databázovém stroji Microsoft SQL Server a schopné obsloužit i kriticky důležité aplikace. SQL Database nabízí předvídatelný výkon, škálovatelnost bez přerušení provozu, kontinuitu podnikových procesů a ochranu dat – to vše téměř bez nutnosti jakékoli správy. Můžete se zaměřit na rychlý vývoj aplikací a zkrácení doby dodání produktu na trh, namísto správy virtuálních počítačů a infrastruktury. Protože je služba SQL Database založena na databázovém stroji [SQL Server](https://msdn.microsoft.com/library/bb545450.aspx), podporuje stávající nástroje, knihovny a rozhraní API což usnadňuje přesunutí a rozšíření do cloudu.

Tento článek je úvodem do základních koncepcí služby SQL Database a funkcí týkajících se výkonu, škálovatelnosti a možností správy, nabízí ale také odkazy na podrobnější informace. Až budete připraveni, můžete během několika minut [vytvořit svoji první databázi SQL](sql-database-get-started.md) nebo [vytvořit fond elastické databáze](sql-database-elastic-pool-create-portal.md). Máte-li zájem o podrobnější prohlídku, podívejte se na toto 30minutové video.

> [AZURE.VIDEO azurecon-2015-get-started-with-azure-sql-database]

## Úprava výkonu a škálování bez výpadků

Databáze SQL jsou dostupné v *úrovních služeb* Basic, Standard a Premium. Každá úroveň služeb nabízí [různé úrovně výkonu a možnosti](sql-database-service-tiers.md) pro podporu zátěží všech typů – od nejlehčích k těm nejnáročnějším. Za pár peněz na měsíc můžete sestavit svoji první aplikaci s malou databází a později ručně nebo z programu [změnit úroveň služby](sql-database-scale-up.md), jakmile se vaše aplikace virálně rozšíří do světa. Změny úrovně si vaši zákazníci ani nevšimnou, provoz nijak nenaruší.

Mnoha firmám a aplikacím stačí vytváření databází a nastavování výkonu izolovaných databází na vyžádání, zejména v případě, že jsou vzorce používání relativně předvídatelné. Ale pokud vaše vzorce používání předvídatelné nejsou, může být správa nákladů a údržba obchodního modelu velmi těžká.

[Elastické fondy](sql-database-elastic-pool.md) ve službě SQL Database řeší právě tento problém. Princip je jednoduchý. Přidělíte výkon fondu a platíte za celkový výkon poskytovaný všem databázím ve fondu společně, namísto za výkon jednotlivých databází. Výkon jednotlivých databází už nemusíte navyšovat ani omezovat podle aktuálních požadavků. Databáze ve fondu, označované jako *elastické databáze*, se budou automaticky škálovat podle potřeby v reálném čase. Elastické databáze výkon spotřebovávají, ale nepřekračují omezení fondu, takže vaše náklady budou předvídatelné, i když využívání databáze odhadnutelné nebude. A navíc můžete [přidávat a odebírat databáze ve fondu](sql-database-elastic-pool-manage-portal.md), škálovat aplikace od několik databází k tisícům a přitom mít rozpočet stále pod kontrolou. Další informace o návrhových schématech aplikací SaaS využívajících elastické fondy najdete v tématu [Návrhová schémata pro víceklientské aplikace SaaS využívající službu Azure SQL Database](sql-database-design-patterns-multi-tenancy-saas-applications.md).

Ať už si vyberete kteroukoli cestu – jednotlivé nebo elastické databáze – nejste k ní odsouzeni na věčné časy. Můžete inovativně míchat izolované databáze s elastickými fondy a měnit úrovně služeb izolovaných databází i celých fondů. Kromě toho s výkonem a dostupností, které Azure nabízí, můžete služby Azure kombinovat se službou SQL Database podle konkrétních potřeb vašich moderních aplikací, rozpočtových možností a efektivity prostředků a otvírat tak zcela nové obchodní příležitosti.

Jak ale můžeme srovnávat relativní výkon databází a jejich fondů? Jak poznáme správnou hodnotu nastavení při přidávání nebo ubírání výkonu? Odpovědí je jednotka DTU (Database Transaction Unit) pro izolované databáze a eDTU (elastická jednotka DTU) pro elastické databáze a fondy. Podrobnosti viz téma [Výkon a možnosti služby SQL Database: Co je k dispozici v jednotlivých úrovních služeb](sql-database-service-tiers.md).

## Udržujte své aplikace a podnikáni v chodu

Dostupnost služby Azure se smlouvou o úrovní služeb [(SLA)](http://azure.microsoft.com/support/legal/sla/) dosahuje špičkové hodnoty 99,99 %, protože staví na globální síti Microsoftem spravovaných datových center. Může tedy udržet vaše aplikace v nepřetržitém provozu každý den po celý rok. S každou databází SQL můžete využít integrovanou ochranu dat, odolnost proti chybám a ochranné mechanismy, které byste jinak museli pracně navrhovat, kupovat, vytvářet a spravovat. I tak si v závislosti na požadavcích vaší firmy můžete vyžádat další vrstvy ochrany a zajistit, aby se vaše aplikace a firmu mohla rychle zotavit v případě přírodní katastrofy, lidské chyby nebo čehokoli jiného. Služba SQL Database nabízí v každé úrovni služeb jinou sadu možností, které můžete využít k zahájení a udržení provozu. Můžete využít obnovení databáze do určitého bodu v čase, a to až 35 dnů zpět. Kromě toho, pokud dojde k výpadku datového centra hostujícího vaše databáze, mohou službu převzít repliky databáze v jiné oblasti. Nebo můžete použít repliky k upgradu nebo přemístění do jiných oblastí.

![Geografická replikace služby SQL Database](./media/sql-database-technical-overview/azure_sqldb_map.png)


V tématu [Kontinuita podnikových procesů](sql-database-business-continuity.md) najdete podrobnosti o různých možnostech, které jsou v jednotlivých úrovních služby dostupné.

## Zabezpečení dat
SQL Server má tradici solidního zabezpečení dat, kterou SQL Database zachovává díky funkcím omezujícím přístup, ochraňujícím data a pomáhajícím s monitorováním aktivit souvisejících s databází. Viz téma [Zabezpečení databáze SQL](sql-database-security.md), kde najdete rychlý přehled možností zabezpečení, které služba SQL Database nabízí. Úplnější přehled funkcí zabezpečení obsahuje téma [Centrum zabezpečení pro databázový stroj SQL Server a SQL Database](https://msdn.microsoft.com/library/bb510589). A nezapomeňte navštívit také [Azure Security Center](https://azure.microsoft.com/support/trust-center/security/), kde najdete informace o bezpečnosti samotné platformy Azure.

## Další kroky
Nyní, když jste si přečetli úvod do služby SQL Database a znáte odpověď na otázku „Co je SQL Database?“, jste připraveni na následující články:

- Na [stránce s cenami](https://azure.microsoft.com/pricing/details/sql-database/) najdete srovnání cen za izolované databáze i fondy a kalkulačky pro výpočet nákladů.
- Další informace [elastických fondech](sql-database-elastic-pool.md).
- Začněte [vytvářet první databáze](sql-database-get-started.md).
- [Připojení a dotazování pomocí SSMS](sql-database-connect-query-ssms.md)
- Začněte vytvářet vaši první aplikaci v jazyce C#, Java, Node.js, PHP, Python nebo Ruby: [Knihovny pro připojení ke službě SQL Database a serveru SQL Server](sql-database-libraries.md).
- Můžete také využít rejstřík všech článků včetně popisů na stránce [Všechna témata týkající se služby Azure SQL Database](sql-database-index-all-articles.md).



<!--HONumber=Jun16_HO2-->


