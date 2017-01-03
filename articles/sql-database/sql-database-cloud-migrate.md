---
title: "Migrace databáze systému SQL Server do služby SQL Database | Dokumentace Microsoftu"
description: "V této části se dozvíte, jak migrovat místní databázi systému SQL Server do služby Azure SQL Database v cloudu. Před migrací databáze otestujte kompatibilitu pomocí nástrojů pro migraci databáze."
keywords: "migrace databáze, migrace databáze systému sql server, nástroje pro migraci databáze, migrace databáze, migrace sql database"
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: 9cf09000-87fc-4589-8543-a89175151bc2
ms.service: sql-database
ms.custom: migrate and move
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: sqldb-migrate
ms.date: 11/08/2016
ms.author: carlrab
translationtype: Human Translation
ms.sourcegitcommit: d9052bd10693c0b7942c0d90fdf89be37b44842d
ms.openlocfilehash: b0093e48266aedda2b6c88b862c0056ebe3b3114


---
# <a name="sql-server-database-migration-to-sql-database-in-the-cloud"></a>Migrace databáze systému SQL Server do služby SQL Database v cloudu
V tomto článku se dozvíte, jak migrovat databázi místního systému SQL Server 2005 nebo novější do služby SQL Azure Database. V tomto procesu migrace databáze migrujete schéma a data z databáze systému SQL Server ve stávajícím prostředí do služby SQL Database. Aby byl tento postup úspěšný, musí stávající databáze nejprve projít testem kompatibility. S příchodem služby SQL Database V12 jsme se začali přibližovat [paritě funkcí](sql-database-features.md) jiné než v případě problému souvisejícího s operacemi na úrovni serveru a mezi databázemi. Databáze a aplikace, které spoléhají na [částečně podporované nebo nepodporované funkce](sql-database-transact-sql-information.md), potřebují určité zpracování, aby se tyto nekompatibility opravily před migrací databáze systému SQL Server.

Migrace vyžaduje, abyste provedli tyto kroky:

* **Testování kompatibility**: Ověřte kompatibilitu databáze se službou SQL Database. 
* **Oprava problémů s kompatibilitou, pokud existují**: Pokud se ověření nepodaří, je potřeba opravit chyby ověření.  
* **Migrace**: Až bude databáze kompatibilní, můžete k provedení migrace použít některou z několika možných metod. 

SQL Server nabízí několik metod provedení těchto úloh. Tento článek obsahuje přehled dostupných metod pro každou úlohu. Následující diagram znázorňuje jednotlivé kroky a metody.

  ![Diagram migrace VSSSDT](./media/sql-database-cloud-migrate/03VSSSDTDiagram.png)

> [!NOTE]
> Pokud chcete migrovat databázi jiného typu než SQL Server, včetně databází Microsoft Access, Sybase, MySQL Oracle a DB2, na databázi Azure SQL Database, získáte informace v tématu [Pomocník s migrací na SQL Server](http://blogs.msdn.com/b/ssma/).
> 
> 

## <a name="database-migration-tools-test-sql-server-database-compatibility-with-sql-database"></a>Nástroje pro migraci databáze testují kompatibilitu databáze systému SQL Server se službou SQL Database
K otestování problémů s kompatibilitou databáze SQL Database před zahájením procesu migrace databáze použijte některou z následujících metod:

> [!div class="op_single_selector"]
> * [SSDT](sql-database-cloud-migrate-fix-compatibility-issues-ssdt.md)
> * [SqlPackage](sql-database-cloud-migrate-determine-compatibility-sqlpackage.md)
> * [SSMS](sql-database-cloud-migrate-determine-compatibility-ssms.md)
> * [SAMW](sql-database-cloud-migrate-fix-compatibility-issues.md)
> 
> 

* [SQL Server Data Tools pro Visual Studio (SSDT)](sql-database-cloud-migrate-fix-compatibility-issues-ssdt.md): SSDT používá nejnovější pravidla kompatibility k zjišťování nekompatibilit služby SQL Database V12. Pokud zjistíte nekompatibility, můžete opravit zjištěné problémy přímo v tomto nástroji. Tato metoda se doporučuje pro testování a opravu problémů s kompatibilitou služby SQL Database V12. 
* [SqlPackage](sql-database-cloud-migrate-determine-compatibility-sqlpackage.md): SqlPackage je nástroj příkazového řádku, který testuje problémy s kompatibilitou a generuje sestavu obsahující zjištěné problémy s kompatibilitou. Pokud chcete tento nástroj použít, ujistěte se, že používáte nejnovější verzi, abyste mohli použít nejnovější pravidla kompatibility. V případě zjištění chyb musíte použít jiný nástroj k opravení všech zjištěných problémů s kompatibilitou – doporučujeme SSDT.  
* [Průvodce aplikací Export Data Tier v sadě SQL Server Management Studio](sql-database-cloud-migrate-determine-compatibility-ssms.md): Tento průvodce zjišťuje a vykazuje chyby na obrazovce. Pokud nejsou zjištěny chyby, můžete pokračovat a dokončit migraci na službu SQL Database. V případě zjištění chyb musíte použít jiný nástroj k opravení všech zjištěných problémů s kompatibilitou – doporučujeme SSDT.
* [SQL Azure Migration Wizard (SAMW)](sql-database-cloud-migrate-fix-compatibility-issues.md): SAMW je nástroj codeplex, který využívá pravidla kompatibility služby Azure SQL Database V11 k zjišťování nekompatibilit služby Azure SQL Database V12. Pokud zjistíte nekompatibilitu, můžete některé problémy opravit přímo v tomto nástroji. Nástroj může najít nekompatibility, které není potřeba opravit. Byl to první dostupný nástroj, který pomáhal s migrací služby Azure SQL Database a má aktivní podporu komunity systému SQL Server. Tento nástroj také dokáže dokončit migraci v rámci vlastního nástroje. 

## <a name="fix-database-migration-compatibility-issues"></a>Řešení problémů s migrací databází
Pokud jsou zjištěny problémy s kompatibilitou, musíte je opravit předtím, než budete pokračovat s migrací databáze systému SQL Server. Můžete narazit na spoustu problémů s kompatibilitou. To závisí na verzi systému SQL Server ve zdrojové databázi i na složitosti databáze, kterou migrujete. Starší verze systému SQL Server mají více problémů s kompatibilitou. Kromě cíleného prohledávání internetu s vybraným vyhledávacím strojem použijte následující prostředky:

* [Funkce databáze systému SQL Server nepodporované ve službě Azure SQL Database](sql-database-transact-sql-information.md)
* [Ukončená funkce databázového stroje v systému SQL Server 2016](https://msdn.microsoft.com/library/ms144262%28v=sql.130%29)
* [Ukončená funkce databázového stroje v systému SQL Server 2014](https://msdn.microsoft.com/library/ms144262%28v=sql.120%29)
* [Ukončená funkce databázového stroje v systému SQL Server 2012](https://msdn.microsoft.com/library/ms144262%28v=sql.110%29)
* [Ukončená funkce databázového stroje v systému SQL Server 2008 R2](https://msdn.microsoft.com/library/ms144262%28v=sql.105%29)
* [Ukončená funkce databázového stroje v systému SQL Server 2005](https://msdn.microsoft.com/library/ms144262%28v=sql.90%29)

Kromě hledání na internetu a využití těchto prostředků použijte [fóra komunity systému MSDN SQL Server](https://social.msdn.microsoft.com/Forums/sqlserver/home?category=sqlserver) nebo [StackOverflow](http://stackoverflow.com/).

K opravě zjištěných problémů použijte některý z následujících nástrojů pro migraci databáze:

> [!div class="op_single_selector"]
> * [SSDT](sql-database-cloud-migrate-fix-compatibility-issues-ssdt.md)
> * [SSMS](sql-database-cloud-migrate-fix-compatibility-issues-ssms.md)
> * [SAMW](sql-database-cloud-migrate-fix-compatibility-issues.md)
> 
> 

* Použití [SQL Server Data Tools for Visual Studio (SSDT)](sql-database-cloud-migrate-fix-compatibility-issues-ssdt.md): Pokud chcete použít SSDT, importujte databázové schéma do SQL Server Data Tools for Visual Studio (SSDT) a vytvořte projekt pro nasazení služby SQL Database V12. Potom odstraňte všechny problémy s kompatibilitou v SSDT. Po dokončení synchronizujte změny zpátky do zdrojové databáze (nebo její kopie). SSDT je v současné době doporučovaná metoda pro testování a opravu problémů s kompatibilitou služby SQL Database V12. Klikněte na odkaz pro [návod používající SSDT](sql-database-cloud-migrate-fix-compatibility-issues-ssdt.md).
* Použití sady [SQL Server Management Studio (SSMS)](sql-database-cloud-migrate-fix-compatibility-issues-ssms.md): Pokud chcete používat SSMS, spusťte příkazy Transact-SQL k opravě chyb zjištěných pomocí jiného nástroje. Tato metoda je primárně určená pro zkušené uživatele k úpravě databázového schématu přímo ve zdrojové databázi. 
* Použití průvodce [SQL Azure Migration Wizard (SAMW)](sql-database-cloud-migrate-fix-compatibility-issues.md): Pokud chcete použít průvodce SAMW, vygenerujte skript Transact-SQL ze zdrojové databáze. Průvodce skript transformuje, kdykoli je to možné, aby bylo schéma kompatibilní se službou SQL Database V12. Po dokončení se SAMW může připojit ke službě SQL Database V12 a spustit skript. Tento nástroj také analyzuje trasovací soubory k určení problémů s kompatibilitou. Skript je možné generovat jenom se schématem nebo může obsahovat data ve formátu BCP.

## <a name="migrate-a-compatible-sql-server-database-to-sql-database"></a>Migrace kompatibilní databáze systému SQL Server do služby SQL Database
Pokud chcete migrovat kompatibilní databázi systému SQL Server, Microsoft nabízí několik metod migrace pro různé scénáře. Metoda, kterou zvolíte, závisí na vaší míře tolerance k prostojům, velikosti a složitosti databáze systému SQL Server a připojení ke cloudu Microsoft Azure.  

> [!div class="op_single_selector"]
> * [SSMS Migration Wizard](sql-database-cloud-migrate-compatible-using-ssms-migration-wizard.md)
> * [Export do souboru BACPAC](sql-database-cloud-migrate-compatible-export-bacpac-ssms.md)
> * [Import ze souboru BACPAC](sql-database-cloud-migrate-compatible-import-bacpac-ssms.md)
> * [Transakční replikace](sql-database-cloud-migrate-compatible-using-transactional-replication.md)
> 
> 

Při výběru metody migrace si nejprve položte otázku, jestli si můžete dovolit během migrace databázi vyjmout z produkce. Migrace databáze v době, kdy probíhají aktivní transakce, může způsobit nekonzistentnosti databáze a její možné poškození. Existuje spousta metod uvedení databáze do nečinnosti od zakázání připojení klientů po vytvoření [snímku databáze](https://msdn.microsoft.com/library/ms175876.aspx).

Pokud chcete mít při migraci minimum prostojů, použijte [replikaci transakce systému SQL Server](sql-database-cloud-migrate-compatible-using-transactional-replication.md). Databáze ale musí splňovat požadavky na transakční replikaci. Pokud si můžete nějaké prostoje dovolit nebo testujete migraci produkční databáze pro pozdější migraci, uvažujte o použití některé z těchto tří metod:

* [SSMS Migration Wizard](sql-database-cloud-migrate-compatible-using-ssms-migration-wizard.md): U malých až středních databází stačí k migraci databáze kompatibilní se systémem SQL Server 2005 nebo novější spustit [průvodce nasazením databáze na databázi Microsoft Azure](sql-database-cloud-migrate-compatible-using-ssms-migration-wizard.md) v sadě SQL Server Management Studio.
* [Export do souboru BACPAC ](sql-database-cloud-migrate-compatible-export-bacpac-ssms.md) a následný [import ze souboru BACPAC](sql-database-cloud-migrate-compatible-import-bacpac-ssms.md): Pokud máte problémy s připojením (žádné připojení, nízká šířka pásma nebo vypršení časového limitu) a používáte středně velké až velké databáze, použijte soubor [BACPAC](https://msdn.microsoft.com/library/ee210546.aspx#Anchor_4). Když využíváte tuto metodu, exportujete schéma systému SQL Server a data do souboru BACPAC. Potom soubor BACPAC exportujete do databáze SQL Database pomocí průvodce Export Data Tier Application Wizard v sadě SQL Server Management Studio nebo nástroje příkazového řádku [SqlPackage](https://msdn.microsoft.com/library/hh550080.aspx).
* Společné použití souboru BACPAC a BCP: U mnohem větších databází použijte soubor [BACPAC](https://msdn.microsoft.com/library/ee210546.aspx#Anchor_4) a [BCP](https://msdn.microsoft.com/library/ms162802.aspx) pro dosažení větší paralelizace ke zvýšení výkonu, spolu s větší složitostí. Pomocí této metody se schéma a data migrují zvlášť.
  
  * [Export schématu jenom do souboru BACPAC](sql-database-cloud-migrate-compatible-export-bacpac-ssms.md).
  * [Import schématu jenom ze souboru BACPAC](sql-database-cloud-migrate-compatible-import-bacpac-ssms.md) do služby SQL Database.
  * Pomocí [BCP](https://msdn.microsoft.com/library/ms162802.aspx) extrahujte data do plochých souborů a následně tyto soubory [paralelně načtěte](https://technet.microsoft.com/library/dd425070.aspx) do služby Azure SQL Database.
    
     ![Migrace databáze systému SQL Server – migrace služby SQL Database do cloudu](./media/sql-database-cloud-migrate/01SSMSDiagram_new.png)

## <a name="next-steps"></a>Další kroky
* [Nejnovější verze rozšíření SSDT](https://msdn.microsoft.com/library/mt204009.aspx)
* [Nejnovější verze aplikace SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx)

## <a name="additional-resources"></a>Další zdroje
* [Funkce služby SQL Database](sql-database-features.md)
  [Částečně podporované nebo nepodporované funkce Transact-SQL](sql-database-transact-sql-information.md)
* [Migrace databází jiného typu než SQL Server s využitím Pomocníka s migrací systému SQL Server](http://blogs.msdn.com/b/ssma/)




<!--HONumber=Jan17_HO1-->


