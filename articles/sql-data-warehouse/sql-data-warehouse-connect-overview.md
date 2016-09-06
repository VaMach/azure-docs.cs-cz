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
   ms.date="08/27/2016"
   ms.author="sonyama;barbkess"/>

# Připojení k Azure SQL Data Warehouse

> [AZURE.SELECTOR]
- [Přehled](sql-data-warehouse-connect-overview.md)
- [Authentication](sql-data-warehouse-authentication.md)
- [Ovladače](sql-data-warehouse-connection-strings.md)

Přehled připojení k Azure SQL Data Warehouse. 

## Zjištění připojovacího řetězce z portálu

Vaše služba SQL Data Warehouse je přidružená k Azure SQL Serveru. K připojení potřebujete plně kvalifikovaný název serveru.  Například **můj_server**.database.windows.net.

Plně kvalifikovaný název serveru zjistíte následujícím způsobem:

1. Přejděte na [portál Azure][].
2. Klikněte na **Databáze SQL** a klikněte na databázi, ke které se chcete připojit. V tomto příkladě se používá ukázková databáze AdventureWorksDW.
3. Vyhledejte úplný název serveru.

    ![Úplný název serveru][1]

## Nastavení připojení

SQL Data Warehouse během připojování a vytváření objektů používá několik standardních nastavení. Ta se nedají přepsat.

| Nastavení databáze       | Hodnota                        |
| :--------------------- | :--------------------------- |
| [ANSI_NULLS][]         | ON                           |
| [QUOTED_IDENTIFIERS][] | ON                           |
| [DATEFORMAT][]         | mdy                          |
| [DATEFIRST][]          | 7                            |

## Monitorování připojení a dotazů

Po vytvoření připojení a nastavení relace můžete začít psát dotazy a odesílat je do služby SQL Data Warehouse.  Další informace o monitorování relací a dotazů najdete v tématu [Monitorování vaší úlohy pomocí DMV][].

## Další kroky

Pokud se chcete začít dotazovat na svůj datový sklad pomocí sady Visual Studio a jiných aplikací, najdete informace v článku [Dotazování pomocí sady Visual Studio][]. 

<!--Articles-->
[Dotazování pomocí sady Visual Studio]: ./sql-data-warehouse-query-visual-studio.md
[Monitorování vaší úlohy pomocí DMV]: ./sql-data-warehouse-manage-monitor.md

<!--MSDN references-->
[ANSI_NULLS]: https://msdn.microsoft.com/library/ms188048.aspx
[QUOTED_IDENTIFIERS]: https://msdn.microsoft.com/library/ms174393.aspx
[DATEFORMAT]: https://msdn.microsoft.com/library/ms189491.aspx
[DATEFIRST]: https://msdn.microsoft.com/library/ms181598.aspx

<!--Other-->
[portál Azure]: https://portal.azure.com

<!--Image references-->
[1]: media/sql-data-warehouse-connect-overview/get-server-name.png





<!--HONumber=ago16_HO5-->


