---
title: "Správa služby SQL Database pomocí SSMS | Dokumentace Microsoftu"
description: "Správa serverů a databází služby SQL Database v nástroji SQL Server Management Studio"
services: sql-database
documentationcenter: .net
author: stevestein
manager: jhubbard
editor: tysonn
ms.assetid: da6f3608-5993-4134-a497-ff2811e9f31f
ms.service: sql-database
ms.custom: overview
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 09/29/2016
ms.author: sstein
translationtype: Human Translation
ms.sourcegitcommit: d9ff74a49742fa77f5989b8b05e0567e3ca81dc5
ms.openlocfilehash: 89cb8827745b31b3a77b64d5cafd586957d60d30


---
# <a name="managing-azure-sql-database-using-sql-server-management-studio"></a>Správa služby Azure SQL Database pomocí nástroje SQL Server Management Studio
> [!div class="op_single_selector"]
> * [Azure Portal](sql-database-manage-portal.md)
> * [SSMS](sql-database-manage-azure-ssms.md)
> * [PowerShell](sql-database-manage-powershell.md)
> 
> 

SQL Server Management Studio (SSMS) umožňuje spravovat servery a databáze služby Azure SQL Database. Toto téma vás provede běžnými úlohami v nástroji SSMS. Než začnete, měli byste už mít ve službě Azure SQL Database vytvořený server a databázi. Další informace najdete v tématech [Vytvoření první databáze služby Azure SQL Database](sql-database-get-started.md) a [Připojení a dotazy pomocí SSMS](sql-database-connect-query-ssms.md).

> [!TIP]
> Pokud potřebujete kurz, ve kterém se naučíte vytvořit server, vytvořit bránu firewall založenou na serveru, zobrazit vlastnosti serveru, připojit se pomocí nástroje SQL Server Management Studio, vytvářet dotazy v hlavní databázi, vytvořit vzorovou databázi a prázdnou databázi nebo se připojit pomocí nástroje SQL Server Management Studio a vytvářet dotazy ve vzorové databázi, přejděte na stránku [kurzu Začínáme](sql-database-get-started.md).
>

Při práci se službou Azure SQL Database doporučujeme vždy používat nejnovější verzi nástroje SSMS. 

> [!IMPORTANT]
> Vždy používejte nejnovější verzi SSMS, protože nástroj neustále vylepšujeme tak, aby fungoval s nejnovějšími aktualizacemi služeb Azure a SQL Database. Pokud chcete získat nejnovější verzi, projděte si téma [Stažení nástroje SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx).
> 
> 

## <a name="create-and-manage-azure-sql-databases"></a>Vytvoření a správa databází služby Azure SQL Database
Pokud jste připojeni k **hlavní** databázi, můžete vytvářet databáze na serveru a měnit nebo odstraňovat existující databáze. Následující kroky popisují, jak provádět některé běžné úlohy správy databází v Management Studiu. Abyste mohli provádět tyto úlohy, ujistěte se, že jste k **hlavní** databázi připojeni pomocí hlavního přihlášení na úrovni serveru, které jste vytvořili při vytvoření vašeho serveru.

Pokud chcete otevřít okno dotazu v Management Studiu, otevřete složku databází, rozbalte složku **Systémové databáze**, klikněte pravým tlačítkem na **hlavní**a potom klikněte na **Nový dotaz**.

* Pomocí příkazu **CREATE DATABASE** vytvoříte databázi. Další informace najdete v tématu [CREATE DATABASE (SQL Database)](https://msdn.microsoft.com/library/dn268335.aspx). Následující příkaz vytvoří databázi s názvem **myTestDB** a určí, že se bude jednat o databázi edice Standard S0 s výchozí maximální velikostí 250 GB.
  
      CREATE DATABASE myTestDB
      (EDITION='Standard',
       SERVICE_OBJECTIVE='S0');

Kliknutím na **Spustit** dotaz spustíte.

* Příkaz **ALTER DATABASE** můžete použít k upravení existující databáze, například pokud chcete změnit název a edici databáze. Další informace najdete v tématu [ALTER DATABASE (SQL Database)](https://msdn.microsoft.com/library/ms174269.aspx). Následující příkaz upraví databázi, kterou jste vytvořili v předchozím kroku, a změní její edici na Standard S1.
  
      ALTER DATABASE myTestDB
      MODIFY
      (SERVICE_OBJECTIVE='S1');
* Pomocí příkazu **DROP DATABASE** můžete odstranit existující databázi. Další informace najdete v tématu [DROP DATABASE (SQL Database)](https://msdn.microsoft.com/library/ms178613.aspx). Následující příkaz odstraní databázi **myTestDB**, ale teď ji neodstraňujte, protože ji budete potřebovat k vytvoření přihlášení v dalším kroku.
  
      DROP DATABASE myTestBase;
* Hlavní databáze obsahuje zobrazení **sys.databases**, které můžete použít k zobrazení podrobností o všech databázích. Pokud chcete zobrazit všechny existující databáze, spusťte následující příkaz:
  
      SELECT * FROM sys.databases;
* Ve službě SQL Database není příkaz **USE** podporován pro přepínání mezi databázemi. Místo toho musíte vytvořit připojení přímo k cílové databázi.

> [!NOTE]
> Řada příkazů Transact-SQL pro vytváření nebo úpravy databází musí být spouštěna v rámci samostatné vlastní dávky a není možné je seskupit s dalšími příkazy Transact-SQL. Další informace najdete v informacích pro konkrétní příkazy.
> 
> 

## <a name="create-and-manage-logins"></a>Vytváření a správa přihlášení
**Hlavní** databáze obsahuje přihlášení a určuje, která přihlášení mají oprávnění k vytváření databází nebo dalších přihlášení. Přihlášení můžete spravovat, pokud se připojíte k **hlavní** databázi pomocí hlavního přihlášení na úrovni serveru, které jste vytvořili při vytvoření vašeho serveru. Pomocí příkazů **CREATE LOGIN**, **ALTER LOGIN** a **DROP LOGIN** můžete spouštět dotazy v hlavní databázi, která spravuje přihlášení v rámci celého serveru. Další informace najdete v tématu [Správa databází a přihlášení ve službě SQL Database](http://msdn.microsoft.com/library/azure/ee336235.aspx). 

* Příkaz **CREATE LOGIN** slouží k vytvoření přihlášení na úrovni serveru. Další informace najdete v tématu [CREATE LOGIN (SQL Database)](https://msdn.microsoft.com/library/ms189751.aspx). Následující příkaz vytvoří přihlášení s názvem **login1**. Hodnotu **password1** nahraďte heslem, které jste si zvolili.
  
      CREATE LOGIN login1 WITH password='password1';
* Příkaz **CREATE USER** slouží k udělení oprávnění na úrovni databáze. Všechna přihlášení musí být vytvořena v **hlavní** databázi. Pokud chcete vytvořit přihlášení pro připojení k jiné databázi, musíte mu v této databázi udělit oprávnění na úrovni databáze pomocí příkazu **CREATE USER**. Další informace najdete v tématu [CREATE USER (SQL Database)](https://msdn.microsoft.com/library/ms173463.aspx). 
* Pokud chcete přihlášení login1 udělit oprávnění k databázi s názvem **myTestDB**, proveďte následující kroky:
  
  1. Aktualizujte Průzkumník objektů, aby se zobrazila databáze **myTestDB**, kterou jste vytvořili. To provedete kliknutím pravým tlačítkem na název serveru v Průzkumníku objektů a potom kliknutím na **Aktualizovat**.  
     
     Pokud jste připojení zavřeli, můžete se znovu připojit výběrem možnosti **Připojit Průzkumník objektů** v nabídce Soubor.
  2. Klikněte na databázi **myTestDB** pravým tlačítkem a pak klikněte na **Nový dotaz**.
  3. Spuštěním následujícího příkazu v databázi myTestDB vytvoříte uživatele databáze s názvem **login1User**, který odpovídá přihlášení na úrovni serveru s názvem **login1**.
     
         CREATE USER login1User FROM LOGIN login1;
* Pomocí uložené procedury **sp\_addrolemember** můžete tomuto uživatelskému účtu udělit odpovídající úroveň oprávnění v databázi. Další informace najdete v tématu [sp_addrolemember (Transact-SQL)](http://msdn.microsoft.com/library/ms187750.aspx). Následující příkaz přidá uživatele **login1User** do role **db\_datareader** a udělí tak uživateli **login1User** v databázi oprávnění jen pro čtení.
  
      exec sp_addrolemember 'db_datareader', 'login1User';    
* Příkaz **ALTER LOGIN** můžete použít k úpravě existujícího přihlášení, například pokud chcete změnit heslo pro přihlášení. Další informace najdete v tématu [ALTER LOGIN (SQL Database)](https://msdn.microsoft.com/library/ms189828.aspx). Příkaz **ALTER LOGIN** spouštějte v **hlavní** databázi. Přejděte zpátky do okna dotazu, který je připojený k této databázi. Následující příkaz upraví přihlášení **login1** a resetuje jeho heslo. Hodnotu **newPassword** nahraďte heslem, které si zvolíte, a **oldPassword** nahraďte aktuálním heslem přihlášení.
  
      ALTER LOGIN login1
      WITH PASSWORD = 'newPassword'
      OLD_PASSWORD = 'oldPassword';
* Pomocí příkazu **DROP LOGIN** můžete odstranit existující přihlášení. Odstranění přihlášení na úrovni serveru odstraní také všechny přidružené uživatelské účty v databázích. Další informace najdete v tématu [DROP DATABASE (SQL Database)](https://msdn.microsoft.com/library/ms178613.aspx). Příkaz **DROP LOGIN** spouštějte v **hlavní** databázi. Tento příkaz odstraní přihlášení **login1**.
  
      DROP LOGIN login1;
* Hlavní databáze obsahuje zobrazení **sys.sql\_logins**, které můžete použít k zobrazení přihlášení. Pokud chcete zobrazit všechna existující přihlášení, spusťte následující příkaz:
  
      SELECT * FROM sys.sql_logins;

## <a name="monitor-sql-database-using-dynamic-management-views"></a>Monitorování služby SQL Database pomocí zobrazení dynamické správy
Služba SQL Database podporuje několik zobrazení dynamické správy, která umožňují monitorovat jednotlivé databáze. Níže je několik příkladů typů dat monitorování, která můžete načíst pomocí těchto zobrazení. Veškeré podrobnosti a další příklady použití najdete v tématu [Monitorování služby SQL Database pomocí zobrazení dynamické správy](https://msdn.microsoft.com/library/azure/ff394114.aspx).

* Dotaz na zobrazení dynamické správy vyžaduje oprávnění **VIEW DATABASE STATE**. Pokud chcete udělit oprávnění **VIEW DATABASE STATE** konkrétnímu uživateli databáze, připojte se k databázi a spusťte v této databázi následující příkaz:
  
      GRANT VIEW DATABASE STATE TO login1User;
* Pomocí zobrazení **sys.dm\_db\_partition\_stats** můžete vypočítat velikost databáze. Zobrazení **sys.dm\_db\_partition\_stats** vrátí informace o počtu stránek a řádků pro každý oddíl databáze, které můžete použít k výpočtu velikosti databáze. Následující dotaz vrátí velikost databáze v megabajtech:
  
      SELECT SUM(reserved_page_count)*8.0/1024
      FROM sys.dm_db_partition_stats;   
* Zobrazení **sys.dm\_exec\_connections** a **sys.dm\_exec\_sessions** slouží k načtení informací o aktuálních připojeních uživatelů a interních úlohách souvisejících s databází. Následující dotaz vrátí informace o aktuálním připojení:
  
      SELECT
          e.connection_id,
          s.session_id,
          s.login_name,
          s.last_request_end_time,
          s.cpu_time
      FROM
          sys.dm_exec_sessions s
          INNER JOIN sys.dm_exec_connections e
            ON s.session_id = e.session_id;
* Zobrazení **sys.dm\_exec\_query\_stats** umožňuje načíst agregovanou statistiku výkonu plánů dotazů v mezipaměti. Následující dotaz vrátí informace o prvních pěti dotazech seřazených podle průměrné doby využití procesoru.
  
      SELECT TOP 5 query_stats.query_hash AS "Query Hash",
          SUM(query_stats.total_worker_time), SUM(query_stats.execution_count) AS "Avg CPU Time",
          MIN(query_stats.statement_text) AS "Statement Text"
      FROM
          (SELECT QS.*,
          SUBSTRING(ST.text, (QS.statement_start_offset/2) + 1,
          ((CASE statement_end_offset
              WHEN -1 THEN DATALENGTH(ST.text)
              ELSE QS.statement_end_offset END
                  - QS.statement_start_offset)/2) + 1) AS statement_text
           FROM sys.dm_exec_query_stats AS QS
           CROSS APPLY sys.dm_exec_sql_text(QS.sql_handle) as ST) as query_stats
      GROUP BY query_stats.query_hash
      ORDER BY 2 DESC;




<!--HONumber=Jan17_HO1-->


