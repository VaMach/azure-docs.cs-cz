---
title: "Připojení ke službě Azure SQL Database pomocí technologie .NET (jazyk C#) | Dokumentace Microsoftu"
description: "Obsahuje ukázku kódu .NET, který můžete použít k připojení a dotazování Azure SQL Database."
services: sql-database
documentationcenter: 
author: ajlam
manager: jhubbard
editor: 
ms.assetid: 7faca033-24b4-4f64-9301-b4de41e73dfd
ms.service: sql-database
ms.custom: quick start connect
ms.workload: drivers
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 04/05/2017
ms.author: andrela;sstein;carlrab
translationtype: Human Translation
ms.sourcegitcommit: 785d3a8920d48e11e80048665e9866f16c514cf7
ms.openlocfilehash: c36c3a3f651bcee38b953b12e48cab8d93a34207
ms.lasthandoff: 04/12/2017


---
# <a name="azure-sql-database-use-net-c-to-connect-and-query-data"></a>Azure SQL Database: Použití technologie .NET (jazyk C#) k připojení a dotazování dat

Tento rychlý start ukazuje použití [C# a ADO.NET](https://msdn.microsoft.com/library/kb9s9ks0.aspx) k připojení k Azure SQL Database a následné použití příkazů jazyka Transact-SQL k dotazování, vkládání, aktualizaci a odstraňování dat v databázi z platforem Windows, Mac OS a Ubuntu Linux.

Tento rychlý start používá jako výchozí bod prostředky vytvořené v některém z těchto rychlých startů:

- [Vytvoření databáze – portál](sql-database-get-started-portal.md)
- [Vytvoření databáze – rozhraní příkazového řádku](sql-database-get-started-cli.md)

## <a name="install-net"></a>Instalace rozhraní .NET

### <a name="windows-net-framework-and-net-core"></a>**Windows .NET Framework a .NET Core**

Visual Studio 2017 Community je plně vybavené, rozšiřitelné a bezplatné prostředí IDE pro vytváření moderních aplikací pro Android, iOS a Windows, stejně jako webových a databázových aplikací a cloudových služeb. Můžete nainstalovat buď úplné rozhraní .NET Framework, nebo jenom jádro .NET Core. Fragmenty kódu v rychlém startu pracují s oběma. Pokud již máte v počítači nainstalovanou sadu Visual Studio, přeskočte několik dalších kroků.

1. Stáhněte [instalační program](https://go.microsoft.com/fwlink/?LinkId=691978). 
2. Spusťte instalační program a podle zobrazených pokynů instalaci dokončete.

### <a name="mac-os"></a>**Mac OS**
Otevřete terminál a přejděte do adresáře, kde plánujete vytvoření projektu .NET Core. Zadejte následující příkazy, abyste nainstalovali **brew**, **OpenSSL** a **.NET Core**. 

```bash
ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
brew update
brew install openssl
mkdir -p /usr/local/lib
ln -s /usr/local/opt/openssl/lib/libcrypto.1.0.0.dylib /usr/local/lib/
ln -s /usr/local/opt/openssl/lib/libssl.1.0.0.dylib /usr/local/lib/
```

V systému macOS nainstalujte .NET Core. Stáhněte [oficiální instalační program](https://go.microsoft.com/fwlink/?linkid=843444). Tento instalační program nainstaluje nástroje a umístí je do proměnné PATH, abyste mohli dotnet spustit z konzoly.

### <a name="linux-ubuntu"></a>**Linux (Ubuntu)**
Otevřete terminál a přejděte do adresáře, kde plánujete vytvoření projektu .NET Core. Zadáním následujících příkazů nainstalujte **.NET Core**.

```bash
sudo sh -c 'echo "deb [arch=amd64] https://apt-mo.trafficmanager.net/repos/dotnet-release/ xenial main" > /etc/apt/sources.list.d/dotnetdev.list'
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 417A0893
sudo apt-get update
sudo apt-get install dotnet-dev-1.0.1
```

## <a name="get-connection-information"></a>Získání informací o připojení

Na webu Azure Portal získejte připojovací řetězec. Připojovací řetězec použijete k připojení k databázi SQL Azure.

1. Přihlaste se k [portálu Azure](https://portal.azure.com/).
2. V nabídce vlevo vyberte **SQL Database** a na stránce **Databáze SQL** klikněte na vaši databázi. 
3. V podokně **Základy** pro vaši databázi zkontrolujte plně kvalifikovaný název serveru. 

    <img src="./media/sql-database-connect-query-dotnet/connection-strings.png" alt="connection strings" style="width: 780px;" />

4. Klikněte na tlačítko **Zobrazit databázové připojovací řetězce**.

5. Zkontrolujte úplný připojovací řetězec **ADO.NET**.

    <img src="./media/sql-database-connect-query-dotnet/adonet-connection-string.png" alt="ADO.NET connection string" style="width: 780px;" />
    
## <a name="add-systemdatasqlclient"></a>Přidání oboru názvů System.Data.SqlClient
Pokud používáte .NET Core, přidejte do souboru ***csproj*** projektu System.Data.SqlClient jako závislost.

```xml
<ItemGroup>
    <PackageReference Include="System.Data.SqlClient" Version="4.3.0" />
</ItemGroup>
```

## <a name="select-data"></a>Výběr dat

1. Ve svém vývojovém prostředí otevřete prázdný soubor kódu.
2. Přidejte ```using System.Data.SqlClient``` do souboru kódu ([obor názvů System.Data.SqlClient](https://msdn.microsoft.com/library/system.data.sqlclient.aspx)). 

3. K dotazování dat v databázi SQL Azure použijte metodu [SqlCommand.ExecuteReader](https://msdn.microsoft.com/library/system.data.sqlclient.sqlcommand.executereader.aspx) s příkazem jazyka Transact-SQL [SELECT](https://msdn.microsoft.com/library/ms189499.aspx). Přidání příslušných hodnot pro váš server

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
            try 
            { 
                SqlConnectionStringBuilder builder = new SqlConnectionStringBuilder();
                builder.DataSource = "your_server.database.windows.net"; 
                builder.UserID = "your_user";            
                builder.Password = "your_password";     
                builder.InitialCatalog = "your_database";

                using (SqlConnection connection = new SqlConnection(builder.ConnectionString))
                {
                    Console.WriteLine("\nQuery data example:");
                    Console.WriteLine("=========================================\n");
                    
                    connection.Open();       
                    StringBuilder sb = new StringBuilder();
                    sb.Append("SELECT TOP 20 pc.Name as CategoryName, p.name as ProductName ");
                    sb.Append("FROM [SalesLT].[ProductCategory] pc ");
                    sb.Append("JOIN [SalesLT].[Product] p ");
                    sb.Append("ON pc.productcategoryid = p.productcategoryid;");
                    String sql = sb.ToString();

                    using (SqlCommand command = new SqlCommand(sql, connection))
                    {
                        using (SqlDataReader reader = command.ExecuteReader())
                        {
                            while (reader.Read())
                            {
                                Console.WriteLine("{0} {1}", reader.GetString(0), reader.GetString(1));
                            }
                        }
                    }                    
                }
            }
            catch (SqlException e)
            {
                Console.WriteLine(e.ToString());
            }
        }
    }
}
```

## <a name="insert-data"></a>Vložení dat

K vložení dat do databáze SQL Azure použijte metodu [SqlCommand.ExecuteNonQuery](https://msdn.microsoft.com/library/system.data.sqlclient.sqlcommand.executenonquery.aspx) s příkazem jazyka Transact-SQL [INSERT](https://msdn.microsoft.com/library/ms174335.aspx).

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
            try 
            { 
                SqlConnectionStringBuilder builder = new SqlConnectionStringBuilder();
                builder.DataSource = "your_server.database.windows.net"; 
                builder.UserID = "your_user";            
                builder.Password = "your_password";     
                builder.InitialCatalog = "your_database";

                using (SqlConnection connection = new SqlConnection(builder.ConnectionString))
                {
                    Console.WriteLine("\nInsert data example:");
                    Console.WriteLine("=========================================\n");

                    connection.Open();       
                    StringBuilder sb = new StringBuilder();
                    sb.Append("INSERT INTO [SalesLT].[Product] ([Name], [ProductNumber], [Color], [StandardCost], [ListPrice], [SellStartDate]) ");
                    sb.Append("VALUES (@Name, @ProductNumber, @Color, @StandardCost, @ListPrice, @SellStartDate);");
                    String sql = sb.ToString();
                    using (SqlCommand command = new SqlCommand(sql, connection))
                    {
                        command.Parameters.AddWithValue("@Name", "BrandNewProduct");
                        command.Parameters.AddWithValue("@ProductNumber", "200989");
                        command.Parameters.AddWithValue("@Color", "Blue");
                        command.Parameters.AddWithValue("@StandardCost", 75);
                        command.Parameters.AddWithValue("@ListPrice", 80);
                        command.Parameters.AddWithValue("@SellStartDate", "7/1/2016");
                        int rowsAffected = command.ExecuteNonQuery();
                        Console.WriteLine(rowsAffected + " row(s) inserted");
                    }         
                }
            }
            catch (SqlException e)
            {
                Console.WriteLine(e.ToString());
            }
        }
    }
}
```

## <a name="update-data"></a>Aktualizace dat

K aktualizaci dat v databázi SQL Azure použijte metodu [SqlCommand.ExecuteNonQuery](https://msdn.microsoft.com/library/system.data.sqlclient.sqlcommand.executenonquery.aspx) s příkazem jazyka Transact-SQL [UPDATE](https://msdn.microsoft.com/library/ms177523.aspx).

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
            try 
            { 
                SqlConnectionStringBuilder builder = new SqlConnectionStringBuilder();
                builder.DataSource = "your_server.database.windows.net"; 
                builder.UserID = "your_user";            
                builder.Password = "your_password";     
                builder.InitialCatalog = "your_database";

                using (SqlConnection connection = new SqlConnection(builder.ConnectionString))
                {
                    Console.WriteLine("\nUpdate data example");
                    Console.WriteLine("=========================================\n");

                    connection.Open();       
                    StringBuilder sb = new StringBuilder();
                    sb.Append("UPDATE [SalesLT].[Product] SET ListPrice = @ListPrice WHERE Name = @Name;");
                    String sql = sb.ToString();
                    using (SqlCommand command = new SqlCommand(sql, connection))
                    {
                        command.Parameters.AddWithValue("@Name", "BrandNewProduct");
                        command.Parameters.AddWithValue("@ListPrice", 500);
                        int rowsAffected = command.ExecuteNonQuery();
                        Console.WriteLine(rowsAffected + " row(s) updated");
                    }         
                }
            }
            catch (SqlException e)
            {
                Console.WriteLine(e.ToString());
            }
        }
    }
}
```

## <a name="delete-data"></a>Odstranění dat

K odstranění dat z databáze SQL Azure použijte metodu [SqlCommand.ExecuteNonQuery](https://msdn.microsoft.com/library/system.data.sqlclient.sqlcommand.executenonquery.aspx) s příkazem jazyka Transact-SQL [DELETE](https://msdn.microsoft.com/library/ms189835.aspx).

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
            try 
            { 
                SqlConnectionStringBuilder builder = new SqlConnectionStringBuilder();
                builder.DataSource = "your_server.database.windows.net"; 
                builder.UserID = "your_user";            
                builder.Password = "your_password";     
                builder.InitialCatalog = "your_database";

                using (SqlConnection connection = new SqlConnection(builder.ConnectionString))
                {
                    Console.WriteLine("\nDelete data example");
                    Console.WriteLine("=========================================\n");

                    connection.Open();       
                    StringBuilder sb = new StringBuilder();
                    sb.Append("DELETE FROM SalesLT.Product WHERE Name = @Name;");
                    String sql = sb.ToString();
                    using (SqlCommand command = new SqlCommand(sql, connection))
                    {
                        command.Parameters.AddWithValue("@Name", "BrandNewProduct");
                        int rowsAffected = command.ExecuteNonQuery();
                        Console.WriteLine(rowsAffected + " row(s) deleted");
                    }         
                }
            }
            catch (SqlException e)
            {
                Console.WriteLine(e.ToString());
            }
        }
    }
}
```

## <a name="next-steps"></a>Další kroky

- Dokumentaci k technologii .NET najdete na stránce [Dokumentace k technologii .NET](https://docs.microsoft.com/dotnet/).
- Informace o připojení a dotazování pomocí aplikace SQL Server Management Studio najdete v tématu [Připojení a dotazování pomocí SSMS](sql-database-connect-query-ssms.md).
- Informace o připojení a dotazování pomocí sady Visual Studio najdete v tématu [Připojení a dotazování pomocí Visual Studio Code](sql-database-connect-query-vscode.md).
- Informace o připojení a dotazování pomocí PHP najdete v tématu [Připojení a dotazování pomocí PHP](sql-database-connect-query-php.md).
- Informace o připojení a dotazování pomocí Node.js najdete v tématu [Připojení a dotazování pomocí Node.js](sql-database-connect-query-nodejs.md).
- Informace o připojení a dotazování pomocí Javy najdete v tématu [Připojení a dotazování pomocí Javy](sql-database-connect-query-java.md).
- Informace o připojení a dotazování pomocí Pythonu najdete v tématu [Připojení a dotazování pomocí Pythonu](sql-database-connect-query-python.md).
- Informace o připojení a dotazování pomocí Ruby najdete v tématu [Připojení a dotazování pomocí Ruby](sql-database-connect-query-ruby.md).

