---
title: "Připojení ke službě Azure SQL Database pomocí technologie .NET (jazyk C#) | Dokumentace Microsoftu"
description: "Pomocí ukázkového kódu v tomto rychlém startu můžete vytvořit moderní aplikaci v jazyce C#, postavenou na výkonné relační databázi v cloudu, kterou zajišťuje služba Azure SQL Database."
services: sql-database
documentationcenter: 
author: stevestein
manager: jhubbard
editor: 
ms.assetid: 7faca033-24b4-4f64-9301-b4de41e73dfd
ms.service: sql-database
ms.custom: development
ms.workload: drivers
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 03/16/2017
ms.author: sstein
translationtype: Human Translation
ms.sourcegitcommit: 424d8654a047a28ef6e32b73952cf98d28547f4f
ms.openlocfilehash: c54ccef3098502c9fbaad13c5fe35ed15bf93f29
ms.lasthandoff: 03/22/2017


---
# <a name="azure-sql-database-use-net-c-to-connect-and-query-data"></a>Azure SQL Database: Použití technologie .NET (jazyk C#) k připojení a dotazování dat

Pomocí [jazyka C# a technologie ADO.NET](https://msdn.microsoft.com/library/kb9s9ks0.aspx) se můžete připojit k databázi SQL Azure a provádět dotazování. Tento průvodce podrobně popisuje použití jazyka C# pro připojení k databázi SQL Azure a následné provádění příkazů k dotazování, vložení, aktualizaci a odstranění.

Tento rychlý start používá jako výchozí bod prostředky vytvořené v některém z těchto rychlých startů:

- [Vytvoření databáze – portál](sql-database-get-started-portal.md)
- [Vytvoření databáze – rozhraní příkazového řádku](sql-database-get-started-cli.md)
- [Vytvoření databáze – PowerShell](sql-database-get-started-powershell.md) 

Než začnete, ujistěte se, že jste nakonfigurovali vývojové prostředí pro jazyk C#. Přečtěte si téma [Instalace bezplatné verze Visual Studio Community](https://www.visualstudio.com/) nebo si nainstalujte [ovladač ADO.NET pro SQL Server](https://www.microsoft.com/net/download).

## <a name="connect-to-database-and-query-data"></a>Připojení k databázi a dotazování dat

Na webu Azure Portal získejte připojovací řetězec. Připojovací řetězec použijete k připojení k databázi SQL Azure.

1. Přihlaste se k [portálu Azure](https://portal.azure.com/).
2. V nabídce vlevo vyberte **SQL Database** a na stránce **Databáze SQL** klikněte na vaši databázi. 
3. V podokně **Základy** pro vaši databázi vyhledejte a klikněte na **Zobrazit databázové připojovací řetězce**.
4. Zkopírujte připojovací řetězec pro **ADO.NET**.

    <img src="./media/sql-database-connect-query-dotnet/connection-strings.png" alt="connection strings" style="width: 780px;" />

5. Otevřete Visual Studio a vytvořte konzolovou aplikaci.
6. Přidejte ```using System.Data.SqlClient``` do souboru kódu ([obor názvů System.Data.SqlClient](https://msdn.microsoft.com/library/system.data.sqlclient.aspx)). 

7. K dotazování dat v databázi SQL Azure použijte metodu [SqlCommand.ExecuteReader](https://msdn.microsoft.com/library/system.data.sqlclient.sqlcommand.executereader.aspx) s příkazem jazyka Transact-SQL [SELECT](https://msdn.microsoft.com/library/ms189499.aspx).

    ```csharp
    string strConn = "<connection string>";
    using (var connection = new SqlConnection(strConn))
    {
   connection.Open();

    SqlCommand selectCommand = new SqlCommand("", connection);
    selectCommand.CommandType = CommandType.Text;

    selectCommand.CommandText = @"SELECT TOP 20 pc.Name as CategoryName, p.name as ProductName
        FROM [SalesLT].[ProductCategory] pc
        JOIN [SalesLT].[Product] p
        ON pc.productcategoryid = p.productcategoryid";

    SqlDataReader reader = selectCommand.ExecuteReader();

    while (reader.Read())
    {
        // show data
        Console.WriteLine($"{reader.GetString(0)}\t{reader.GetString(1)}");
    }
    reader.Close();
    }
    ```

## <a name="insert-data"></a>Vložení dat

K vložení dat do databáze SQL Azure použijte metodu [SqlCommand.ExecuteNonQuery](https://msdn.microsoft.com/library/system.data.sqlclient.sqlcommand.executenonquery.aspx) s příkazem jazyka Transact-SQL [INSERT](https://msdn.microsoft.com/library/ms174335.aspx).

```csharp
SqlCommand insertCommand = new SqlCommand("", connection);
insertCommand.CommandType = CommandType.Text;
insertCommand.CommandText = @"INSERT INTO[SalesLT].[Product]
            ( [Name]
            , [ProductNumber]
            , [Color]
            , [StandardCost]
            , [ListPrice]
            , [SellStartDate]
            )
VALUES
(
            @Name,
            @ProductNumber,
            @Color,
            @StandardCost,
            @ListPrice,
            @SellStartDate)";

            insertCommand.Parameters.AddWithValue("@Name", "BrandNewProduct");
            insertCommand.Parameters.AddWithValue("@ProductNumber", "200989");
            insertCommand.Parameters.AddWithValue("@Color", "Blue");
            insertCommand.Parameters.AddWithValue("@StandardCost", 75);
            insertCommand.Parameters.AddWithValue("@ListPrice", 80);
            insertCommand.Parameters.AddWithValue("@SellStartDate", "7/1/2016");

int newrows = insertCommand.ExecuteNonQuery();
Console.WriteLine($"Inserted {newrows.ToString()} row(s).");
```

## <a name="update-data"></a>Aktualizace dat

K aktualizaci dat v databázi SQL Azure použijte metodu [SqlCommand.ExecuteNonQuery](https://msdn.microsoft.com/library/system.data.sqlclient.sqlcommand.executenonquery.aspx) s příkazem jazyka Transact-SQL [UPDATE](https://msdn.microsoft.com/library/ms177523.aspx).

```csharp
SqlCommand updateCommand = new SqlCommand("", connection);
updateCommand.CommandType = CommandType.Text;
updateCommand.CommandText = @"UPDATE SalesLT.Product SET ListPrice = @ListPrice WHERE Name = @Name";
updateCommand.Parameters.AddWithValue("@Name", "BrandNewProduct");
updateCommand.Parameters.AddWithValue("@ListPrice", 500);

int updatedrows = updateCommand.ExecuteNonQuery();
Console.WriteLine($"Updated {updatedrows.ToString()} row(s).");
```

## <a name="delete-data"></a>Odstranění dat

K odstranění dat z databáze SQL Azure použijte metodu [SqlCommand.ExecuteNonQuery](https://msdn.microsoft.com/library/system.data.sqlclient.sqlcommand.executenonquery.aspx) s příkazem jazyka Transact-SQL [DELETE](https://msdn.microsoft.com/library/ms189835.aspx).

```csharp
SqlCommand deleteCommand = new SqlCommand("", connection);
deleteCommand.CommandType = CommandType.Text;
deleteCommand.CommandText = @"DELETE FROM SalesLT.Product WHERE Name = @Name";
deleteCommand.Parameters.AddWithValue("@Name", "BrandNewProduct");

int deletedrows = deleteCommand.ExecuteNonQuery();
Console.WriteLine($"Deleted {deletedrows.ToString()} row(s).");
```

## <a name="complete-script"></a>Celý skript

Následující skript obsahuje všechny předchozí kroky v jednom bloku kódu.

```csharp
using System;
using System.Data;
using System.Data.SqlClient;

namespace ConsoleApplication1
{
    class Program
    {
        static void Main(string[] args)
        {

            string strConn = "<connection string>";

            using (var connection = new SqlConnection(strConn))
            {
                connection.Open();

                Console.WriteLine("Query data example:");
                Console.WriteLine("\n=========================================\n");

                SqlCommand selectCommand = new SqlCommand("", connection);
                selectCommand.CommandType = CommandType.Text;

                selectCommand.CommandText = @"SELECT TOP 20 pc.Name as CategoryName, p.name as ProductName
                   FROM [SalesLT].[ProductCategory] pc
                   JOIN [SalesLT].[Product] p
                   ON pc.productcategoryid = p.productcategoryid";

                SqlDataReader reader = selectCommand.ExecuteReader();

                while (reader.Read())
                {
                    // show data columns
                    Console.WriteLine($"{reader.GetString(0)}\t{reader.GetString(1)}");
                }
                reader.Close();
                Console.WriteLine("\nPress any key to continue ...");
                Console.ReadLine();

                Console.WriteLine("\nInsert data example:");
                Console.WriteLine("=========================================\n");
                SqlCommand insertCommand = new SqlCommand("", connection);
                insertCommand.CommandType = CommandType.Text;
                insertCommand.CommandText = @"INSERT INTO[SalesLT].[Product]
                          ( [Name]
                          , [ProductNumber]
                          , [Color]
                          , [StandardCost]
                          , [ListPrice]
                          , [SellStartDate]
                          )
                VALUES
                (
                            @Name,
                            @ProductNumber,
                            @Color,
                            @StandardCost,
                            @ListPrice,
                            @SellStartDate)";

                insertCommand.Parameters.AddWithValue("@Name", "BrandNewProduct");
                insertCommand.Parameters.AddWithValue("@ProductNumber", "200989");
                insertCommand.Parameters.AddWithValue("@Color", "Blue");
                insertCommand.Parameters.AddWithValue("@StandardCost", 75);
                insertCommand.Parameters.AddWithValue("@ListPrice", 80);
                insertCommand.Parameters.AddWithValue("@SellStartDate", "7/1/2016");

                int newrows = insertCommand.ExecuteNonQuery();
                Console.WriteLine($"Inserted {newrows.ToString()} row(s).");
                Console.WriteLine("\nPress any key to continue ...");
                Console.ReadLine();

                Console.WriteLine("\nUpdate data example:");
                Console.WriteLine("======================\n");
                SqlCommand updateCommand = new SqlCommand("", connection);
                updateCommand.CommandType = CommandType.Text;
                updateCommand.CommandText = @"UPDATE SalesLT.Product SET ListPrice = @ListPrice WHERE Name = @Name";
                updateCommand.Parameters.AddWithValue("@Name", "BrandNewProduct");
                updateCommand.Parameters.AddWithValue("@ListPrice", 500);

                int updatedrows = updateCommand.ExecuteNonQuery();
                Console.WriteLine($"Updated {updatedrows.ToString()} row(s).");
                Console.WriteLine("\nPress any key to continue ...");
                Console.ReadLine();

                Console.WriteLine("\nDelete data example:");
                Console.WriteLine("======================\n");
                SqlCommand deleteCommand = new SqlCommand("", connection);
                deleteCommand.CommandType = CommandType.Text;
                deleteCommand.CommandText = @"DELETE FROM SalesLT.Product WHERE Name = @Name";
                deleteCommand.Parameters.AddWithValue("@Name", "BrandNewProduct");

                int deletedrows = deleteCommand.ExecuteNonQuery();
                Console.WriteLine($"Deleted {deletedrows.ToString()} row(s).");
                Console.WriteLine("\nPress any key to continue ...");
                Console.ReadLine();
            }
        }
    }
}
```

## <a name="next-steps"></a>Další kroky

- Dokumentaci k technologii .NET najdete na stránce [Dokumentace k technologii .NET](https://docs.microsoft.com/dotnet/).
- Informace o dotazování a upravování dat pomocí nástroje Visual Studio Code najdete na stránce [Visual Studio Code](https://code.visualstudio.com/docs).

