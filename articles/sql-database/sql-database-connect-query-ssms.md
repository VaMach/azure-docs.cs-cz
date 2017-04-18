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
ms.custom: quick start manage
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 03/15/2017
ms.author: carlrab
translationtype: Human Translation
ms.sourcegitcommit: 785d3a8920d48e11e80048665e9866f16c514cf7
ms.openlocfilehash: c173f1b6937739f662eb41aa1886e66cb06ed729
ms.lasthandoff: 04/12/2017


---
# <a name="azure-sql-database-use-sql-server-management-studio-to-connect-and-query-data"></a>Azure SQL Database: Připojení a dotazování dat pomocí aplikace SQL Server Management Studio

[SQL Server Management Studio](https://msdn.microsoft.com/library/ms174173.aspx) je nástroj správy používaný k vytváření a správě prostředků SQL Serveru z uživatelského rozhraní nebo ve skriptech. Tento rychlý start ukazuje použití SSMS k připojení k Azure SQL Database a následné použití příkazů jazyka Transact-SQL k dotazování, vkládání, aktualizaci a odstraňování dat v databázi. 

Tento rychlý start používá jako výchozí bod prostředky vytvořené v některém z těchto rychlých startů:

- [Vytvoření databáze – portál](sql-database-get-started-portal.md)
- [Vytvoření databáze – rozhraní příkazového řádku](sql-database-get-started-cli.md)

Než začnete, ujistěte se, že máte nainstalovanou nejnovější verzi aplikace [SSMS](https://msdn.microsoft.com/library/mt238290.aspx). 

## <a name="get-connection-information"></a>Získání informací o připojení

Na webu Azure Portal získejte plně kvalifikovaný název serveru služby Azure SQL Database. Plně kvalifikovaný název serveru použijete k připojení k serveru pomocí aplikace SQL Server Management Studio.

1. Přihlaste se k [portálu Azure](https://portal.azure.com/).
2. V nabídce vlevo vyberte **SQL Database** a na stránce **Databáze SQL** klikněte na vaši databázi. 
3. V podokně **Základy** na stránce webu Azure Portal pro vaši databázi vyhledejte a potom zkopírujte **Název serveru**.

   ![informace o připojení](./media/sql-database-connect-query-ssms/connection-information.png) 


## <a name="connect-to-the-server-and-your-new-database"></a>Připojení k serveru a nové databázi

Pomocí aplikace SQL Server Management Studio navažte připojení k serveru služby Azure SQL Database. 

> [!IMPORTANT]
> Logický server Azure SQL Database naslouchá na portu 1433. Pokud se pokoušíte připojovat k logickému serveru Azure SQL Database z oblasti za podnikovou bránou firewall, je třeba v podnikové brány firewall otevřít tento port, abyste se mohli úspěšně připojit.
>

1. Otevřete SQL Server Management Studio.

2. V dialogovém okně **Připojení k serveru** zadejte následující informace:
   - **Typ serveru:** Zadejte Databázový stroj.
   - **Název serveru:** Zadejte plně kvalifikovaný název serveru, například **mynewserver20170313.database.windows.net**.
   - **Ověřování:** Zadejte Ověřování SQL Serveru.
   - **Přihlášení:** Zadejte účet správce serveru.
   - **Heslo:** Zadejte heslo pro účet správce serveru.

   ![Připojení k serveru](./media/sql-database-connect-query-ssms/connect.png)  

3. Klikněte na tlačítko **Možnosti**. V části **Připojit k databázi** zadejte **mySampleDatabase**, abyste se připojili k této databázi, kterou jste dříve vytvořili.

   ![připojení k databázi na serveru](./media/sql-database-connect-query-ssms/options-connect-to-db.png)  

4. Klikněte na **Připojit**. V aplikaci SSMS se otevře okno Průzkumníka objektů. 

   ![Připojeno k serveru](./media/sql-database-connect-query-ssms/connected.png)  

4. V Průzkumníku objektů zobrazte objekty v ukázkové databázi rozbalením **Databáze** a potom **mySampleDatabase**.

## <a name="query-data"></a>Dotazování dat

K dotazování dat v databázi SQL Azure použijte příkaz jazyka Transact-SQL [SELECT](https://msdn.microsoft.com/library/ms189499.aspx).

1. V Průzkumníku objektů klikněte pravým tlačítkem na **mySampleDatabase** a potom klikněte na **Nový dotaz**. Otevře se prázdné okno dotazu připojené k vaší databázi.
2. Do okna dotazu zadejte následující dotaz:

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

1. V okně dotazu nahraďte předchozí dotaz následujícím dotazem:

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

2. Kliknutím na **Provést** na panelu nástrojů vložte nový řádek do tabulky Product.

    <img src="./media/sql-database-connect-query-ssms/insert.png" alt="insert" style="width: 780px;" />

## <a name="update-data"></a>Aktualizace dat

K aktualizaci dat v databázi SQL Azure použijte příkaz jazyka Transact-SQL [UPDATE](https://msdn.microsoft.com/library/ms177523.aspx).

1. V okně dotazu nahraďte předchozí dotaz následujícím dotazem:

   ```sql
   UPDATE [SalesLT].[Product]
   SET [ListPrice] = 125
   WHERE Name = 'myNewProduct';
   ```

2. Kliknutím na **Provést** na panelu nástrojů aktualizujte zadaný řádek v tabulce Product.

    <img src="./media/sql-database-connect-query-ssms/update.png" alt="update" style="width: 780px;" />

## <a name="delete-data"></a>Odstranění dat

K odstranění dat z databáze SQL Azure použijte příkaz jazyka Transact-SQL [DELETE](https://msdn.microsoft.com/library/ms189835.aspx).

1. V okně dotazu nahraďte předchozí dotaz následujícím dotazem:

   ```sql
   DELETE FROM [SalesLT].[Product]
   WHERE Name = 'myNewProduct';
   ```

2. Kliknutím na **Provést** na panelu nástrojů odstraňte zadaný řádek z tabulky Product.

    <img src="./media/sql-database-connect-query-ssms/delete.png" alt="delete" style="width: 780px;" />

## <a name="next-steps"></a>Další kroky

- Další informace o aplikaci SSMS najdete v tématu [Použití aplikace SQL Server Management Studio](https://msdn.microsoft.com/library/ms174173.aspx).
- Informace o připojení a dotazování pomocí Visual Studio Code najdete v tématu [Připojení a dotazování pomocí Visual Studio Code](sql-database-connect-query-vscode.md).
- Informace o připojení a dotazování pomocí .NET najdete v tématu [Připojení a dotazování pomocí .NET](sql-database-connect-query-dotnet.md).
- Informace o připojení a dotazování pomocí PHP najdete v tématu [Připojení a dotazování pomocí PHP](sql-database-connect-query-php.md).
- Informace o připojení a dotazování pomocí Node.js najdete v tématu [Připojení a dotazování pomocí Node.js](sql-database-connect-query-nodejs.md).
- Informace o připojení a dotazování pomocí Javy najdete v tématu [Připojení a dotazování pomocí Javy](sql-database-connect-query-java.md).
- Informace o připojení a dotazování pomocí Pythonu najdete v tématu [Připojení a dotazování pomocí Pythonu](sql-database-connect-query-python.md).
- Informace o připojení a dotazování pomocí Ruby najdete v tématu [Připojení a dotazování pomocí Ruby](sql-database-connect-query-ruby.md).

