---
title: "SSMS: Připojení a dotazování dat ve službě Azure SQL Database | Dokumentace Microsoftu"
description: "Zjistěte, jak se připojit k SQL Database na Azure pomocí služby SQL Server Management Studio (SSMS). Potom spustíte příkazy jazyka Transact-SQL (T-SQL) k dotazování a úpravě dat."
metacanonical: 
keywords: "Připojení k SQL Database, SQL Server Management Studio"
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: 7cd2a114-c13c-4ace-9088-97bd9d68de12
ms.service: sql-database
ms.custom: development
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 03/15/2017
ms.author: carlrab
translationtype: Human Translation
ms.sourcegitcommit: 303cb9950f46916fbdd58762acd1608c925c1328
ms.openlocfilehash: 7ae47bcce700336206d532b414b7d0eea41d87c5
ms.lasthandoff: 04/04/2017


---
# <a name="azure-sql-database-use-sql-server-management-studio-to-connect-and-query-data"></a>Azure SQL Database: Připojení a dotazování dat pomocí aplikace SQL Server Management Studio

Pomocí aplikace [SQL Server Management Studio](https://msdn.microsoft.com/library/ms174173.aspx) můžete vytvářet a spravovat prostředky SQL Serveru z uživatelského rozhraní nebo ve skriptech. Tento rychlý start podrobně popisuje použití aplikace SSMS pro připojení k databázi SQL Azure a následné provádění příkazů k dotazování, vložení, aktualizaci a odstranění.

Tento rychlý start používá jako výchozí bod prostředky vytvořené v některém z těchto rychlých startů:

- [Vytvoření databáze – portál](sql-database-get-started-portal.md)
- [Vytvoření databáze – rozhraní příkazového řádku](sql-database-get-started-cli.md)

Než začnete, ujistěte se, že máte nainstalovanou nejnovější verzi aplikace [SSMS](https://msdn.microsoft.com/library/mt238290.aspx). 

## <a name="get-connection-information"></a>Získání informací o připojení

Na webu Azure Portal získejte plně kvalifikovaný název serveru služby Azure SQL Database. Plně kvalifikovaný název serveru použijete k připojení k serveru pomocí aplikace SQL Server Management Studio.

1. Přihlaste se k [portálu Azure](https://portal.azure.com/).
2. V nabídce vlevo vyberte **SQL Database** a na stránce **Databáze SQL** klikněte na vaši databázi. 
3. V podokně **Základy** na stránce webu Azure Portal pro vaši databázi vyhledejte a potom zkopírujte **Název serveru**.

    <img src="./media/sql-database-connect-query-ssms/connection-information.png" alt="connection information" style="width: 780px;" />

## <a name="connect-to-the-server-and-your-new-database"></a>Připojení k serveru a nové databázi

Pomocí aplikace SQL Server Management Studio navažte připojení k serveru služby Azure SQL Database.

1. Otevřete SQL Server Management Studio.

2. V dialogovém okně **Připojení k serveru** zadejte následující informace:
   - **Typ serveru:** Zadejte Databázový stroj.
   - **Název serveru:** Zadejte plně kvalifikovaný název serveru, například **mynewserver20170313.database.windows.net**.
   - **Ověřování:** Zadejte Ověřování SQL Serveru.
   - **Přihlášení:** Zadejte účet správce serveru.
   - **Heslo:** Zadejte heslo pro účet správce serveru.
 
    <img src="./media/sql-database-connect-query-ssms/connect.png" alt="connect to server" style="width: 780px;" />

3. Klikněte na **Připojit**. V aplikaci SSMS se otevře okno Průzkumníka objektů. 

    <img src="./media/sql-database-connect-query-ssms/connected.png" alt="connected to server" style="width: 780px;" />

4. V Průzkumníku objektů zobrazte objekty v ukázkové databázi rozbalením **Databáze** a potom **mySampleDatabase**.

## <a name="query-data"></a>Dotazování dat

K dotazování dat v databázi SQL Azure použijte příkaz jazyka Transact-SQL [SELECT](https://msdn.microsoft.com/library/ms189499.aspx).

1. V Průzkumníku objektů klikněte pravým tlačítkem na **mySampleDatabase** a potom klikněte na **Nový dotaz**. Otevře se prázdné okno dotazu připojené k vaší databázi.
2. V okně dotazu zadejte následující dotaz:

   ```sql
   SELECT pc.Name as CategoryName, p.name as ProductName
   FROM [SalesLT].[ProductCategory] pc
   JOIN [SalesLT].[Product] p
   ON pc.productcategoryid = p.productcategoryid;
   ```

3. Kliknutím na **Provést** na panelu nástrojů načtěte data z tabulek Product a ProductCategory.

    <img src="./media/sql-database-connect-query-ssms/query.png" alt="query" style="width: 780px;" />

## <a name="insert-data"></a>Vložení dat

K vložení dat do databáze SQL Azure použijte příkaz jazyka Transact-SQL [INSERT](https://msdn.microsoft.com/library/ms174335.aspx).

1. Na panelu nástrojů klikněte na **Nový dotaz**. Otevře se prázdné okno dotazu připojené k vaší databázi.
2. V okně dotazu zadejte následující dotaz:

   ```sql
   INSERT INTO [SalesLT].[Product]
           ( [Name]
           , [ProductNumber]
           , [Color]
           , [ProductCategoryID]
           , [StandardCost]
           , [ListPrice]
           , [SellStartDate]
           )
     VALUES
           ('myNewProduct'
           ,123456789
           ,'NewColor'
           ,1
           ,100
           ,100
           ,GETDATE() );
   ```

3. Kliknutím na **Provést** na panelu nástrojů vložte nový řádek do tabulky Product.

    <img src="./media/sql-database-connect-query-ssms/insert.png" alt="insert" style="width: 780px;" />

## <a name="update-data"></a>Aktualizace dat

K aktualizaci dat v databázi SQL Azure použijte příkaz jazyka Transact-SQL [UPDATE](https://msdn.microsoft.com/library/ms177523.aspx).

1. Na panelu nástrojů klikněte na **Nový dotaz**. Otevře se prázdné okno dotazu připojené k vaší databázi.
2. V okně dotazu zadejte následující dotaz:

   ```sql
   UPDATE [SalesLT].[Product]
   SET [ListPrice] = 125
   WHERE Name = 'myNewProduct';
   ```

3. Kliknutím na **Provést** na panelu nástrojů aktualizujte zadaný řádek v tabulce Product.

    <img src="./media/sql-database-connect-query-ssms/update.png" alt="update" style="width: 780px;" />

## <a name="delete-data"></a>Odstranění dat

K odstranění dat z databáze SQL Azure použijte příkaz jazyka Transact-SQL [DELETE](https://msdn.microsoft.com/library/ms189835.aspx).

1. Na panelu nástrojů klikněte na **Nový dotaz**. Otevře se prázdné okno dotazu připojené k vaší databázi.
2. V okně dotazu zadejte následující dotaz:

   ```sql
   DELETE FROM [SalesLT].[Product]
   WHERE Name = 'myNewProduct';
   ```

3. Kliknutím na **Provést** na panelu nástrojů odstraňte zadaný řádek z tabulky Product.

    <img src="./media/sql-database-connect-query-ssms/delete.png" alt="delete" style="width: 780px;" />

## <a name="next-steps"></a>Další kroky

- Další informace o aplikaci SSMS najdete v tématu [Použití aplikace SQL Server Management Studio](https://msdn.microsoft.com/library/ms174173.aspx).
- Informace o dotazování a upravování dat pomocí nástroje Visual Studio Code najdete na stránce [Visual Studio Code](https://code.visualstudio.com/docs).

