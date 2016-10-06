<properties
   pageTitle="Připojení k Azure SQL Data Warehouse | Microsoft Azure"
   description="Postup vyhledání názvu serveru a připojovacího řetězce pro Azure SQL Data Warehouse"
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
   ms.date="09/26/2016"
   ms.author="sonyama;barbkess"/>


# Připojení k Azure SQL Data Warehouse

Tento článek vám pomůže s prvním připojením k SQL Data Warehouse.

## Vyhledání názvu serveru

Prvním krokem při připojení k SQL Data Warehouse je vědět, jak najít název serveru.  Například název serveru v následujícím příkladu je sample.database.windows.net. Plně kvalifikovaný název serveru zjistíte následujícím způsobem:

1. Přejděte na [portál Azure][].
2. Klikněte na **Databáze SQL**. 
3. Klikněte na databázi, ke které se chcete připojit.
4. Vyhledejte úplný název serveru.

    ![Úplný název serveru][1]

## Podporované ovladače a připojovací řetězce

Azure SQL Data Warehouse podporuje [ADO.NET][], [ODBC][], [PHP][] a [JDBC][]. Kliknutím na jeden z předchozích ovladačů vyhledáte nejnovější verzi a dokumentaci. Chcete-li automaticky vygenerovat připojovací řetězec pro ovladač, který používáte, z webu Azure Portal, můžete kliknout na **Zobrazit připojovací řetězce databáze** z předchozího příkladu.  Následuje několik příkladů toho, jak připojovací řetězce vypadají pro jednotlivé ovladače.

> [AZURE.NOTE] Zvažte nastavení časového limitu připojení na 300 sekund, pokud chcete, aby vaše připojení přestálo krátká období nedostupnosti.

### Příklad připojovacího řetězce pro ADO.NET

```C#
Server=tcp:{your_server}.database.windows.net,1433;Database={your_database};User ID={your_user_name};Password={your_password_here};Encrypt=True;TrustServerCertificate=False;Connection Timeout=30;
```

### Příklad připojovacího řetězce pro ODBC

```C#
Driver={SQL Server Native Client 11.0};Server=tcp:{your_server}.database.windows.net,1433;Database={your_database};Uid={your_user_name};Pwd={your_password_here};Encrypt=yes;TrustServerCertificate=no;Connection Timeout=30;
```

### Příklad připojovacího řetězce pro PHP

```PHP
Server: {your_server}.database.windows.net,1433 \r\nSQL Database: {your_database}\r\nUser Name: {your_user_name}\r\n\r\nPHP Data Objects(PDO) Sample Code:\r\n\r\ntry {\r\n   $conn = new PDO ( \"sqlsrv:server = tcp:{your_server}.database.windows.net,1433; Database = {your_database}\", \"{your_user_name}\", \"{your_password_here}\");\r\n    $conn->setAttribute( PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION );\r\n}\r\ncatch ( PDOException $e ) {\r\n   print( \"Error connecting to SQL Server.\" );\r\n   die(print_r($e));\r\n}\r\n\rSQL Server Extension Sample Code:\r\n\r\n$connectionInfo = array(\"UID\" => \"{your_user_name}\", \"pwd\" => \"{your_password_here}\", \"Database\" => \"{your_database}\", \"LoginTimeout\" => 30, \"Encrypt\" => 1, \"TrustServerCertificate\" => 0);\r\n$serverName = \"tcp:{your_server}.database.windows.net,1433\";\r\n$conn = sqlsrv_connect($serverName, $connectionInfo);
```

### Příklad připojovacího řetězce pro JDBC

```Java
jdbc:sqlserver://yourserver.database.windows.net:1433;database=yourdatabase;user={your_user_name};password={your_password_here};encrypt=true;trustServerCertificate=false;hostNameInCertificate=*.database.windows.net;loginTimeout=30;
```

## Nastavení připojení

SQL Data Warehouse během připojování a vytváření objektů používá několik standardních nastavení. Tato nastavení nelze přepsat a zahrnují:

| Nastavení databáze       | Hodnota                        |
| :--------------------- | :--------------------------- |
| [ANSI_NULLS][]         | ON                           |
| [QUOTED_IDENTIFIERS][] | ON                           |
| [DATEFORMAT][]         | mdy                          |
| [DATEFIRST][]          | 7                            |

## Další kroky

Informace o připojení a dotazování pomocí sady Visual Studio najdete v oddílu [Dotazování pomocí sady Visual Studio][]. Další informace o možnostech ověřování najdete v oddílu [Ověřování do Azure SQL Data Warehouse][].

<!--Articles-->
[Dotazování pomocí sady Visual Studio]: ./sql-data-warehouse-query-visual-studio.md
[Ověřování do Azure SQL Data Warehouse]: ./sql-data-warehouse-authentication.md

<!--MSDN references-->
[ADO.NET]: https://msdn.microsoft.com/library/e80y5yhx(v=vs.110).aspx
[ODBC]: https://msdn.microsoft.com/library/jj730314.aspx
[PHP]: https://msdn.microsoft.com/library/cc296172.aspx?f=255&MSPPError=-2147217396
[JDBC]: https://msdn.microsoft.com/library/mt484311(v=sql.110).aspx
[ANSI_NULLS]: https://msdn.microsoft.com/library/ms188048.aspx
[QUOTED_IDENTIFIERS]: https://msdn.microsoft.com/library/ms174393.aspx
[DATEFORMAT]: https://msdn.microsoft.com/library/ms189491.aspx
[DATEFIRST]: https://msdn.microsoft.com/library/ms181598.aspx

<!--Other-->
[portál Azure]: https://portal.azure.com

<!--Image references-->
[1]: media/sql-data-warehouse-connect-overview/get-server-name.png





<!--HONumber=Sep16_HO4-->


