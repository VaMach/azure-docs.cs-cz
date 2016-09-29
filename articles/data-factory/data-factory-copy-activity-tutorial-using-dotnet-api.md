<properties 
    pageTitle="Kurz: Vytvoření kanálu s aktivitou kopírování pomocí rozhraní .NET API | Microsoft Azure" 
    description="V tomto kurzu vytvoříte kanál Azure Data Factory s aktivitou kopírování pomocí rozhraní .NET API." 
    services="data-factory" 
    documentationCenter="" 
    authors="spelluru" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="get-started-article" 
    ms.date="09/16/2016" 
    ms.author="spelluru"/>


# Kurz: Vytvoření kanálu s aktivitou kopírování pomocí rozhraní .NET API
> [AZURE.SELECTOR]
- [Přehled a požadavky](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
- [portál Azure](data-factory-copy-activity-tutorial-using-azure-portal.md)
- [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md)
- [PowerShell](data-factory-copy-activity-tutorial-using-powershell.md)
- [REST API](data-factory-copy-activity-tutorial-using-rest-api.md)
- [.NET API](data-factory-copy-activity-tutorial-using-dotnet-api.md)
- [Průvodce kopírováním](data-factory-copy-data-wizard-tutorial.md)

V tomto kurzu je uvedeno, jak vytvořit a monitorovat objekt pro vytváření dat Azure pomocí rozhraní .NET API. Kanál v objektu pro vytváření dat využívá aktivitu kopírování, s jejíž pomocí kopíruje data ze služby Azure Blob Storage do služby Azure SQL Database.

Aktivita kopírování provádí přesun dat ve službě Azure Data Factory. Aktivita používá globálně dostupnou službu, která může kopírovat data mezi různými úložišti dat zabezpečeným, spolehlivým a škálovatelným způsobem. Podrobnosti o aktivitě kopírování najdete v článku [Aktivity přesunu dat](data-factory-data-movement-activities.md).   

> [AZURE.NOTE] 
> Tento článek nepopisuje všechny možnosti rozhraní .NET API služby Data Factory. Podrobné informace o sadě Data Factory .NET SDK najdete v [referencích k rozhraní .NET API služby Data Factory](https://msdn.microsoft.com/library/mt415893.aspx). 

## Požadavky
- Projděte si [Přehled a požadavky kurzu](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md), kde najdete informace o kurzu, a proveďte **nutné** kroky. 
- Visual Studio 2012 nebo 2013 nebo 2015
- Stáhněte sadu [Azure .NET SDK](http://azure.microsoft.com/downloads/) a nainstalujte ji.
- Azure Powershell Podle pokynů v článku [Instalace a konfigurace prostředí Azure PowerShell](../powershell-install-configure.md) si na počítač nainstalujte prostředí Azure PowerShell. K vytvoření aplikace v Azure Active Directory použijete Azure PowerShell.

### Vytvoření aplikace v Azure Active Directory
Vytvořte aplikaci Azure Active Directory, vytvořte pro ni instanční objekt a přiřaďte ho roli **Přispěvatel Data Factory**.  

1. Spusťte **PowerShell**. 
1. Spusťte následující příkaz a zadejte uživatelské jméno a heslo, které používáte k přihlášení na web Azure Portal.
    
        Login-AzureRmAccount   
2. Spuštěním následujícího příkazu zobrazíte všechna předplatná pro tento účet.

        Get-AzureRmSubscription 
3. Spuštěním následujícího příkazu vyberte předplatné, se kterým chcete pracovat. Místo **&lt;NameOfAzureSubscription**&gt; zadejte název svého předplatného Azure. 

        Get-AzureRmSubscription -SubscriptionName <NameOfAzureSubscription> | Set-AzureRmContext

    > [AZURE.IMPORTANT] Poznamenejte si **SubscriptionId** a **TenantId** z výstupu tohoto příkazu. 
4. Spuštěním následujícího příkazu v PowerShellu vytvořte skupinu prostředků Azure s názvem **ADFTutorialResourceGroup**.  

        New-AzureRmResourceGroup -Name ADFTutorialResourceGroup  -Location "West US"

    Pokud skupina prostředků už existuje, určete, jestli se má aktualizovat (Y), nebo ponechat tak, jak je (N). 

    Pokud používáte jinou skupinu prostředků, použijte v postupech v tomto kurzu místo skupiny ADFTutorialResourceGroup název vaší skupiny prostředků.
5. Vytvořte aplikaci Azure Active Directory. 

        $azureAdApplication = New-AzureRmADApplication -DisplayName "ADFCopyTutotiralApp" -HomePage "https://www.contoso.org" -IdentifierUris "https://www.adfcopytutorialapp.org/example" -Password "Pass@word1"

    Pokud se zobrazí následující chyba, zadejte jinou adresu URL a spusťte příkaz znovu. 

        Another object with the same value for property identifierUris already exists.

6. Vytvořte instanční objekt služby AD. 

        New-AzureRmADServicePrincipal -ApplicationId $azureAdApplication.ApplicationId

7. Přidejte instanční objekt k roli **Přispěvatel Data Factory**. 

        New-AzureRmRoleAssignment -RoleDefinitionName "Data Factory Contributor" -ServicePrincipalName $azureAdApplication.ApplicationId.Guid

8. Získejte ID aplikace.

        $azureAdApplication

    Poznamenejte si ID aplikace (**applicationID** ve výstupu).

Z těchto kroků byste měli mít tyto čtyři hodnoty: 

- ID tenanta
- ID předplatného
- ID aplikace 
- Heslo (zadané v prvním příkazu)   

## Názorný postup
1. Pomocí sady Visual Studio 2012/2013/2015 vytvořte konzolovou aplikaci C# .NET.
    1. Spusťte **Visual Studio** 2012/2013/2015.
    2. Klikněte na **Soubor**, přejděte na **Nový** a klikněte na **Projekt**.
    3. Rozbalte **Šablony** a vyberte **Visual C#**. V tomto názorném postupu použijete C#, ale mohli byste využít libovolný jazyk .NET.
    4. V seznamu typů projektů napravo vyberte **Konzolová aplikace**.
    5. Jako název zadejte **DataFactoryAPITestApp**.
    6. Jako umístění vyberte **C:\ADFGetStarted**.
    7. Projekt vytvoříte kliknutím na **OK**.
2. Klikněte na **Nástroje**, přejděte na **Správce balíčků NuGet** a klikněte na **Konzola Správce balíčků**.
3.  V **konzole Správce balíčků** spusťte následující příkazy jeden po druhém. 

        Install-Package Microsoft.Azure.Management.DataFactories
        Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -Version 2.19.208020213
6. Do souboru **App.config** přidejte následující část **appSetttings**. Tyto nastavení používá pomocná metoda: **GetAuthorizationHeader**. 

    Nahraďte hodnoty pro **&lt;ID aplikace&gt;**, **&lt;Heslo&gt;**, **&lt;ID předplatného&gt;** a **&lt;ID tenanta&gt;** vlastními hodnotami. 

        <appSettings>
            <add key="ActiveDirectoryEndpoint" value="https://login.windows.net/" />
            <add key="ResourceManagerEndpoint" value="https://management.azure.com/" />
            <add key="WindowsManagementUri" value="https://management.core.windows.net/" />

            <!-- Replace the following values with your own -->
            <add key="ApplicationId" value="<Application ID>" />
            <add key="Password" value="<Password>" />    
            <add key="SubscriptionId" value= "Subscription ID" />
            <add key="ActiveDirectoryTenantId" value="tenant ID" />
        </appSettings>
6. Do zdrojového souboru (Program.cs) v projektu přidejte následující příkazy **using**.

        using System.Threading;
        using System.Configuration;
        using System.Collections.ObjectModel;
        
        using Microsoft.Azure.Management.DataFactories;
        using Microsoft.Azure.Management.DataFactories.Models;
        using Microsoft.Azure.Management.DataFactories.Common.Models;
        
        using Microsoft.IdentityModel.Clients.ActiveDirectory;
        using Microsoft.Azure;
6. Do metody **Main** přidejte následující kód, který vytvoří instanci třídy **DataPipelineManagementClient**. Tento objekt použijete k vytvoření objektu pro vytváření dat, propojené služby, vstupních a výstupních datových sad a kanálu. Použijete ho také k monitorování řezů datových sad při spuštění.    

            // create data factory management client
            string resourceGroupName = "ADFTutorialResourceGroup";
            string dataFactoryName = "APITutorialFactory";

            TokenCloudCredentials aadTokenCredentials =
                new TokenCloudCredentials(
                    ConfigurationManager.AppSettings["SubscriptionId"],
                    GetAuthorizationHeader());

            Uri resourceManagerUri = new Uri(ConfigurationManager.AppSettings["ResourceManagerEndpoint"]);

            DataFactoryManagementClient client = new DataFactoryManagementClient(aadTokenCredentials, resourceManagerUri);

    > [AZURE.IMPORTANT] 
    > Hodnotu **resourceGroupName** nahraďte názvem skupiny prostředků Azure. 
    > 
    > Aktualizujte název objektu pro vytváření dat (**dataFactoryName**) tak, aby byl jedinečný. Název objektu pro vytváření dat musí být globálně jedinečný. V tématu [Objekty pro vytváření dat – pravidla pojmenování](data-factory-naming-rules.md) najdete pravidla pojmenování artefaktů služby Data Factory. 

7. Do metody **Main** přidejte následující kód, který vytvoří **objekt pro vytváření dat**.

            // create a data factory
            Console.WriteLine("Creating a data factory");
            client.DataFactories.CreateOrUpdate(resourceGroupName,
                new DataFactoryCreateOrUpdateParameters()
                {
                    DataFactory = new DataFactory()
                    {
                        Name = dataFactoryName,
                        Location = "westus",
                        Properties = new DataFactoryProperties() { }
                    }
                }
            );

8. Do metody **Main** přidejte následující kód, který vytvoří **propojenou službu Azure Storage**. 

    > [AZURE.IMPORTANT] Položky **storageaccountname** a **accountkey** nahraďte názvem svého účtu Azure Storage a jeho klíčem. 

            // create a linked service for input data store: Azure Storage
            Console.WriteLine("Creating Azure Storage linked service");
            client.LinkedServices.CreateOrUpdate(resourceGroupName, dataFactoryName,
                new LinkedServiceCreateOrUpdateParameters()
                {
                    LinkedService = new LinkedService()
                    {
                        Name = "AzureStorageLinkedService",
                        Properties = new LinkedServiceProperties
                        (
                            new AzureStorageLinkedService("DefaultEndpointsProtocol=https;AccountName=<storageaccountname>;AccountKey=<accountkey>")
                        )
                    }
                }
            );
9. Do metody **Main** přidejte následující kód, který vytvoří **propojenou službu Azure SQL**.
 
    > [AZURE.IMPORTANT] Položky **servername**, **databasename**, **username** a **password** nahraďte názvem serveru SQL Azure, názvem databáze, uživatelským jménem a heslem.  

            // create a linked service for output data store: Azure SQL Database
            Console.WriteLine("Creating Azure SQL Database linked service");
            client.LinkedServices.CreateOrUpdate(resourceGroupName, dataFactoryName,
                new LinkedServiceCreateOrUpdateParameters()
                {
                    LinkedService = new LinkedService()
                    {
                        Name = "AzureSqlLinkedService",
                        Properties = new LinkedServiceProperties
                        (
                            new AzureSqlDatabaseLinkedService("Data Source=tcp:<servername>.database.windows.net,1433;Initial Catalog=<databasename>;User ID=<username>;Password=<password>;Integrated Security=False;Encrypt=True;Connect Timeout=30")
                        )
                    }
                }
            );
10. Do metody **Main** přidejte následující kód, který vytvoří **vstupní a výstupní datové sady**. 

            // create input and output datasets
            Console.WriteLine("Creating input and output datasets");
            string Dataset_Source = "DatasetBlobSource";
            string Dataset_Destination = "DatasetAzureSqlDestination";

            Console.WriteLine("Creating input dataset of type: Azure Blob");
            client.Datasets.CreateOrUpdate(resourceGroupName, dataFactoryName,


            new DatasetCreateOrUpdateParameters()
            {
                Dataset = new Dataset()
                {
                    Name = Dataset_Source,
                    Properties = new DatasetProperties()
                    {
                        Structure = new List<DataElement>()
                        {
                            new DataElement() { Name = "FirstName", Type = "String" },
                            new DataElement() { Name = "LastName", Type = "String" }
                        },
                        LinkedServiceName = "AzureStorageLinkedService",
                        TypeProperties = new AzureBlobDataset()
                        {
                            FolderPath = "adftutorial/",
                            FileName = "emp.txt"
                        },
                        External = true,
                        Availability = new Availability()
                        {
                            Frequency = SchedulePeriod.Hour,
                            Interval = 1,
                        },

                        Policy = new Policy()
                        {
                            Validation = new ValidationPolicy()
                            {
                                MinimumRows = 1
                            }
                        }
                    }
                }
            });


            Console.WriteLine("Creating output dataset of type: Azure SQL");
            client.Datasets.CreateOrUpdate(resourceGroupName, dataFactoryName,
                new DatasetCreateOrUpdateParameters()
                {
                    Dataset = new Dataset()
                    {
                        Name = Dataset_Destination,
                        Properties = new DatasetProperties()
                        {
                            Structure = new List<DataElement>()
                            {
                                new DataElement() { Name = "FirstName", Type = "String" },
                                new DataElement() { Name = "LastName", Type = "String" }
                            },
                            LinkedServiceName = "AzureSqlLinkedService",
                            TypeProperties = new AzureSqlTableDataset()
                            {
                                TableName = "emp"
                            },

                            Availability = new Availability()
                            {
                                Frequency = SchedulePeriod.Hour,
                                Interval = 1,
                            },
                        }
                    }
                });

11. Do metody **Main** přidejte následující kód, který **vytvoří a aktivuje kanál**. Tento kanál má aktivitu **CopyActivity**, která jako zdroj používá **BlobSource** a jako jímku používá **BlobSink**.

            // create a pipeline
            Console.WriteLine("Creating a pipeline");
            DateTime PipelineActivePeriodStartTime = new DateTime(2016, 8, 9, 0, 0, 0, 0, DateTimeKind.Utc);
            DateTime PipelineActivePeriodEndTime = PipelineActivePeriodStartTime.AddMinutes(60);
            string PipelineName = "ADFTutorialPipeline";

            client.Pipelines.CreateOrUpdate(resourceGroupName, dataFactoryName,
                new PipelineCreateOrUpdateParameters()
                {
                    Pipeline = new Pipeline()
                    {
                        Name = PipelineName,
                        Properties = new PipelineProperties()
                        {
                            Description = "Demo Pipeline for data transfer between blobs",

                    // Initial value for pipeline's active period. With this, you won't need to set slice status
                    Start = PipelineActivePeriodStartTime,
                            End = PipelineActivePeriodEndTime,

                            Activities = new List<Activity>()
                            {
                                new Activity()
                                {
                                    Name = "BlobToAzureSql",
                                    Inputs = new List<ActivityInput>()
                                    {
                                        new ActivityInput() {
                                            Name = Dataset_Source
                                        }
                                    },
                                    Outputs = new List<ActivityOutput>()
                                    {
                                        new ActivityOutput()
                                        {
                                            Name = Dataset_Destination
                                        }
                                    },
                                    TypeProperties = new CopyActivity()
                                    {
                                        Source = new BlobSource(),
                                        Sink = new BlobSink()
                                        {
                                            WriteBatchSize = 10000,
                                            WriteBatchTimeout = TimeSpan.FromMinutes(10)
                                        }
                                    }
                                }
                            },
                        }
                    }
                }); 

12. Do metody **Main** přidejte následující kód pro získání stavu datového řezu výstupní datové sady. V této ukázce se očekává jenom jeden řez.   
 
            // Pulling status within a timeout threshold
            DateTime start = DateTime.Now;
            bool done = false;

            while (DateTime.Now - start < TimeSpan.FromMinutes(5) && !done)
            {
                Console.WriteLine("Pulling the slice status");
                // wait before the next status check
                Thread.Sleep(1000 * 12);

                var datalistResponse = client.DataSlices.List(resourceGroupName, dataFactoryName, Dataset_Destination,
                    new DataSliceListParameters()
                    {
                        DataSliceRangeStartTime = PipelineActivePeriodStartTime.ConvertToISO8601DateTimeString(),
                        DataSliceRangeEndTime = PipelineActivePeriodEndTime.ConvertToISO8601DateTimeString()
                    });

                foreach (DataSlice slice in datalistResponse.DataSlices)
                {
                    if (slice.State == DataSliceState.Failed || slice.State == DataSliceState.Ready)
                    {
                        Console.WriteLine("Slice execution is done with status: {0}", slice.State);
                        done = true;
                        break;
                    }
                    else
                    {
                        Console.WriteLine("Slice status is: {0}", slice.State);
                    }
                }
            }

14. Do metody **Main** přidejte následující kód pro získání běhových dat pro datový řez.

            Console.WriteLine("Getting run details of a data slice");

            // give it a few minutes for the output slice to be ready
            Console.WriteLine("\nGive it a few minutes for the output slice to be ready and press any key.");
            Console.ReadKey();

            var datasliceRunListResponse = client.DataSliceRuns.List(
                    resourceGroupName,
                    dataFactoryName,
                    Dataset_Destination,
                    new DataSliceRunListParameters()
                    {
                        DataSliceStartTime = PipelineActivePeriodStartTime.ConvertToISO8601DateTimeString()
                    }
                );

            foreach (DataSliceRun run in datasliceRunListResponse.DataSliceRuns)
            {
                Console.WriteLine("Status: \t\t{0}", run.Status);
                Console.WriteLine("DataSliceStart: \t{0}", run.DataSliceStart);
                Console.WriteLine("DataSliceEnd: \t\t{0}", run.DataSliceEnd);
                Console.WriteLine("ActivityId: \t\t{0}", run.ActivityName);
                Console.WriteLine("ProcessingStartTime: \t{0}", run.ProcessingStartTime);
                Console.WriteLine("ProcessingEndTime: \t{0}", run.ProcessingEndTime);
                Console.WriteLine("ErrorMessage: \t{0}", run.ErrorMessage);
            }

            Console.WriteLine("\nPress any key to exit.");
            Console.ReadKey();

13. Do třídy **Program** přidejte následující pomocnou metodu, kterou používá metoda **Main**.  
 
        public static string GetAuthorizationHeader()
        {
            AuthenticationResult result = null;
            var thread = new Thread(() =>
            {
                try
                {
                    var context = new AuthenticationContext(ConfigurationManager.AppSettings["ActiveDirectoryEndpoint"] + ConfigurationManager.AppSettings["ActiveDirectoryTenantId"]);

                    ClientCredential credential = new ClientCredential(ConfigurationManager.AppSettings["ApplicationId"], ConfigurationManager.AppSettings["Password"]);
                    result = context.AcquireToken(resource: ConfigurationManager.AppSettings["WindowsManagementUri"], clientCredential: credential);
                }
                catch (Exception threadEx)
                {
                    Console.WriteLine(threadEx.Message);
                }
            });

            thread.SetApartmentState(ApartmentState.STA);
            thread.Name = "AcquireTokenThread";
            thread.Start();
            thread.Join();

            if (result != null)
            {
                return result.AccessToken;
            }

            throw new InvalidOperationException("Failed to acquire token");
        }  


15. V Průzkumníku řešení rozbalte projekt (**DataFactoryAPITestApp**), klikněte pravým tlačítkem na **Odkazy** a potom klikněte na **Přidat odkaz**. Zaškrtněte políčko pro sestavení **System.Configuration** a klikněte na **OK**. 
16. Sestavte konzolovou aplikaci. Klikněte v nabídce na **Sestavit** a potom klikněte na **Sestavit řešení**. 
16. Potvrďte, že kontejner **adftutorial** v Azure Blob Storage obsahuje alespoň jeden soubor. Pokud ne, vytvořte v Poznámkovém bloku soubor **Emp.txt** s následujícím obsahem a nahrajte ho do kontejneru adftutorial.

        John, Doe
        Jane, Doe
     
17. Ukázku spusťte kliknutím na **Ladit** -> **Spustit ladění** v nabídce. Když se zobrazí **Získávání běhových podrobností o datovém řezu**, počkejte několik minut a stiskněte **ENTER**. 
18. Pomocí webu Azure Portal ověřte, že je objekt pro vytváření dat **APITutorialFactory** vytvořený s těmito artefakty: 
    - Propojená služba: **LinkedService_AzureStorage** 
    - Datová sada: **DatasetBlobSource** a **DatasetBlobDestination**.
    - Kanál: **PipelineBlobSample** 
18. Ověřte, že se výstupní soubor vytvořil ve složce **apifactoryoutput** v kontejneru **adftutorial**.

## Další kroky

- Pročtěte si článek [Aktivity přesunu dat](data-factory-data-movement-activities.md), který obsahuje podrobné informace o aktivitě kopírování použité v tomto kurzu.
- Podrobné informace o sadě Data Factory .NET SDK najdete v [referencích k rozhraní .NET API služby Data Factory](https://msdn.microsoft.com/library/mt415893.aspx). Tento článek nepopisuje všechny možnosti rozhraní .NET API služby Data Factory. 

 



<!--HONumber=Sep16_HO3-->


