---
title: "Použití sady Visual Studio a .NET k dotazování služby Azure SQL Database | Dokumentace Microsoftu"
description: "Toto téma vám ukáže, jak pomocí sady Visual Studio vytvořit program, který se připojí ke službě Azure SQL Database a bude ji dotazovat s použitím příkazů jazyka Transact-SQL."
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: 
ms.service: sql-database
ms.custom: mvc,develop apps
ms.workload: drivers
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 07/05/2017
ms.author: carlrab
ms.translationtype: HT
ms.sourcegitcommit: 137671152878e6e1ee5ba398dd5267feefc435b7
ms.openlocfilehash: 105dab17823a7e7f6957a604833f4ecad35c14bd
ms.contentlocale: cs-cz
ms.lasthandoff: 07/28/2017

---
# <a name="use-net-c-with-visual-studio-to-connect-and-query-an-azure-sql-database"></a>Použití .NET (jazyk C#) a sady Visual Studio k připojení k databázi SQL Azure a jejímu dotazování

Tento rychlý úvodní kurz ukazuje použití rozhraní [.NET Framework](https://www.microsoft.com/net/) a sady Visual Studio k vytvoření programu v jazyce C# pro připojení k databázi SQL Azure a použití příkazů jazyka Transact-SQL k dotazování dat.

## <a name="prerequisites"></a>Požadavky

Abyste mohli absolvovat tento rychlý úvodní kurz, ujistěte se, že máte následující:

- Databázi SQL Azure. Tento rychlý start používá prostředky vytvořené v některém z těchto rychlých startů: 

   - [Vytvoření databáze – portál](sql-database-get-started-portal.md)
   - [Vytvoření databáze – rozhraní příkazového řádku](sql-database-get-started-cli.md)
   - [Vytvoření databáze – PowerShell](sql-database-get-started-powershell.md)

- [Pravidlo brány firewall na úrovni serveru](sql-database-get-started-portal.md#create-a-server-level-firewall-rule) pro veřejnou IP adresu počítače, který používáte pro tento rychlý úvodní kurz.
- Instalaci sady [Visual Studio Community 2017, Visual Studio Professional 2017 nebo Visual Studio Enterprise 2017](https://www.visualstudio.com/downloads/).

## <a name="sql-server-connection-information"></a>Informace o připojení k SQL serveru

Získejte informace o připojení potřebné pro připojení k databázi SQL Azure. V dalších postupech budete potřebovat plně kvalifikovaný název serveru, název databáze a přihlašovací údaje.

1. Přihlaste se k portálu [Azure Portal](https://portal.azure.com/).
2. V nabídce vlevo vyberte **SQL Database** a na stránce **Databáze SQL** klikněte na vaši databázi. 
3. Na stránce **Přehled** pro vaši databázi si prohlédněte plně kvalifikovaný název serveru, jak je znázorněno na následujícím obrázku. Pokud na název serveru najedete myší, můžete vyvolat možnost **Kopírování kliknutím**. 

   ![název-serveru](./media/sql-database-connect-query-dotnet/server-name.png) 

4. Pokud zapomenete přihlašovací informace pro váš server služby Azure SQL Database, přejděte na stránku serveru služby SQL Database, abyste zobrazili jméno správce serveru. V případě potřeby můžete obnovit heslo.

5. Klikněte na tlačítko **Zobrazit databázové připojovací řetězce**.

6. Zkontrolujte úplný připojovací řetězec **ADO.NET**.

    ![Připojovací řetězec pro ADO.NET](./media/sql-database-connect-query-dotnet/adonet-connection-string.png)

> [!IMPORTANT]
> Musíte mít nastavené pravidlo brány firewall pro veřejnou IP adresu počítače, na kterém provádíte tento kurz. Pokud jste na jiném počítači nebo máte jinou veřejnou IP adresu, vytvořte [pravidlo brány firewall na úrovni serveru pomocí webu Azure Portal](sql-database-get-started-portal.md#create-a-server-level-firewall-rule). 
>
  
## <a name="create-a-new-visual-studio-project"></a>Vytvoření nového projektu v sadě Visual Studio

1. V sadě Visual Studio vyberte **Soubor**, **Nový**, **Projekt**. 
2. V dialogovém okně **Nový projekt** rozbalte **Visual C#**.
3. Vyberte **Konzolová aplikace** a jako název projektu zadejte *sqltest*.
4. Kliknutím na **OK** vytvořte nový projekt a otevřete ho v sadě Visual Studio.
4. V Průzkumníku řešení klikněte pravým tlačítkem na **sqltest** a klikněte na **Správa balíčků NuGet**. 
5. V části **Procházet** vyhledejte a po nalezení vyberte ```System.Data.SqlClient```.
6. Na stránce **System.Data.SqlClient** klikněte na **Nainstalovat**.
7. Po dokončení instalace zkontrolujte změny a potom kliknutím na **OK** zavřete okno **Náhled**. 
8. Pokud se zobrazí okno **Souhlas s podmínkami licence**, klikněte na **Souhlasím**.

## <a name="insert-code-to-query-sql-database"></a>Vložení kódu pro dotazování databáze SQL
1. Přepněte na soubor **Program.cs** (případně ho otevřete).

2. Obsah souboru **Program.cs** nahraďte následujícím kódem a přidejte odpovídající hodnoty pro váš server, databázi, uživatele a heslo.

```csharp
using System;
using System.Data.SqlClient;
using System.Text;

namespace sqltest
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
            Console.ReadLine();
        }
    }
}
```

## <a name="run-the-code"></a>Spuštění kódu

1. Stisknutím klávesy **F5** spusťte aplikaci.
2. Ověřte, že se vrátilo prvních 20 řádků, a potom zavřete okno aplikace.

## <a name="next-steps"></a>Další kroky

- Informace o [připojení k databázi SQL Azure a jejím dotazování pomocí .NET Core](sql-database-connect-query-dotnet-core.md) v systému Windows, Linux nebo macOS  
- Informace o tom, [jak začít s .NET Core v systému Windows, Linux nebo macOS pomocí příkazového řádku](/dotnet/core/tutorials/using-with-xplat-cli)
- Informace o [návrhu první databáze SQL Azure pomocí aplikace SSMS](sql-database-design-first-database.md) nebo [návrhu první databáze SQL Azure pomocí .NET](sql-database-design-first-database-csharp.md)
- Další informace o .NET najdete v [dokumentaci rozhraní .NET](https://docs.microsoft.com/dotnet/).

