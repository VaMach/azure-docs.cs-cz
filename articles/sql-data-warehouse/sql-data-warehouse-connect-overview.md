<properties
   pageTitle="Připojení k Azure SQL Data Warehouse | Microsoft Azure"
   description="Přehled připojení pro připojení k Azure SQL Data Warehouse"
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="sonyam"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="06/20/2016"
   ms.author="sonyama;barbkess"/>

# Připojení k Azure SQL Data Warehouse

> [AZURE.SELECTOR]
- [Přehled](sql-data-warehouse-connect-overview.md)
- [Authentication](sql-data-warehouse-authentication.md)
- [Ovladače](sql-data-warehouse-connection-strings.md)

Přehled připojení k Azure SQL Data Warehouse. 

## Zjištění připojovacího řetězce z portálu

Vaše služba SQL Data Warehouse je přidružená k Azure SQL Serveru. K připojení potřebujete plně kvalifikovaný název serveru (**název_serveru**. database.windows.net*).

Plně kvalifikovaný název serveru zjistíte následujícím způsobem:

1. Přejděte na [portál Azure][].
2. Klikněte na **Databáze SQL** a klikněte na databázi, ke které se chcete připojit. V tomto příkladě se používá ukázková databáze AdventureWorksDW.
3. Vyhledejte úplný název serveru.

    ![Úplný název serveru][1]

## Nastavení připojení
SQL Data Warehouse během připojování a vytváření objektů používá několik standardních nastavení. Ta se nedají přepsat.

| Nastavení databáze   | Hodnota                        |
| :----------------- | :--------------------------- |
| ANSI_NULLS         | ON                           |
| QUOTED_IDENTIFIERS | ON                           |
| NO_COUNT           | OFF                          |
| DATEFORMAT         | mdy                          |
| DATEFIRST          | 7                            |
| Databázová kolace | SQL_Latin1_General_CP1_CI_AS |

## Relace a požadavky
Po vytvoření připojení a nastavení relace můžete začít psát dotazy a odesílat je do služby SQL Data Warehouse.

Každý dotaz se bude reprezentovat pomocí jednoho nebo víc identifikátorů požadavku. Všechny dotazy odeslané v tomto připojení jsou součástí jedné relace, takže je bude reprezentovat jedno ID relace.

Protože je ale služba SQL Data Warehouse distribuovaný systém s architekturou MPP (Massively Parallel Processing), identifikátory relace a požadavku se zveřejňují trochu jinak než v SQL Serveru.

Relace a požadavky jsou logicky reprezentované příslušnými identifikátory.

| Identifikátor | Příklad hodnoty |
| :--------- | :------------ |
| ID relace | SID123456     |
| ID požadavku | QID123456     |

Všimněte si, že ID relace má předponu SID, což je zkratka pro Session ID (ID relace) a ID požadavku má předponu QID, což je zkratka pro Query ID (ID požadavku).

Tyto údaje vám pomůžou identifikovat dotaz při monitorování jeho výkonu. Výkon dotazu můžete monitorovat buď pomocí webu [Azure Portal], nebo pomocí zobrazení dynamické správy.

Tento dotaz identifikuje aktuální relaci.

```sql
SELECT SESSION_ID()
;
```

Pokud chcete zobrazit všechny dotazy, které jsou buď spuštěné, nebo se ve vašem datovém skladu nedávno spustily, můžete použít následující příklad. Ten vytvoří zobrazení a potom ho spustí.

```sql
CREATE VIEW dbo.vSessionRequests
AS
SELECT   s.[session_id]                                 AS Session_ID
        ,s.[status]                                     AS Session_Status
        ,s.[login_name]                                 AS Session_LoginName
        ,s.[login_time]                                 AS Session_LoginTime
        ,r.[request_id]                                 AS Request_ID
        ,r.[status]                                     AS Request_Status
        ,r.[submit_time]                                AS Request_SubmitTime
        ,r.[start_time]                                 AS Request_StartTime
        ,r.[end_compile_time]                           AS Request_EndCompileTime
        ,r.[end_time]                                   AS Request_EndTime
        ,r.[total_elapsed_time]                         AS Request_TotalElapsedDuration_ms
        ,DATEDIFF(ms,[submit_time],[start_time])        AS Request_InitiateDuration_ms
        ,DATEDIFF(ms,[start_time],[end_compile_time])   AS Request_CompileDuration_ms
        ,DATEDIFF(ms,[end_compile_time],[end_time])     AS Request_ExecDuration_ms
        ,[label]                                        AS Request_QueryLabel
        ,[command]                                      AS Request_Command
        ,[database_id]                                  AS Request_Database_ID
FROM    sys.dm_pdw_exec_requests r
JOIN    sys.dm_pdw_exec_sessions s  ON  r.[session_id] = s.[session_id]
WHERE   s.[session_id] <> SESSION_ID()
;

SELECT * FROM dbo.vSessionRequests;
```

## Další kroky

Pokud se chcete začít dotazovat na svůj datový sklad pomocí sady Visual Studio a jiných aplikací, najdete informace v článku [Dotazování pomocí sady Visual Studio][].


<!--Arcticles-->

[Dotazování pomocí sady Visual Studio]: ./sql-data-warehouse-query-visual-studio.md

<!--Other-->
[portál Azure]: https://portal.azure.com

<!--Image references-->

[1]: media/sql-data-warehouse-connect-overview/get-server-name.png





<!---HONumber=Aug16_HO4-->


