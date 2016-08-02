<properties
   pageTitle="Začínáme: Připojení k Azure SQL Data Warehouse | Microsoft Azure"
   description="Začněte tím, že se připojíte k SQL Data Warehouse a spustíte pár dotazů."
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
   ms.date="05/16/2016"
   ms.author="mausher;barbkess;sonyama"/>

# Připojení a dotazování pomocí SQLCMD

> [AZURE.SELECTOR]
- [Visual Studio](sql-data-warehouse-get-started-connect.md)
- [SQLCMD](sql-data-warehouse-get-started-connect-sqlcmd.md)
- [AAD](sql-data-warehouse-get-started-connect-aad-authentication.md)


Tento návod ukazuje, jak se během pár minut připojit a zadávat dotazy na databázi Azure SQL Data Warehouse pomocí nástroje sqlcmd.exe. Tento návod vám ukáže, jak:

+ Nainstalovat požadovaný software
+ Připojit se k databázi, která obsahuje ukázkovou databázi AdventureWorksDW
+ Zadávat dotazy na data v ukázkové databázi  

## Požadavky

+ Pokud si budete chtít stáhnout [sqlcmd.exe][], podívejte se na stránku pro stažení nástrojů [Microsoft Command Line Utilities 11 for SQL Server][].

## Získání plně kvalifikovaného názvu SQL serveru Azure

K připojení ke své databázi potřebujete úplný název serveru (***název_serveru**. database.windows.net*) obsahujícího databázi, ke které se chcete připojit.

1. Přejděte na [portál Azure][].
2. Přejděte do databáze, ke které se chcete připojit.
3. Vyhledejte úplný název serveru (použijeme ho v následujících krocích):

![][1]


## Připojení k SQL Data Warehouse pomocí sqlcmd

Když se budete chtít ke konkrétní instanci SQL Data Warehouse připojení pomocí nástroje příkazového řádku sqlcmd, budete muset otevřít příkazový řádek a zadat příkaz **sqlcmd** a za ním připojovací řetězec pro vaši databázi SQL Data Warehouse. Připojovací řetězec bude muset mít následující parametry:

+ **Server (-S):** Server v následující podobě: `<`název serveru`>`.database.windows.net
+ **Database (-d):** Název databáze
+ **User (-U):** Uživatel serveru v následující podobě: `<`Uživatel`>`
+ **Password (-P):** Heslo přidružené k uživateli
+ **Enable Quoted Identifiers (-I):** Aby bylo možné se připojit k instanci SQL Data Warehouse, musí být povolené identifikátory v uvozovkách.

Proto platí, že když se budete chtít připojit k instanci SQL Data Warehouse, zadali byste následující příkaz:

```sql
C:\>sqlcmd -S <Server Name>.database.windows.net -d <Database> -U <User> -P <Password> -I
```

## Spuštění ukázkových dotazů

Po připojení můžete pro instanci zadávat všechny podporované příkazy jazyka Transact-SQL.

```sql
C:\>sqlcmd -S <Server Name>.database.windows.net -d <Database> -U <User> -P <Password> -I
1> SELECT name FROM sys.tables;
2> GO
3> QUIT
```

Další informace o sqlcmd najdete v [dokumentaci k sqlcmd][sqlcmd.exe].


## Další kroky

Teď, když se můžete připojit a můžete zadávat dotazy, zkuste se [připojit pomocí PowerBI][].

Informace o tom, jak si pro prostředí nakonfigurovat ověřování systému Windows, najdete v tématu [Připojení k SQL Database nebo SQL Data Warehouse pomocí ověřování služby Azure Active Directory][].

<!--Articles-->
[Připojení k SQL Database nebo SQL Data Warehouse pomocí ověřování služby Azure Active Directory]: ../sql-data-warehouse/sql-data-warehouse-get-started-connect-aad-authentication.md
[připojit pomocí PowerBI]: ./sql-data-warehouse-integrate-power-bi.md
[Visual Studio]: ./sql-data-warehouse-get-started-connect.md
[SQLCMD]: ./sql-data-warehouse-get-started-connect-sqlcmd.md

<!--Other-->
[sqlcmd.exe]: https://msdn.microsoft.com/en-us/library/ms162773.aspx
[Microsoft Command Line Utilities 11 for SQL Server]: http://go.microsoft.com/fwlink/?LinkId=321501
[portál Azure]: https://portal.azure.com

<!--Image references-->
[1]: ./media/sql-data-warehouse-get-started-connect/get-server-name.png



<!--HONumber=Jun16_HO2-->


