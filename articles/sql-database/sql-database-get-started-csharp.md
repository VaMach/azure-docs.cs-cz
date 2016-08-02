<properties
    pageTitle="Vyzkoušejte SQL Database: Použití jazyka C# k vytvoření databáze SQL | Microsoft Azure"
    description="Vyzkoušejte SQL Database při vývoji aplikací v jazyce C# využívajících SQL a vytvořte Azure SQL Database v C# s použitím knihovny SQL Database Library pro .NET."
    keywords="try sql, sql c#"   
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="hero-article"
   ms.tgt_pltfrm="csharp"
   ms.workload="data-management"
   ms.date="05/26/2016"
   ms.author="sstein"/>

# Vyzkoušejte SQL Database: Použijte C# k vytvoření databáze SQL pomocí knihovny SQL Database Library pro .NET.


> [AZURE.SELECTOR]
- [Portál Azure](sql-database-get-started.md)
- [C#](sql-database-get-started-csharp.md)
- [PowerShell](sql-database-get-started-powershell.md)

Naučte se používat příkazy jazyka C# k vytvoření Azure SQL Database v C# pomocí [knihovny SQL Database Library pro .NET](https://www.nuget.org/packages/Microsoft.Azure.Management.Sql). SQL Database si můžete vyzkoušet vytvořením jedné databáze pomocí příkazů SQL a C#. Chcete-li vytvářet fondy elastické databáze, přečtěte si článek [Vytvoření fondu elastické databáze](sql-database-elastic-pool-create-portal.md). Jednotlivé fragmenty kódu jsou pro přehlednost rozděleny a na konci tohoto článku najdete ukázkovou aplikaci konzoly, která spojuje všechny příkazy dohromady.

Azure SQL Database Library pro .NET poskytuje rozhraní API založené na [Azure Resource Manageru](../resource-group-overview.md), které zabaluje rozhraní [SQL Database REST API založené na Správci prostředků](https://msdn.microsoft.com/library/azure/mt163571.aspx). Tato klientská knihovna používá stejné postupy, jako ostatní klientské knihovny založené na Správci prostředků. Správci prostředků vyžaduje skupiny prostředků a ověřování pomocí AAD ([Azure Active Directory](https://msdn.microsoft.com/library/azure/mt168838.aspx)).

<br>

> [AZURE.NOTE] SQL Database Library pro .NET je aktuálně ve verzi Preview.

<br>

K dokončení kroků v tomto článku budete potřebovat následující:

- Předplatné Azure. Pokud potřebujete předplatné Azure, jednoduše klikněte na tlačítko **Bezplatná zkušební verze** v horní části stránky a pak se vraťte k dokončení tohoto článku.
- Visual Studio. Bezplatnou kopii sady Visual Studio naleznete na stránce [Soubory ke stažení pro Visual Studio](https://www.visualstudio.com/downloads/download-visual-studio-vs).


## Instalace potřebných knihoven

Než začnete vytvářet databáze SQL pomocí C#, získejte potřebné knihovny pro správu, nejlépe instalací následujících balíčků pomocí [Konzoly správce balíčků](http://docs.nuget.org/Consume/Package-Manager-Console) v sadě Visual Studio (**Nástroje** > **Správce balíčků NuGet** > **Konzola správce balíčků**):

    Install-Package Microsoft.Azure.Management.Sql –Pre
    Install-Package Microsoft.Azure.Management.ResourceManager –Pre
    Install-Package Microsoft.Azure.Common.Authentication –Pre


## Konfigurace ověřování s Azure Active Directory

Nejprve je nutné povolit klientské aplikaci přístup ke službě SQL Database nastavením potřebného ověřování.

Chcete-li klientskou aplikaci ověřovat na základě aktuálního uživatele, je nutné ji nejprve zaregistrovat v doméně AAD přidružené k předplatnému, pod kterým byly prostředky Azure vytvořeny. Pokud jste předplatné Azure vytvořili s účtem Microsoft namísto pracovního nebo školního účtu, již máte výchozí doménu AAD.

Pokud chcete vytvořit novou aplikaci a zaregistrovat ji do správného adresáře, proveďte následující:

1. Přejděte na [portál Azure Classic](https://manage.windowsazure.com/).
1. Na levé straně vyberte službu **Active Directory**, vyberte adresář, pro který chcete aplikaci ověřovat, a klikněte jeho **Název**.

    ![Vyzkoušejte SQL Database: Nastavení Azure Active Directory (AAD).][1]

2. Na stránce adresáře klikněte na **APLIKACE**.

    ![Stránka adresáře s aplikacemi.][5]

4. Kliknutím na **PŘIDAT** vytvořte novou aplikaci.

5. Vyberte možnost **Přidat aplikaci, kterou vyvíjí moje organizace**.

5. Zadejte **NÁZEV** aplikace a vyberte **NATIVNÍ KLIENTSKÁ APLIKACE**.

    ![Zadejte následující informace o vaší aplikaci SQL C#:][7]

6. Zadejte identifikátor **REDIRECT URI**. Nemusí jít o skutečný koncový bod, stačí platný identifikátor URI.

    ![Přidejte k aplikaci SQL C# adresu URL pro přesměrování.][8]

7. Dokončete vytváření aplikace, klikněte na **KONFIGUROVAT** a zkopírujte **ID KLIENTA** (ID klienta budete potřebovat později ve vašem kódu).

    ![Získejte ID klienta pro vaši aplikaci SQL C#.][9]


1. Na konci stránky klikněte na **Přidat aplikaci**.
1. Vyberte **Aplikace Microsoft**.
1. Vyberte **Azure Service Management API** a dokončete průvodce.
2. S vybraným rozhraním API je teď třeba udělit konkrétní oprávnění požadovaná pro přístup k tomuto rozhraní API výběrem možnosti **Přístup k Azure Service Managementu (Preview)**.

    ![Nastavte oprávnění.][2]

2. Klikněte na **ULOŽIT**.



### Zjištění názvu domény

Název domény budete potřebovat pro svůj kód. Snadná cesta k nalezení správného názvu domény je tato:

1. Přejděte na [portál Azure](http://portal.azure.com).
2. Umístěte ukazatel nad vaše jméno v pravém horním rohu a poznamenejte si doménu, která se zobrazí v místním okně.

    ![Zjištění názvu domény.][3]





**Další zdroje AAD**  

Další informace o použití služby Azure Active Directory k ověřování najdete v [tomto užitečném článku na blogu](http://www.cloudidentity.com/blog/2013/09/12/active-directory-authentication-library-adal-v1-for-net-general-availability/).


### Získání přístupového tokenu pro aktuálního uživatele

Klientská aplikace musí získat přístupový token aplikace pro aktuálního uživatele. Při prvním spuštění tohoto kódu uživatelem se zobrazí výzva k zadání přihlašovacích údajů a výsledný token se uloží v místní mezipaměti. Při dalších spuštěních se bude token načítat z mezipaměti a uživatel se bude muset opět přihlásit jen v případě, že vyprší platnost tokenu.

Tento kód vrátí hodnotu Microsoft.IdentityModel.Clients.ActiveDirectory.AuthenticationResult, kterou budete potřebovat v dalších fragmentech kódu níže.

        private static AuthenticationResult GetAccessToken()
        {
            AuthenticationContext authContext = new AuthenticationContext
                ("https://login.windows.net/" + domainName /* Tenant ID or AAD domain */);

            AuthenticationResult token = authContext.AcquireToken
                ("https://management.azure.com/"/* the Azure Resource Management endpoint */,
                    clientId,
            new Uri(redirectUri) /* redirect URI */,
            PromptBehavior.Auto /* with Auto user will not be prompted if an unexpired token is cached */);

            return token;
        }



> [AZURE.NOTE] Příklady v tomto článku používají synchronní formu každého požadavku rozhraní API a provádění se tedy zablokuje, dokud se nedokončí volání REST základní služby. Dostupné jsou i asynchronní metody



## Vytvoření skupiny prostředků

Používáte-li Správce prostředků, pak musí být všechny prostředky vytvořeny ve skupině prostředků. Skupina prostředků je kontejner, který obsahuje související prostředky pro aplikaci. Při použití Azure SQL Database musí být server databáze vytvořený v existující skupině prostředků.

        static void CreateResourceGroup()
        {
            creds = new Microsoft.Rest.TokenCredentials(token.AccessToken);

            // Create a resource management client
            ResourceManagementClient resourceClient = new ResourceManagementClient(creds);

            // Resource group parameters
            ResourceGroup resourceGroupParameters = new ResourceGroup()
            {
                Location = location,
            };

            //Create a resource group
            resourceClient.SubscriptionId = subscriptionId;
            var resourceGroupResult = resourceClient.ResourceGroups.CreateOrUpdate(resourceGroupName, resourceGroupParameters);
        }


## Vytvoření serveru

Databáze SQL se ukládají v serverech. Název serveru musí být globálně jedinečný mezi všemi servery Azure SQL, proto se zobrazí chyba, když se název serveru už používá. Stojí také za zmínku, že dokončení tohoto příkazu může trvat i několik minut.

    static void CreateServer()
    {
        // Create a SQL Database management client
        SqlManagementClient sqlClient = new SqlManagementClient(new TokenCloudCredentials(subscriptionId, token.AccessToken));

        // Create a server
        ServerCreateOrUpdateParameters serverParameters = new ServerCreateOrUpdateParameters()
        {
            Location = location,
            Properties = new ServerCreateOrUpdateProperties()
            {
                AdministratorLogin = administratorLogin,
                AdministratorLoginPassword = administratorPassword,
                Version = serverVersion
            }
        };
            var serverResult = sqlClient.Servers.CreateOrUpdate(resourceGroupName, serverName, serverParameters);
    }


### Vytvoření externího správce serveru


    // Create a server external admin
    ServerAdministratorCreateOrUpdateParameters aadAdminParameters = new ServerAdministratorCreateOrUpdateParameters()
    {
        Properties = new ServerAdministratorCreateOrUpdateProperties()
        {
            AdministratorType = "ActiveDirectory",
            Login = "<login>",
            Sid = new Guid("<Azure AD admin sid>"),
            TenantId = new Guid("<Azure AD tenant id>")
        }
    };

    var adminResult = sqlClient.ServerAdministrators.CreateOrUpdate(resourceGroupName, serverName, "ActiveDirectory", aadAdminParameters);
    var adminGetResult = sqlClient.ServerAdministrators.Get(resourceGroupName, serverName, "ActiveDirectory");




## Vytvoření pravidla brány firewall pro povolení přístupu k serveru

Ve výchozím nastavení se k serveru nemůžete připojit odkudkoli. Aby bylo možné připojit se k serveru nebo kterékoli databázi na něm, je třeba vytvořit [pravidlo brány firewall](https://msdn.microsoft.com/library/azure/ee621782.aspx), které povoluje přístup z IP adresy klienta.

Následující příklad vytvoří pravidlo, které otevře přístup k serveru z libovolné IP adresy. Doporučuje se k databázi SQL vytvořit uživatelská jména a hesla a nespoléhat se při jejím zabezpečení jen na pravidlech brány firewall.


        static void CreateFirewallRule()
        {
            // Create a firewall rule on the server
            FirewallRuleCreateOrUpdateParameters firewallParameters = new FirewallRuleCreateOrUpdateParameters()
            {
                Properties = new FirewallRuleCreateOrUpdateProperties()
                {
                    StartIpAddress = "0.0.0.0",
                    EndIpAddress = "255.255.255.255"
                }
            };
            var firewallResult = sqlClient.FirewallRules.CreateOrUpdate(resourceGroupName, serverName, firewallRuleName, firewallParameters);
        }




Pokud chcete povolit jiné službě Azure přístup k serveru, přidejte pravidlo brány firewall a nastavte StartIpAddress i EndIpAddress na hodnotu 0.0.0.0. Pamatujte, že tím umožníte přístup k serveru z *jakéhokoli* předplatného Azure.


## Vytvoření nové databáze SQL pomocí C&#x23;

Následující příkaz jazyka C# vytvoří novou databázi SQL, pokud databáze s daným názvem na serveru již neexistuje. Pokud databáze se stejným názvem existuje, bude aktualizována.


        static void CreateDatabase()
        {
            // Create a database

            // Retrieve the server on which the database will be created
            Server currentServer = sqlClient.Servers.Get(resourceGroupName, serverName).Server;

            // Create a database: configure create or update parameters and properties explicitly
            DatabaseCreateOrUpdateParameters newDatabaseParameters = new DatabaseCreateOrUpdateParameters()
            {
                Location = currentServer.Location,
                Properties = new DatabaseCreateOrUpdateProperties()
                {
                    Edition = databaseEdition,
                    RequestedServiceObjectiveName = databasePerfLevel,
                }
            };
            var dbResponse = sqlClient.Databases.CreateOrUpdate(resourceGroupName, serverName, databaseName, newDatabaseParameters);
        }



## Ukázková konzolová aplikace C&#x23;

Následující příklad vytvoří skupinu prostředků, server, pravidlo brány firewall a databázi SQL. Oddíl *Konfigurace ověřování s Azure Active Directory* v horní části tohoto článku ukazuje, kde získat hodnoty clientId, redirectUri a domainName.


    using Microsoft.Azure;
    using Microsoft.Azure.Management.ResourceManager;
    using Microsoft.Azure.Management.ResourceManager.Models;
    using Microsoft.Azure.Management.Sql;
    using Microsoft.Azure.Management.Sql.Models;
    using Microsoft.IdentityModel.Clients.ActiveDirectory;
    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Text;
    using System.Threading.Tasks;

    namespace SqlDbConsoleApp
    {
    class Program
    {
        // Get these values from the Azure portal
        static string subscriptionId = "<Azure subscription id>";
        static string clientId = "<Azure App clientId>";
        static string redirectUri = "<Azure App redirectURI>";
        static string domainName = "<domain>";

        // You create these values
        static string resourceGroupName = "<your resource group name>";
        static string location = "<Azure data center location>";

        static string serverName = "<your server name>";
        static string administratorLogin = "<your server admin>";

        // store your password securely!
        static string administratorPassword = "<your server admin password>";
        static string serverVersion = "12.0";

        static string firewallRuleName = "<your firewall rule name>";

        static string databaseName = "dbfromcsharparticle";
        static string databaseEdition = "Basic"; // Basic, Standard, or Premium
        static string databasePerfLevel = "";

        static AuthenticationResult token;
        static Microsoft.Rest.TokenCredentials creds;

        static SqlManagementClient sqlClient;


        static void Main(string[] args)
        {
            Console.WriteLine("Logging in...");

            token = GetAccessToken();

            Console.WriteLine("Logged in as: " + token.UserInfo.DisplayableId);

            Console.WriteLine("Creating resource group...");
            CreateResourceGroup();

            sqlClient = new SqlManagementClient(new TokenCloudCredentials(subscriptionId, token.AccessToken));

            Console.WriteLine("Creating server...");
            CreateServer();

            Console.WriteLine("Creating firewall rule...");
            CreateFirewallRule();

            Console.WriteLine("Creating database...");

            DatabaseCreateOrUpdateResponse dbResponse = CreateDatabase();
            Console.WriteLine("Status: " + dbResponse.Status.ToString()
                + " Code: " + dbResponse.StatusCode.ToString());

            Console.WriteLine("Press enter to exit...");
            Console.ReadLine();
        }

        static void CreateResourceGroup()
        {
            creds = new Microsoft.Rest.TokenCredentials(token.AccessToken);

            // Create a resource management client
            ResourceManagementClient resourceClient = new ResourceManagementClient(creds);

            // Resource group parameters
            ResourceGroup resourceGroupParameters = new ResourceGroup()
            {
                Location = location,
            };

            //Create a resource group
            resourceClient.SubscriptionId = subscriptionId;
            var resourceGroupResult = resourceClient.ResourceGroups.CreateOrUpdate(resourceGroupName, resourceGroupParameters);
        }

        static void CreateServer()
        {
            // Create a SQL Database management client
            //SqlManagementClient sqlClient = new SqlManagementClient(new TokenCloudCredentials(subscriptionId, token.AccessToken));

            // Create a server
            ServerCreateOrUpdateParameters serverParameters = new ServerCreateOrUpdateParameters()
            {
                Location = location,
                Properties = new ServerCreateOrUpdateProperties()
                {
                    AdministratorLogin = administratorLogin,
                    AdministratorLoginPassword = administratorPassword,
                    Version = serverVersion
                }
            };
            var serverResult = sqlClient.Servers.CreateOrUpdate(resourceGroupName, serverName, serverParameters);
        }

        static void CreateFirewallRule()
        {
            // Create a firewall rule on the server
            FirewallRuleCreateOrUpdateParameters firewallParameters = new FirewallRuleCreateOrUpdateParameters()
            {
                Properties = new FirewallRuleCreateOrUpdateProperties()
                {
                    // replace with your client IP address
                    StartIpAddress = "0.0.0.0",
                    EndIpAddress = "255.255.255.255"
                }
            };
            var firewallResult = sqlClient.FirewallRules.CreateOrUpdate(resourceGroupName, serverName, firewallRuleName, firewallParameters);
        }

        static DatabaseCreateOrUpdateResponse CreateDatabase()
        {
            // Create a database

            // Retrieve the server on which the database will be created
            Server currentServer = sqlClient.Servers.Get(resourceGroupName, serverName).Server;

            // Create a database: configure create or update parameters and properties explicitly
            DatabaseCreateOrUpdateParameters newDatabaseParameters = new DatabaseCreateOrUpdateParameters()
            {
                Location = currentServer.Location,
                Properties = new DatabaseCreateOrUpdateProperties()
                {
                    Edition = databaseEdition,
                    RequestedServiceObjectiveName = databasePerfLevel,
                }
            };
            var dbResponse = sqlClient.Databases.CreateOrUpdate(resourceGroupName, serverName, databaseName, newDatabaseParameters);
            return dbResponse;
        }

        private static AuthenticationResult GetAccessToken()
        {
            AuthenticationContext authContext = new AuthenticationContext
                ("https://login.windows.net/" + domainName /* Tenant ID or AAD domain */);

            AuthenticationResult token = authContext.AcquireToken
                ("https://management.azure.com/"/* the Azure Resource Management endpoint */,
                    clientId,
            new Uri(redirectUri) /* redirect URI */,
            PromptBehavior.Auto /* with Auto user will not be prompted if an unexpired token is cached */);

            return token;
        }
    }
    }




## Další kroky
Nyní, když jste si vyzkoušeli SQL Database a nastavili databázi pomocí C#, jste připraveni na následující články:

- [Připojení k SQL Database přes SQL Server Management Studio a provedení ukázkového dotazu T-SQL](sql-database-connect-query-ssms.md)

## Další zdroje

- [SQL Database](https://azure.microsoft.com/documentation/services/sql-database/)





<!--Image references-->
[1]: ./media/sql-database-get-started-csharp/aad.png
[2]: ./media/sql-database-get-started-csharp/permissions.png
[3]: ./media/sql-database-get-started-csharp/getdomain.png
[4]: ./media/sql-database-get-started-csharp/aad2.png
[5]: ./media/sql-database-get-started-csharp/aad-applications.png
[6]: ./media/sql-database-get-started-csharp/add.png
[7]: ./media/sql-database-get-started-csharp/add-application.png
[8]: ./media/sql-database-get-started-csharp/add-application2.png
[9]: ./media/sql-database-get-started-csharp/clientid.png



<!--HONumber=Jun16_HO2-->


