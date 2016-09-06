<properties
    pageTitle="Vytvoření fondu elastické databáze s C# | Microsoft Azure"
    description="Pomocí technik vývoje databází v C# můžete v Azure SQL Database vytvořit škálovatelný fond elastické databáze umožňující sdílení prostředků mezi mnoha databázemi."
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="csharp"
    ms.workload="data-management"
    ms.date="08/18/2016"
    ms.author="sstein"/>

# Vytvoření nového fondu elastické databáze s C&#x23;

> [AZURE.SELECTOR]
- [portál Azure](sql-database-elastic-pool-create-portal.md)
- [PowerShell](sql-database-elastic-pool-create-powershell.md)
- [C#](sql-database-elastic-pool-create-csharp.md)


Naučte se vytvořit [fond elastické databáze](sql-database-elastic-pool.md) pomocí C#.

Běžné kódy chyb naleznete v článku [Kódy chyb SQL pro klientské aplikace SQL Database: chyby připojení k databázi a další problémy](sql-database-develop-error-messages.md).

V příkladech se používá [Knihovna SQL Database pro .NET](https://msdn.microsoft.com/library/azure/mt349017.aspx). Pokud tedy není nainstalovaná, před dalším pokračováním ji nainstalujte. Tuto knihovnu nainstalujete spuštěním následujícího příkazu v [konzole správce balíčků](http://docs.nuget.org/Consume/Package-Manager-Console) sady Visual Studio (**Nástroje** > **Správce balíčků NuGet** > **Konzola Správce balíčků**):

    Install-Package Microsoft.Azure.Management.Sql –Pre

## Vytvoření fondu

Vytvořte instanci [SqlManagementClient](https://msdn.microsoft.com/library/microsoft.azure.management.sql.sqlmanagementclient) pomocí hodnot z [Azure Active Directory](sql-database-client-id-keys.md). Vytvořte instanci [ElasticPoolCreateOrUpdateParameters](https://msdn.microsoft.com/library/microsoft.azure.management.sql.models.elasticpoolcreateorupdateparameters) a zavolejte metodu [CreateOrUpdate](https://msdn.microsoft.com/library/microsoft.azure.management.sql.databaseoperationsextensions.createorupdate). Hodnoty eDTU na fond a minimální a maximální DTU jsou omezeny úrovní služeb (Basic, Standard nebo Premium). Viz odstavec [Omezení eDTU a úložiště pro elastické fondy a elastické databáze](sql-database-elastic-pool.md#eDTU-and-storage-limits-for-elastic-pools-and-elastic-databases).


    ElasticPoolCreateOrUpdateParameters newPoolParameters = new ElasticPoolCreateOrUpdateParameters()
    {
        Location = "South Central US",
        Properties = new ElasticPoolCreateOrUpdateProperties()
        {
            Edition = "Standard",
            Dtu = 400,
            DatabaseDtuMin = 0,
            DatabaseDtuMax = 100
         }
    };

    // Create the pool
    var newPoolResponse = sqlClient.ElasticPools.CreateOrUpdate("resourcegroup-name", "server-name", "ElasticPool1", newPoolParameters);

## Vytvoření databáze ve fondu

Vytvořte instanci [DataBaseCreateOrUpdateProperties](https://msdn.microsoft.com/library/microsoft.azure.management.sql.models.databasecreateorupdateproperties) a nastavte vlastnosti nové databáze. Poté zavolejte metodu CreateOrUpdate se skupinou prostředků, názvem serveru a názvem nové databáze.

    // Create a database: configure create or update parameters and properties explicitly
    DatabaseCreateOrUpdateParameters newPooledDatabaseParameters = new DatabaseCreateOrUpdateParameters()
    {
        Location = currentServer.Location,
        Properties = new DatabaseCreateOrUpdateProperties()
        {
            Edition = "Standard",
            RequestedServiceObjectiveName = "ElasticPool",
            ElasticPoolName = "ElasticPool1",
            MaxSizeBytes = 268435456000, // 250 GB,
            Collation = "SQL_Latin1_General_CP1_CI_AS"
        }
    };

    var poolDbResponse = sqlClient.Databases.CreateOrUpdate("resourcegroup-name", "server-name", "Database2", newPooledDatabaseParameters);

Chcete-li do fondu přesunout existující databázi, přečtěte si část [Přesun databáze do pružného fondu](sql-database-elastic-pool-manage-csharp.md#Move-a-database-into-an-elastic-pool).

## Příklad: Vytvoření fondu pomocí C&#x23;

Tento příklad vytvoří skupinu prostředků Azure, server Azure SQL a elastický fond. 
 

Pro spuštění tohoto příkladu potřebujete následující knihovny: Instalaci můžete provést spuštěním následujících příkazů v [konzole správce balíčků](http://docs.nuget.org/Consume/Package-Manager-Console) sady Visual Studio (**Nástroje** > **Správce balíčků NuGet** > **Konzola Správce balíčků**).

    Install-Package Microsoft.Azure.Management.Sql –Pre
    Install-Package Microsoft.Azure.Management.ResourceManager –Pre
    Install-Package Microsoft.Azure.Common.Authentication –Pre

Vytvořte aplikaci konzoly a nahraďte obsah souboru Program.cs následujícím kódem. Chcete-li získat požadované ID klienta a související hodnoty, vytvořte nativní aplikaci podle následujícího článku: [Registrace vaší aplikace a získání požadovaných hodnot klienta a klíče pro připojení aplikace k SQL Database](sql-database-client-id-keys.md).

    using Microsoft.Azure;
    using Microsoft.Azure.Management.ResourceManager;
    using Microsoft.Azure.Management.ResourceManager.Models;
    using Microsoft.Azure.Management.Sql;
    using Microsoft.Azure.Management.Sql.Models;
    using Microsoft.IdentityModel.Clients.ActiveDirectory;
    using System;
    
    namespace SqlDbElasticPoolsSample
    {
    class Program
    {
        // authentication variables
        static string subscriptionId = "<your Azure subscription>";
        static string clientId = "<your client id>";
        static string redirectUri = "<your redirect URI>";
        static string domainName = "<i.e. microsoft.onmicrosoft.com>";
        static AuthenticationResult token;

        // resource group variables
        static string datacenterLocation = "<location>";
        static string resourceGroupName = "<resource group name>";

        // server variables
        static string serverName = "<server name>";
        static string adminLogin = "<server admin>";
        static string adminPassword = "<server password (store it securely!)>";
        static string serverVersion = "12.0";

        // pool variables
        static string elasticPoolName = "<pool name>";
        static string poolEdition = "Standard";
        static int poolDtus = 400;
        static int databaseMinDtus = 0;
        static int databaseMaxDtus = 100;


        static void Main(string[] args)
        {
            // Sign in to Azure
            token = GetAccessToken();
            Console.WriteLine("Logged in as: " + token.UserInfo.DisplayableId);

            // Create a resource group
            ResourceGroup rg = CreateResourceGroup();
            Console.WriteLine(rg.Name + " created.");

            // Create a server
            Console.WriteLine("Creating server... ");
            ServerGetResponse srvr = CreateServer();
            Console.WriteLine("Creation of server " + srvr.Server.Name + ": " + srvr.StatusCode.ToString());

            // Create a pool
            Console.WriteLine("Creating elastic database pool... ");
            ElasticPoolCreateOrUpdateResponse epool = CreateElasticDatabasePool();
            Console.WriteLine("Creation of pool " + epool.ElasticPool.Name + ": " + epool.Status.ToString());


            // keep the console open until Enter is pressed!
            Console.WriteLine("Press Enter to exit.");
            Console.ReadLine();
        }


        static ElasticPoolCreateOrUpdateResponse CreateElasticDatabasePool()
        {
            // Create a SQL Database management client
            SqlManagementClient sqlClient = new SqlManagementClient(new TokenCloudCredentials(subscriptionId, token.AccessToken));

            // Create elastic pool: configure create or update parameters and properties explicitly
            ElasticPoolCreateOrUpdateParameters newPoolParameters = new ElasticPoolCreateOrUpdateParameters()
            {
                Location = datacenterLocation,
                Properties = new ElasticPoolCreateOrUpdateProperties()
                {
                    Edition = poolEdition,
                    Dtu = poolDtus, 
                    DatabaseDtuMin = databaseMinDtus,
                    DatabaseDtuMax = databaseMaxDtus
                }
            };

            // Create the pool
            var newPoolResponse = sqlClient.ElasticPools.CreateOrUpdate(resourceGroupName, serverName, elasticPoolName, newPoolParameters);
            return newPoolResponse;
        }



        static ServerGetResponse CreateServer()
        {

            // Create a SQL Database management client
            SqlManagementClient sqlClient = new SqlManagementClient(new TokenCloudCredentials(subscriptionId, token.AccessToken));

            // Create a server
            ServerCreateOrUpdateParameters serverParameters = new ServerCreateOrUpdateParameters()
            {
                Location = datacenterLocation,
                Properties = new ServerCreateOrUpdateProperties()
                {
                    AdministratorLogin = adminLogin,
                    AdministratorLoginPassword = adminPassword,
                    Version = serverVersion
                }
            };

            var serverResult = sqlClient.Servers.CreateOrUpdate(resourceGroupName, serverName, serverParameters);
            return serverResult;

        }


        static ResourceGroup CreateResourceGroup()
        {
           
            Microsoft.Rest.TokenCredentials creds = new Microsoft.Rest.TokenCredentials(token.AccessToken);
            // Create a resource management client 
            ResourceManagementClient resourceClient = new ResourceManagementClient(creds);

            // Resource group parameters
            ResourceGroup resourceGroupParameters = new ResourceGroup()
            {
                Location = datacenterLocation,
            };

            //Create a resource group
            resourceClient.SubscriptionId = subscriptionId;
            var resourceGroupResult = resourceClient.ResourceGroups.CreateOrUpdate(resourceGroupName, resourceGroupParameters);
            return resourceGroupResult;
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

- [Správa fondu](sql-database-elastic-pool-manage-csharp.md)
- [Vytvoření elastických úloh](sql-database-elastic-jobs-overview.md): Elastické úlohy umožňují spouštění skriptů T-SQL pro libovolný počet databází ve fondu.
- [Horizontální navýšení kapacity s Azure SQL Database](sql-database-elastic-scale-introduction.md): Používejte nástroje elastické databáze k horizontálnímu navýšení kapacity.

## Další zdroje

- [SQL Database](https://azure.microsoft.com/documentation/services/sql-database/)
- [Rozhraní API pro správu prostředků Azure](https://msdn.microsoft.com/library/azure/dn948464.aspx)



<!--HONumber=ago16_HO5-->


