---
title: "Připojení k Azure Database for PostgreSQL z C# | Dokumentace Microsoftu"
description: "V tomto rychlém startu najdete vzorový kód C# (.Net), který můžete použít k připojení a dotazování dat z databáze Azure Database for PostgreSQL."
services: postgresql
author: jasonwhowell
ms.author: jasonh
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql-database
ms.custom: mvc
ms.devlang: csharp
ms.topic: hero-article
ms.date: 06/23/2017
ms.translationtype: Human Translation
ms.sourcegitcommit: 857267f46f6a2d545fc402ebf3a12f21c62ecd21
ms.openlocfilehash: 87854637471fa15a76ae216cb57c549962810b7b
ms.contentlocale: cs-cz
ms.lasthandoff: 06/28/2017

---

<a id="azure-database-for-postgresql-use-net-c-to-connect-and-query-data" class="xliff"></a>

# Azure Database for PostgreSQL: Použití technologie .NET (jazyk C#) k připojení a dotazování dat
Tento rychlý start ukazuje, jak se připojit ke službě Azure Database for MySQL pomocí aplikace v C#. Ukazuje, jak pomocí příkazů jazyka SQL dotazovat, vkládat, aktualizovat a odstraňovat data v databázi. Kroky v tomto článku předpokládají, že máte zkušenosti s vývojem pomocí C# a teprve začínáte pracovat se službou Azure Database for PostgreSQL.

<a id="prerequisites" class="xliff"></a>

## Požadavky
Tento rychlý start jako výchozí bod využívá prostředky vytvořené v některém z těchto průvodců:
- [Vytvoření databáze – portál](quickstart-create-server-database-portal.md)
- [Vytvoření databáze – rozhraní příkazového řádku](quickstart-create-server-database-azure-cli.md)

Budete také muset:
- Nainstalovat [.Net Framework](https://www.microsoft.com/net/download)
- Nainstalovat [Visual Studio](https://www.visualstudio.com/downloads/)
- Nainstalovat [Npgsql](http://www.npgsql.org/doc/index.html) 

<a id="install-visual-studio-and-net" class="xliff"></a>

## Instalace sady Visual Studio a .NET
Kroky v této části předpokládají, že máte zkušenosti s vývojem pomocí rozhraní .NET.

<a id="windows-net-framework-and-net-core" class="xliff"></a>

### **Windows .NET Framework a .NET Core**
Visual Studio 2017 Community je plně vybavené, rozšiřitelné a bezplatné integrované vývojové prostředí (IDE) pro vytváření moderních aplikací pro Android, iOS a Windows, stejně jako webových a databázových aplikací a cloudových služeb. Můžete nainstalovat buď úplné rozhraní .NET Framework, nebo jenom jádro .NET Core. Fragmenty kódu v rychlém startu pracují s oběma. Pokud již máte v počítači nainstalovanou sadu Visual Studio, přeskočte několik dalších kroků.

1. Stáhněte si [instalační program sady Visual Studio 2017](https://www.visualstudio.com/thank-you-downloading-visual-studio/?sku=Community&rel=15). 
2. Spusťte instalační program a podle zobrazených pokynů instalaci dokončete.

<a id="mac-os" class="xliff"></a>

### **Mac OS**
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

<a id="linux-ubuntu" class="xliff"></a>

### **Linux (Ubuntu)**
Otevřete terminál a přejděte do adresáře, kde plánujete vytvoření projektu .NET Core. Zadáním následujících příkazů nainstalujte **.NET Core**.

```bash
sudo sh -c 'echo "deb [arch=amd64] https://apt-mo.trafficmanager.net/repos/dotnet-release/ xenial main" > /etc/apt/sources.list.d/dotnetdev.list'
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 417A0893
sudo apt-get update
sudo apt-get install dotnet-dev-1.0.1
```

<a id="install-npgsql-references-into-your-visual-studio-solution" class="xliff"></a>

## Instalace referencí Npgsql do řešení v sadě Visual Studio
Pro připojení z aplikace v C# k PostgreSQL použijte opensourcovou knihovnu ADO.Net s názvem Npgsql. Ve snadném stažení a správě referencí vám pomůže NuGet.

1. Vytvořte nové řešení v C# nebo otevřete řešení, které už existuje: 
   - V sadě Visual Studio vytvoříte řešení kliknutím na **Nový** > **Projekt** v nabídce Soubor.
   - V dialogovém okně Nový projekt rozbalte **Šablony** > **Visual C#**. 
   - Zvolte odpovídající šablonu, třeba **Aplikace konzoly (.NET Core)**.

2. Pomocí Správce balíčků Nuget nainstalujte Npgsql:
   - Klikněte na **Nástroje** > **Správce balíčků NuGet** > **Konzola správce balíčků**.
   - V **Konzole Správce balíčků** zadejte `Install-Package Npgsql`.
   - Příkaz Install stáhne Npgsql.dll a související sestavení a přidá je do řešení jako závislosti.

<a id="get-connection-information" class="xliff"></a>

## Získání informací o připojení
Získejte informace o připojení potřebné pro připojení ke službě Azure Database for PostgreSQL. Potřebujete plně kvalifikovaný název serveru a přihlašovací údaje.

1. Přihlaste se k [portálu Azure](https://portal.azure.com/).
2. V nabídce vlevo na webu Azure Portal klikněte na **Všechny prostředky** a vyhledejte vytvořený server, například **mypgserver-20170401**.
3. Klikněte na název serveru **mypgserver-20170401**.
4. Vyberte stránku **Přehled** serveru. Poznamenejte si **Název serveru** a **Přihlašovací jméno správce serveru**.
 ![Azure Database for PostgreSQL – přihlášení správce serveru](./media/connect-csharp/1-connection-string.png)
5. Pokud zapomenete přihlašovací údaje pro váš server, přejděte na stránku **Přehled**, kde můžete zobrazit přihlašovací jméno správce serveru a v případě potřeby obnovit heslo.

<a id="connect-create-table-and-insert-data" class="xliff"></a>

## Připojení, vytvoření tabulky a vložení dat
Použijte následující kód k připojení a načtení dat pomocí příkazů jazyka SQL **CREATE TABLE** a **INSERT INTO**. Tento kód pro navázání připojení k PostgreSQL využívá třídu NpgsqlCommand s metodou [Open()](http://www.npgsql.org/api/Npgsql.NpgsqlConnection.html#Npgsql_NpgsqlConnection_Open). Potom tento kód použije metodu [CreateCommand()](http://www.npgsql.org/api/Npgsql.NpgsqlConnection.html#Npgsql_NpgsqlConnection_CreateCommand), nastaví vlastnost CommandText a volá metodu [ExecuteNonQuery()](http://www.npgsql.org/api/Npgsql.NpgsqlCommand.html#Npgsql_NpgsqlCommand_ExecuteNonQuery) pro spuštění databázových příkazů. 

Nahraďte parametry Host (hostitel), DBName (název databáze), User (uživatel) a Password (heslo) hodnotami, které jste zadali při vytváření vlastního serveru a databáze. 

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using Npgsql;

namespace Driver
{
    public class AzurePostgresCreate
    {
        // Obtain connection string information from the portal
        //
        private static string Host = "mypgserver-20170401.postgres.database.azure.com";
        private static string User = "mylogin@mypgserver-20170401";
        private static string DBname = "mypgsqldb";
        private static string Password = "<server_admin_password>";
        private static string Port = "5432";

        static void Main(string[] args)
        {
            // Build connection string using parameters from portal
            //
            string connString =
                String.Format(
                    "Server={0}; User Id={1}; Database={2}; Port={3}; Password={4};",
                    Host,
                    User,
                    DBname,
                    Port,
                    Password);

            var conn = new NpgsqlConnection(connString);

            Console.Out.WriteLine("Opening connection");
            conn.Open();

            var command = conn.CreateCommand();
            command.CommandText = "DROP TABLE IF EXISTS inventory;";
            command.ExecuteNonQuery();
            Console.Out.WriteLine("Finished dropping table (if existed)");

            command.CommandText = "CREATE TABLE inventory (id serial PRIMARY KEY, name VARCHAR(50), quantity INTEGER);";
            command.ExecuteNonQuery();
            Console.Out.WriteLine("Finished creating table");

            command.CommandText =
                String.Format(
                    @"
                                INSERT INTO inventory (name, quantity) VALUES ({0}, {1});
                                INSERT INTO inventory (name, quantity) VALUES ({2}, {3});
                                INSERT INTO inventory (name, quantity) VALUES ({4}, {5});
                            ",
                    "\'banana\'", 150,
                    "\'orange\'", 154,
                    "\'apple\'", 100
                    );

            int nRows = command.ExecuteNonQuery();
            Console.Out.WriteLine(String.Format("Number of rows inserted={0}", nRows));

            Console.Out.WriteLine("Closing connection");
            conn.Close();

            Console.WriteLine("Press RETURN to exit");
            Console.ReadLine();
        }
    }
}
```

<a id="read-data" class="xliff"></a>

## Čtení dat
Pomocí následujícího kódu se připojte a načtěte data s využitím příkazu **SELECT** jazyka SQL. Tento kód pro navázání připojení k PostgreSQL využívá třídu NpgsqlCommand s metodou [Open()](http://www.npgsql.org/api/Npgsql.NpgsqlConnection.html#Npgsql_NpgsqlConnection_Open). Potom tento kód použije metodu [CreateCommand()](http://www.npgsql.org/api/Npgsql.NpgsqlConnection.html#Npgsql_NpgsqlConnection_CreateCommand) a metodu [ExecuteReader()](http://www.npgsql.org/api/Npgsql.NpgsqlCommand.html#Npgsql_NpgsqlCommand_ExecuteReader) pro spuštění databázových příkazů. Dál tento kód použije [Read()](http://www.npgsql.org/api/Npgsql.NpgsqlDataReader.html#Npgsql_NpgsqlDataReader_Read) k přechodu na záznamy ve výsledcích. Potom tento kód použije [GetInt32()](http://www.npgsql.org/api/Npgsql.NpgsqlDataReader.html#Npgsql_NpgsqlDataReader_GetInt32_System_Int32_) a [GetString()](http://www.npgsql.org/api/Npgsql.NpgsqlDataReader.html#Npgsql_NpgsqlDataReader_GetString_System_Int32_) k analýze hodnot v záznamu.

Nahraďte parametry Host (hostitel), DBName (název databáze), User (uživatel) a Password (heslo) hodnotami, které jste zadali při vytváření vlastního serveru a databáze. 

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using Npgsql;

namespace Driver
{
    public class AzurePostgresRead
    {
        // Obtain connection string information from the portal
        //
        private static string Host = "mypgserver-20170401.postgres.database.azure.com";
        private static string User = "mylogin@mypgserver-20170401";
        private static string DBname = "mypgsqldb";
        private static string Password = "<server_admin_password>";
        private static string Port = "5432";

        static void Main(string[] args)
        {
            // Build connection string using parameters from portal
            //
            string connString =
                String.Format(
                    "Server={0}; User Id={1}; Database={2}; Port={3}; Password={4};",
                    Host,
                    User,
                    DBname,
                    Port,
                    Password);

            var conn = new NpgsqlConnection(connString);

            Console.Out.WriteLine("Opening connection");
            conn.Open();

            var command = conn.CreateCommand();
            command.CommandText = "SELECT * FROM inventory;";

            var reader = command.ExecuteReader();
            while (reader.Read())
            {
                Console.WriteLine(
                    string.Format(
                        "Reading from table=({0}, {1}, {2})",
                        reader.GetInt32(0).ToString(),
                        reader.GetString(1),
                        reader.GetInt32(2).ToString()
                        )
                    );
            }

            Console.Out.WriteLine("Closing connection");
            conn.Close();

            Console.WriteLine("Press RETURN to exit");
            Console.ReadLine();
        }
    }
}
```


<a id="update-data" class="xliff"></a>

## Aktualizace dat
Použijte následující kód k připojení a čtení dat pomocí příkazu jazyka SQL **UPDATE**. Tento kód pro navázání připojení k PostgreSQL využívá třídu NpgsqlCommand s metodou [Open()](http://www.npgsql.org/api/Npgsql.NpgsqlConnection.html#Npgsql_NpgsqlConnection_Open). Potom tento kód použije metodu [CreateCommand()](http://www.npgsql.org/api/Npgsql.NpgsqlConnection.html#Npgsql_NpgsqlConnection_CreateCommand), nastaví vlastnost CommandText a volá metodu [ExecuteNonQuery()](http://www.npgsql.org/api/Npgsql.NpgsqlCommand.html#Npgsql_NpgsqlCommand_ExecuteNonQuery) pro spuštění databázových příkazů.

Nahraďte parametry Host (hostitel), DBName (název databáze), User (uživatel) a Password (heslo) hodnotami, které jste zadali při vytváření vlastního serveru a databáze. 

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using Npgsql;

namespace Driver
{
    public class AzurePostgresUpdate
    {
        // Obtain connection string information from the portal
        //
        private static string Host = "mypgserver-20170401.postgres.database.azure.com";
        private static string User = "mylogin@mypgserver-20170401";
        private static string DBname = "mypgsqldb";
        private static string Password = "<server_admin_password>";
        private static string Port = "5432";

        static void Main(string[] args)
        {
            // Build connection string using parameters from portal
            //
            string connString =
                String.Format(
                    "Server={0}; User Id={1}; Database={2}; Port={3}; Password={4};",
                    Host,
                    User,
                    DBname,
                    Port,
                    Password);

            var conn = new NpgsqlConnection(connString);

            Console.Out.WriteLine("Opening connection");
            conn.Open();

            var command = conn.CreateCommand();
            command.CommandText =
            String.Format("UPDATE inventory SET quantity = {0} WHERE name = {1};",
                200,
                "\'banana\'"
                );

            int nRows = command.ExecuteNonQuery();
            Console.Out.WriteLine(String.Format("Number of rows updated={0}", nRows));

            Console.Out.WriteLine("Closing connection");
            conn.Close();

            Console.WriteLine("Press RETURN to exit");
            Console.ReadLine();
        }
    }
}
```


<a id="delete-data" class="xliff"></a>

## Odstranění dat
Pomocí následujícího kódu se připojte a načtěte data s využitím příkazu **DELETE** jazyka SQL. 

 Tento kód pro navázání připojení k PostgreSQL využívá třídu NpgsqlCommand s metodou [Open()](http://www.npgsql.org/api/Npgsql.NpgsqlConnection.html#Npgsql_NpgsqlConnection_Open). Potom tento kód použije metodu [CreateCommand()](http://www.npgsql.org/api/Npgsql.NpgsqlConnection.html#Npgsql_NpgsqlConnection_CreateCommand), nastaví vlastnost CommandText a volá metodu [ExecuteNonQuery()](http://www.npgsql.org/api/Npgsql.NpgsqlCommand.html#Npgsql_NpgsqlCommand_ExecuteNonQuery) pro spuštění databázových příkazů.

Nahraďte parametry Host (hostitel), DBName (název databáze), User (uživatel) a Password (heslo) hodnotami, které jste zadali při vytváření vlastního serveru a databáze. 

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using Npgsql;

namespace Driver
{
    public class AzurePostgresDelete
    {
        // Obtain connection string information from the portal
        //
        private static string Host = "mypgserver-20170401.postgres.database.azure.com";
        private static string User = "mylogin@mypgserver-20170401";
        private static string DBname = "mypgsqldb";
        private static string Password = "<server_admin_password>";
        private static string Port = "5432";

        static void Main(string[] args)
        {
            // Build connection string using parameters from portal
            //
            string connString =
                String.Format(
                    "Server={0}; User Id={1}; Database={2}; Port={3}; Password={4};",
                    Host,
                    User,
                    DBname,
                    Port,
                    Password);

            var conn = new NpgsqlConnection(connString);

            Console.Out.WriteLine("Opening connection");
            conn.Open();

            var command = conn.CreateCommand();
            command.CommandText =
            String.Format("DELETE FROM inventory WHERE name = {0};",
                "\'orange\'");
            int nRows = command.ExecuteNonQuery();
            Console.Out.WriteLine(String.Format("Number of rows deleted={0}", nRows));

            Console.Out.WriteLine("Closing connection");
            conn.Close();

            Console.WriteLine("Press RETURN to exit");
            Console.ReadLine();
        }
    }
}
```

<a id="next-steps" class="xliff"></a>

## Další kroky
> [!div class="nextstepaction"]
> [Migrace vaší databáze pomocí exportu a importu](./howto-migrate-using-export-and-import.md)

