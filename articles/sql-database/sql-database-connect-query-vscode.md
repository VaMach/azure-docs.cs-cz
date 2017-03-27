---
title: "VS Code: Připojení a dotazování dat ve službě Azure SQL Database | Dokumentace Microsoftu"
description: "Zjistěte, jak se připojit ke službě SQL Database pomocí nástroje Visual Studio Code. Potom spustíte příkazy jazyka Transact-SQL (T-SQL) k dotazování a úpravě dat."
metacanonical: 
keywords: "Připojení k službě SQL Database"
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: 676bd799-a571-4bb8-848b-fb1720007866
ms.service: sql-database
ms.custom: manage
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 03/17/2017
ms.author: carlrab
translationtype: Human Translation
ms.sourcegitcommit: 0d8472cb3b0d891d2b184621d62830d1ccd5e2e7
ms.openlocfilehash: 9192ed7b69e52c59efe33981e1e557b634679196
ms.lasthandoff: 03/21/2017


---
# <a name="azure-sql-database-use-visual-studio-code-to-connect-and-query-data"></a>Azure SQL Database: Použití nástroje Visual Studio Code k připojení a dotazování dat

[Visual Studio Code](https://code.visualstudio.com/docs) je grafický editor kódu pro systémy Linux, macOS a Windows s podporou rozšíření. Pomocí nástroje Visual Studio Code s [rozšířením mssql](https://aka.ms/mssql-marketplace) se můžete připojit k databázi SQL Azure a dotazovat data. Tento průvodce podrobně popisuje použití nástroje Visual Studio Code pro připojení k databázi SQL Azure a následné provádění příkazů k dotazování, vložení, aktualizaci a odstranění.

Tento rychlý start používá jako výchozí bod prostředky vytvořené v některém z těchto rychlých startů:

- [Vytvoření databáze – portál](sql-database-get-started-portal.md)
- [Vytvoření databáze – rozhraní příkazového řádku](sql-database-get-started-cli.md)
- [Vytvoření databáze – PowerShell](sql-database-get-started-powershell.md) 

Než začnete, ujistěte se, že máte nainstalovanou nejnovější verzi nástroje [Visual Studio Code](https://code.visualstudio.com/Download) a načtené [rozšíření mssql](https://aka.ms/mssql-marketplace). Pokyny k instalaci rozšíření mssql najdete v tématu popisujícím [instalaci nástroje VS Code](https://docs.microsoft.com/sql/linux/sql-server-linux-develop-use-vscode#install-vs-code). 

## <a name="get-connection-information"></a>Získání informací o připojení

Na webu Azure Portal získejte plně kvalifikovaný název serveru služby Azure SQL Database. Plně kvalifikovaný název serveru použijete k připojení k serveru pomocí nástroje Visual Studio Code.

1. Přihlaste se k [portálu Azure](https://portal.azure.com/).
2. V nabídce vlevo vyberte **SQL Database** a na stránce **Databáze SQL** klikněte na vaši databázi. 
3. V podokně **Základy** na stránce webu Azure Portal pro vaši databázi vyhledejte a potom zkopírujte **Název serveru** pro pozdější použití v tomto rychlém startu.

    <img src="./media/sql-database-connect-query-ssms/connection-information.png" alt="connection information" style="width: 780px;" />

## <a name="set-language-mode-to-sql"></a>Nastavení jazykového režimu na SQL

Nastavte v nástroji Visual Studio Code jazykový režim na **SQL**, abyste povolili příkazy mssql a technologii IntelliSense jazyka T-SQL.

1. Otevřete nové okno nástroje Visual Studio Code. 

2. Stiskněte kombinaci kláves **CTRL + K, M**, zadejte **SQL** a stisknutím klávesy **ENTER** nastavte jazykový režim na SQL. 

<img src="./media/sql-database-connect-query-vscode/vscode-language-mode.png" alt="SQL language mode" style="width: 780px;" />

## <a name="connect-to-the-server"></a>Připojení k serveru

Pomocí nástroje Visual Studio Code navažte připojení k serveru služby Azure SQL Database.

1. V nástroji VS Code stisknutím kombinace kláves **CTRL + SHIFT + P** (nebo **F1**) otevřete paletu příkazů.

2. Zadejte **sqlcon** a stiskněte klávesu **ENTER**.

3. Kliknutím na **Ano** nastavte jazyk na **SQL**.

4. Stisknutím klávesy **ENTER** vyberte **Vytvořit profil připojení**. Tím se vytvoří profil připojení pro vaši instanci SQL Serveru.

5. Postupujte podle výzev a zadejte vlastnosti připojení pro nový profil připojení. Po zadání všech hodnot pokračujte stisknutím klávesy **ENTER**. 

   Následující tabulka popisuje vlastnosti profilu připojení.

   | Nastavení | Popis |
   |-----|-----|
   | **Název serveru** | Zadejte plně kvalifikovaný název serveru, například **mynewserver20170313.database.windows.net**. |
   | **Název databáze** | Zadejte název databáze, například **mySampleDatabase**. |
   | **Ověřování** | Vyberte Přihlášení SQL. |
   | **Uživatelské jméno** | Zadejte účet správce serveru. |
   | **Heslo (Přihlášení SQL)** | Zadejte heslo pro účet správce serveru. | 
   | **Uložit heslo?** | Vyberte **Ano** nebo **Ne**. |
   | **[Volitelné] Zadejte název pro tento profil.** | Zadejte název profilu připojení, například **mySampleDatabase**. 

6. Stisknutím klávesy **ESC** zavřete zprávu informující o vytvoření a připojení profilu.

7. Na stavovém řádku ověřte připojení.

   <img src="./media/sql-database-connect-query-vscode/vscode-connection-status.png" alt="Connection status" style="width: 780px;" />

## <a name="query-data"></a>Dotazování dat

K dotazování dat v databázi SQL Azure použijte příkaz jazyka Transact-SQL [SELECT](https://msdn.microsoft.com/library/ms189499.aspx).

1. V okně **Editor** zadejte do prázdného okna dotazu následující dotaz:

   ```sql
   SELECT pc.Name as CategoryName, p.name as ProductName
   FROM [SalesLT].[ProductCategory] pc
   JOIN [SalesLT].[Product] p
   ON pc.productcategoryid = p.productcategoryid;
   ```

3. Stisknutím kombinace kláves **CTRL + SHIFT + E** načtěte data z tabulek Product a ProductCategory.

    <img src="./media/sql-database-connect-query-vscode/query.png" alt="Query" style="width: 780px;" />

## <a name="insert-data"></a>Vložení dat

K vložení dat do databáze SQL Azure použijte příkaz jazyka Transact-SQL [INSERT](https://msdn.microsoft.com/library/ms174335.aspx).

1. V okně **Editor** smažte předchozí dotaz a zadejte následující dotaz:

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

3. Stisknutím kombinace kláves **CTRL + SHIFT + E** vložte nový řádek do tabulky Product.

## <a name="update-data"></a>Aktualizace dat

K aktualizaci dat v databázi SQL Azure použijte příkaz jazyka Transact-SQL [UPDATE](https://msdn.microsoft.com/library/ms177523.aspx).

1.  V okně **Editor** smažte předchozí dotaz a zadejte následující dotaz:

   ```sql
   UPDATE [SalesLT].[Product]
   SET [ListPrice] = 125
   WHERE Name = 'myNewProduct';
   ```

3. Stisknutím kombinace kláves **CTRL + SHIFT + E** aktualizujte zadaný řádek v tabulce Product.

## <a name="delete-data"></a>Odstranění dat

K odstranění dat z databáze SQL Azure použijte příkaz jazyka Transact-SQL [DELETE](https://msdn.microsoft.com/library/ms189835.aspx).

1. V okně **Editor** smažte předchozí dotaz a zadejte následující dotaz:

   ```sql
   DELETE FROM [SalesLT].[Product]
   WHERE Name = 'myNewProduct';
   ```

3. Stisknutím kombinace kláves **CTRL + SHIFT + E** odstraňte zadaný řádek v tabulce Product.

## <a name="next-steps"></a>Další kroky

- Informace o nástroji Visual Studio Code najdete na stránce [Visual Studio Code](https://code.visualstudio.com/docs).
- Informace o dotazování a upravování dat pomocí aplikace SQL Server Management Studio najdete v tématu týkajícím se aplikace [SSMS](https://msdn.microsoft.com/library/ms174173.aspx).
