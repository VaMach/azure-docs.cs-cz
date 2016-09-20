<properties
   pageTitle="Použití sady .NET SDK pro Data Lake Store k vývoji aplikací | Azure"
   description="Použití sady .NET SDK pro Azure Data Lake Store k vývoji aplikací"
   services="data-lake-store"
   documentationCenter=""
   authors="nitinme"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="09/13/2016"
   ms.author="nitinme"/>

# Začínáme s Azure Data Lake Store pomocí sady .NET SDK

> [AZURE.SELECTOR]
- [Portál](data-lake-store-get-started-portal.md)
- [PowerShell](data-lake-store-get-started-powershell.md)
- [.NET SDK](data-lake-store-get-started-net-sdk.md)
- [Java SDK](data-lake-store-get-started-java-sdk.md)
- [REST API](data-lake-store-get-started-rest-api.md)
- [Azure CLI](data-lake-store-get-started-cli.md)
- [Node.js](data-lake-store-manage-use-nodejs.md)

Naučte se používat sadu [.NET SDK pro Azure Data Lake Store](https://msdn.microsoft.com/library/mt581387.aspx) k vytvoření účtu Azure Data Lake a provádění základních operací, jako je vytváření složek, nahrávání a stahování datových souborů, odstranění účtu atd. Další informace týkající se Data Lake najdete v tématu [Azure Data Lake Store](data-lake-store-overview.md).

## Požadavky

* Visual Studio 2013 nebo 2015. Níže uvedené pokyny používají Visual Studio 2015.
* **Předplatné Azure**. Viz [Získání bezplatné zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/).
* **Vytvoření aplikace Azure Active Directory**. Ověřování pomocí služby Azure Active Directory můžete provádět dvěma způsoby – **interaktivně** a **neinteraktivně**. Předpoklady se liší podle toho, jak chcete ověřovat.
    * **V případě interaktivního ověřování** (používá se v tomto článku) – Ve službě Azure Active Directory je nutné vytvořit **nativní klientskou aplikaci**. Po vytvoření aplikace načtěte následující hodnoty týkající se aplikace.
        - Získejte **ID klienta** a **identifikátor URI přesměrování** aplikace.
        - Nastavte delegovaná oprávnění.

    * **V případě neinteraktivního ověřování** – Ve službě Azure Active Directory je nutné vytvořit **webovou aplikaci**. Po vytvoření aplikace načtěte následující hodnoty týkající se aplikace.
        - Získejte **ID klienta**, **tajný klíč klienta** a **identifikátor URI přesměrování** aplikace.
        - Nastavte delegovaná oprávnění.
        - Přiřaďte aplikaci Azure Active Directory k roli. Tato role může být na úrovni rozsahu, ve kterém chcete aplikaci Azure Active Directory udělit oprávnění. Aplikaci můžete přiřadit například na úrovni předplatného nebo na úrovni skupiny prostředků. 

    Pokyny týkající se načtení těchto hodnot, nastavení oprávnění a přiřazování rolí najdete v tématu [Vytvoření aplikace Active Directory a objektu zabezpečení pomocí portálu](../resource-group-create-service-principal-portal.md).

## Vytvoření aplikace .NET

1. Otevřete Visual Studio a vytvořte konzolovou aplikaci.

2. V nabídce **Soubor** klikněte na položku **Nový** a potom klikněte na položku **Projekt**.

3. V části **Nový projekt** zadejte nebo vyberte tyto hodnoty:

  	| Vlastnost | Hodnota                       |
  	|----------|-----------------------------|
  	| Kategorie | Šablony/Visual C#/Windows |
  	| Šablona | Konzolová aplikace         |
  	| Name (Název)     | VytvořeníAplikaceADL        |

4. Kliknutím na tlačítko **OK** vytvořte projekt.

5. Přidejte do projektu balíčky Nuget.

    1. V Průzkumníku řešení klikněte pravým tlačítkem na název projektu a klikněte na možnost **Správa balíčků NuGet**.
    2. Ujistěte se, že na kartě **Správce balíčků Nuget** je položka **Zdroj balíčku** nastavena na možnost **nuget.org** a je zaškrtnuto políčko **Zahrnout předběžné verze**.
    3. Najděte a nainstalujte následující balíčky Data Lake Store:

        * `Microsoft.Azure.Management.DataLake.Store`
        * `Microsoft.Azure.Management.DataLake.StoreUploader`

        ![Přidání zdroje Nuget](./media/data-lake-store-get-started-net-sdk/ADL.Install.Nuget.Package.png "Create a new Azure Data Lake account")

    4. Nainstalujte taky balíček `Microsoft.IdentityModel.Clients.ActiveDirectory` pro ověřování Azure Active Directory. Nezapomeňte *zrušit zaškrtnutí* políčka **Zahrnout předběžné verze**, abyste nainstalovali stabilní verzi tohoto balíčku.

        ![Přidání zdroje Nuget](./media/data-lake-store-get-started-net-sdk/adl.install.azure.auth.png "Create a new Azure Data Lake account")


    5. Zavřete **Správce balíčků Nuget**.

7. Otevřete soubor **Program.cs**, odstraňte stávající kód a potom vložte následující příkazy, čímž přidáte odkazy na obory názvů.

        using System;
        using System.IO;
        using System.Security;
        using System.Text;
        using System.Collections.Generic;
        using System.Linq;

        using Microsoft.Azure.Management.DataLake.Store;
        using Microsoft.Azure.Management.DataLake.Store.Models;
        using Microsoft.Azure.Management.DataLake.StoreUploader;
        using Microsoft.IdentityModel.Clients.ActiveDirectory;
        using Microsoft.Rest;

8. Deklarujte proměnné, jak vidíte níže, a zadejte hodnotu názvu Data Lake Store a názvu skupiny prostředků. Aplikace vytvoří vámi zadaný název Data Lake Store. Skupina prostředků, kterou tady zadáte, by už měla existovat. Taky se ujistěte, že místní cesta a název souboru, které tady zadáte, existují na počítači. Za deklarace oboru názvů přidejte následující fragment kódu.

        namespace SdkSample
        {
            class Program
            {
                private static DataLakeStoreAccountManagementClient _adlsClient;
                private static DataLakeStoreFileSystemManagementClient _adlsFileSystemClient;

                private static string _adlsAccountName;
                private static string _resourceGroupName;
                private static string _location;

                private static void Main(string[] args)
                {
                    _adlsAccountName = "<DATA-LAKE-STORE-NAME>"; // TODO: Replace this value with the name for a NEW Store account.
                    _resourceGroupName = "<RESOURCE-GROUP-NAME>"; // TODO: Replace this value. This resource group should already exist.
                    _location = "East US 2";

                    string localFolderPath = @"C:\local_path\"; // TODO: Make sure this exists and can be overwritten.
                    string localFilePath = localFolderPath + "file.txt"; // TODO: Make sure this exists and can be overwritten.
                    string remoteFolderPath = "/data_lake_path/";
                    string remoteFilePath = remoteFolderPath + "file.txt";
                }
            }
        }

Ve zbývající části tohoto článku vidíte, jak používat dostupné metody rozhraní .NET k provádění operací, jako je ověřování uživatelů, vytvoření účtu Data Lake Store, nahrávání souborů atd. Pokud hledáte ucelenou ukázku práce s Data Lake Store, přejděte k části [Příloha](#appendix-sample-code) dále v tomto článku.

## Ověření uživatele

Ověřování pomocí služby Azure Active Directory se dá provádět dvěma způsoby:

* **Interaktivně**, kdy se uživatel přihlašuje pomocí aplikace. Tato možnost je implementovaná v metodě `AuthenticateUser` ve fragmentu kódu zobrazeném níže.

* **Neinteraktivně**, kdy aplikace poskytuje svoje vlastní přihlašovací údaje. Tato možnost je implementovaná v metodě `AuthenticateAppliaction` ve fragmentu kódu zobrazeném níže.

### Interaktivní ověřování

Následující fragment kódu ukazuje metodu `AuthenticateUser`, kterou můžete použít k interaktivnímu přihlašování.

    // Authenticate the user with AAD through an interactive popup.
    // You need to have an application registered with AAD in order to authenticate.
    //   For more information and instructions on how to register your application with AAD, see:
    //   https://azure.microsoft.com/en-us/documentation/articles/resource-group-create-service-principal-portal/
    public static TokenCredentials AuthenticateUser(string tenantId, string resource, string appClientId, Uri appRedirectUri, string userId = "")
    {
        var authContext = new AuthenticationContext("https://login.microsoftonline.com/" + tenantId);
    
        var tokenAuthResult = authContext.AcquireToken(resource, appClientId, appRedirectUri,
            PromptBehavior.Auto, UserIdentifier.AnyUser);
    
        return new TokenCredentials(tokenAuthResult.AccessToken);
    }

### Neinteraktivní ověřování

Následující fragment kódu ukazuje metodu `AuthenticateApplication`, kterou můžete použít k neinteraktivnímu přihlašování.

    // Authenticate the application with AAD through the application's secret key.
    // You need to have an application registered with AAD in order to authenticate.
    //   For more information and instructions on how to register your application with AAD, see:
    //   https://azure.microsoft.com/en-us/documentation/articles/resource-group-create-service-principal-portal/
    public static TokenCredentials AuthenticateApplication(string tenantId, string resource, string appClientId, Uri appRedirectUri, SecureString clientSecret)
    {
        var authContext = new AuthenticationContext("https://login.microsoftonline.com/" + tenantId);
        var credential = new ClientCredential(appClientId, clientSecret);
    
        var tokenAuthResult = authContext.AcquireToken(resource, credential);
    
        return new TokenCredentials(tokenAuthResult.AccessToken);
    }
    
## Vytvoření účtu Data Lake Store

Následující fragment kódu ukazuje metodu `CreateAccount`, kterou můžete použít k vytvoření účtu Data Lake Store.

    // Create Data Lake Store account
    public static void CreateAccount()
    {
        var adlsParameters = new DataLakeStoreAccount(location: _location);
        _adlsClient.Account.Create(_resourceGroupName, _adlsAccountName, adlsParameters);
    } 

## Zobrazení seznamu všech účtů Data Lake Store v rámci předplatného

Následující fragment kódu ukazuje metodu `ListAdlStoreAccounts`, kterou můžete použít k zobrazení seznamu všech účtů Data Lake Store v rámci konkrétního předplatného Azure.

    // List all ADLS accounts within the subscription
    public static List<DataLakeStoreAccount> ListAdlStoreAccounts()
    {
        var response = _adlsClient.Account.List(_adlsAccountName);
        var accounts = new List<DataLakeStoreAccount>(response);
    
        while (response.NextPageLink != null)
        {
            response = _adlsClient.Account.ListNext(response.NextPageLink);
            accounts.AddRange(response);
        }
    
        return accounts;
    }

## Vytvoření adresáře

Následující fragment kódu ukazuje metodu `CreateDirectory`, kterou můžete použít k vytvoření adresáře v rámci účtu Data Lake Store.

    // Create a directory
    public static void CreateDirectory(string path)
    {
        _adlsFileSystemClient.FileSystem.Mkdirs(_adlsAccountName, path);
    }

## Nahrání souboru do Data Lake Store

Následující fragment kódu ukazuje metodu `UploadFile`, kterou můžete použít k nahrání souborů do účtu Data Lake Store.

    // Upload a file
    public static void UploadFile(string srcFilePath, string destFilePath, bool force = true)
    {
        var parameters = new UploadParameters(srcFilePath, destFilePath, _adlsAccountName, isOverwrite: force);
        var frontend = new DataLakeStoreFrontEndAdapter(_adlsAccountName, _adlsFileSystemClient);
        var uploader = new DataLakeStoreUploader(parameters, frontend);
        uploader.Execute();
    }

DataLakeStoreUploader podporuje rekurzivní odesílání a stahování z místní cesty k souboru (nebo složce) do Data Lake Store.    

## Získání informací o souboru nebo adresáři

Následující fragment kódu ukazuje metodu `GetItemInfo`, kterou můžete použít k načtení informací o souboru nebo adresáři dostupném v Data Lake Store. 

    // Get file or directory info
    public static FileStatusProperties GetItemInfo(string path)
    {
        return _adlsFileSystemClient.FileSystem.GetFileStatus(_adlsAccountName, path).FileStatus;
    }

## Zobrazení seznamu souboru nebo adresářů

Následující fragment kódu ukazuje metodu `ListItem`, kterou můžete použít k zobrazení seznamu souboru a adresářů v účtu Data Lake Store.
    
    // List files and directories
    public static List<FileStatusProperties> ListItems(string directoryPath)
    {
        return _adlsFileSystemClient.FileSystem.ListFileStatus(_adlsAccountName, directoryPath).FileStatuses.FileStatus.ToList();
    }

## Řetězení souborů

Následující fragment kódu ukazuje metodu `ConcatenateFiles`, která slouží k řetězení souborů. 

    // Concatenate files
    public static void ConcatenateFiles(string[] srcFilePaths, string destFilePath)
    {
        _adlsFileSystemClient.FileSystem.Concat(_adlsAccountName, destFilePath, srcFilePaths);
    }

## Připojení k souboru

Následující fragment kódu ukazuje metodu `AppendToFile`, která slouží k připojení dat k souboru, který je už uložený v účtu Data Lake Store.

    // Append to file
    public static void AppendToFile(string path, string content)
    {
        var stream = new MemoryStream(Encoding.UTF8.GetBytes(content));

        _adlsFileSystemClient.FileSystem.Append(_adlsAccountName, path, stream);
    }

## Stažení souboru

Následující fragment kódu ukazuje metodu `DownloadFile`, která slouží k stažení souboru z účtu Data Lake Store.

    // Download file
    public static void DownloadFile(string srcPath, string destPath)
    {
        var stream = _adlsFileSystemClient.FileSystem.Open(_adlsAccountName, srcPath);
        var fileStream = new FileStream(destPath, FileMode.Create);

        stream.CopyTo(fileStream);
        fileStream.Close();
        stream.Close();
    }

## Odstranění účtu Data Lake Store

Následující fragment kódu ukazuje metodu `DeleteAccount`, kterou můžete použít k odstranění účtu Data Lake Store.

    // Delete account
    public static void DeleteAccount()
    {
        _adlsClient.Account.Delete(_resourceGroupName, _adlsAccountName);
    }

## Dodatek: Ukázkový kód

Následující fragment kódu je úplná ukázka kódu, kterou můžete zkopírovat a vložit do aplikace, abyste viděli ucelenou operaci v Data Lake Store. Před spuštěním tohoto fragmentu kódu nezapomeňte zadat požadované hodnoty, například název Data Lake Store, název skupiny prostředků atd. Je taky nutné zadat hodnoty požadované pro ověřování Azure Active Directory, například **\<APPLICATION-CLIENT-ID>** , **\<APPLICATION-REPLY-URI>** a **\<SUBSCRIPTION-ID>**.

Fragment kódu zobrazený níže nabízí metody pro oba přístupy, tj. interaktivní i neinteraktivní, ale blok kódu pro neinteraktivní přístup je změněn na komentář. Interaktivní metoda vyžaduje, abyste zadali ID klienta aplikace AAD a identifikátor URI přesměrování. V tomto požadavku na odkaz obsahuje pokyny, jak získat tyto.

>[AZURE.NOTE] Pokud chcete fragment kódu upravit a raději použít neinteraktivní (`AuthenticateApplication`) metodu, je nutné zadat vstupy metody, které kromě ID klienta a identifikátoru URI odpovědi klienta obsahují taky ověřovací klíč klienta. Informace týkající se vygenerování a načtení ověřovacího klíče klienta jsou taky dostupné v tématu [Vytvoření aplikace Active Directory a objektu zabezpečení pomocí portálu](../resource-group-create-service-principal-portal.md).
    
Nakonec se ujistěte, že na počítači existuje místní cesta a název souboru, které tady zadáte. Pokud hledáte ukázková data, která byste mohli nahrát, můžete použít složku **Ambulance Data** z [úložiště Git Azure Data Lake](https://github.com/MicrosoftBigData/usql/tree/master/Examples/Samples/Data/AmbulanceData).



    using System;
    using System.IO;
    using System.Security;
    using System.Text;
    using System.Collections.Generic;
    using System.Linq;

    using Microsoft.Azure.Management.DataLake.Store;
    using Microsoft.Azure.Management.DataLake.Store.Models;
    using Microsoft.Azure.Management.DataLake.StoreUploader;
    using Microsoft.IdentityModel.Clients.ActiveDirectory;
    using Microsoft.Rest;

    namespace SdkSample
    {
        class Program
        {
            private static DataLakeStoreAccountManagementClient _adlsClient;
            private static DataLakeStoreFileSystemManagementClient _adlsFileSystemClient;

            private static string _adlsAccountName;
            private static string _resourceGroupName;
            private static string _location;

            private static void Main(string[] args)
            {
                _adlsAccountName = "<DATA-LAKE-STORE-NAME>"; // TODO: Replace this value with the name for a NEW Store account.
                _resourceGroupName = "<RESOURCE-GROUP-NAME>"; // TODO: Replace this value. This resource group should already exist.
                _location = "East US 2";

                string localFolderPath = @"C:\local_path\"; // TODO: Make sure this exists and can be overwritten.
                string localFilePath = localFolderPath + "file.txt"; // TODO: Make sure this exists and can be overwritten.
                string remoteFolderPath = "/data_lake_path/";
                string remoteFilePath = remoteFolderPath + "file.txt";

                // Authenticate the user
                var tokenCreds = AuthenticateUser("common", "https://management.core.windows.net/",
                    "<APPLICATION-CLIENT-ID>", new Uri("<APPLICATION-REPLY-URI>")); // TODO: Replace bracketed values.

                SetupClients(tokenCreds, "<SUBSCRIPTION-ID>"); // TODO: Replace bracketed value.

                // Run sample scenarios
                WaitForNewline("Authenticated.", "Creating NEW account.");
                CreateAccount();
                WaitForNewline("Account created.", "Creating a directory.");

                // Create a directory in the Data Lake Store
                CreateDirectory(remoteFolderPath);
                WaitForNewline("Directory created.", "Showing directory info.");

                // Get info about the directory in the Data Lake Store
                var itemInfo = GetItemInfo(remoteFolderPath);
                Console.WriteLine("Type: " + itemInfo.Type);
                Console.WriteLine("Last modified (UTC): " +
                                  new DateTime(1970, 1, 1, 0, 0, 0, 0, DateTimeKind.Utc).AddMilliseconds(
                                      itemInfo.ModificationTime.Value));
                WaitForNewline("Directory info shown.", "Uploading a file.");

                // Upload a file to the Data Lake Store
                UploadFile(localFilePath, remoteFilePath);
                WaitForNewline("File uploaded.", "Listing files and directories.");

                // List the files in the Data Lake Store
                var itemList = ListItems(remoteFolderPath);
                var fileMenuItems = itemList.Select(a => String.Format("{0,15} {1}", a.Type, a.PathSuffix));
                Console.WriteLine(String.Join("\r\n", fileMenuItems));
                WaitForNewline("Files and directories listed.", "Appending content to a file.");

                // Append to a file in the Data Lake Store
                AppendToFile(remoteFilePath, "123");
                WaitForNewline("Content appended.", "Downloading a file.");

                // Download a file from the Data Lake Store
                DownloadFile(remoteFilePath, localFilePath);
                WaitForNewline("File downloaded.", "Deleting account.");

                // Delete account
                DeleteAccount();
                WaitForNewline("Account deleted. You can now exit.");
            }

            // Helper function to show status and wait for user input
            public static void WaitForNewline(string reason, string nextAction = "")
            {
                if (!String.IsNullOrWhiteSpace(nextAction))
                {
                    Console.WriteLine(reason + "\r\nPress ENTER to continue...");
                    Console.ReadLine();
                    Console.WriteLine(nextAction);
                }
                else
                {
                    Console.WriteLine(reason + "\r\nPress ENTER to continue...");
                    Console.ReadLine();
                }
            }

            // Authenticate the user with AAD through an interactive popup.
            // You need to have an application registered with AAD in order to authenticate.
            //   For more information and instructions on how to register your application with AAD, see:
            //   https://azure.microsoft.com/en-us/documentation/articles/resource-group-create-service-principal-portal/
            public static TokenCredentials AuthenticateUser(string tenantId, string resource, string appClientId, Uri appRedirectUri, string userId = "")
            {
                var authContext = new AuthenticationContext("https://login.microsoftonline.com/" + tenantId);

                var tokenAuthResult = authContext.AcquireToken(resource, appClientId, appRedirectUri,
                    PromptBehavior.Auto, UserIdentifier.AnyUser);

                return new TokenCredentials(tokenAuthResult.AccessToken);
            }
            
            /*
            // Authenticate the application with AAD through the application's secret key.
            // You need to have an application registered with AAD in order to authenticate.
            //   For more information and instructions on how to register your application with AAD, see:
            //   https://azure.microsoft.com/en-us/documentation/articles/resource-group-create-service-principal-portal/
            public static TokenCredentials AuthenticateApplication(string tenantId, string resource, string appClientId, Uri appRedirectUri, SecureString clientSecret)
            {
                var authContext = new AuthenticationContext("https://login.microsoftonline.com/" + tenantId);
                var credential = new ClientCredential(appClientId, clientSecret);

                var tokenAuthResult = authContext.AcquireToken(resource, credential);

                return new TokenCredentials(tokenAuthResult.AccessToken);
            }
            */

            //Set up clients
            public static void SetupClients(TokenCredentials tokenCreds, string subscriptionId)
            {
                _adlsClient = new DataLakeStoreAccountManagementClient(tokenCreds);
                _adlsClient.SubscriptionId = subscriptionId;

                _adlsFileSystemClient = new DataLakeStoreFileSystemManagementClient(tokenCreds);
            }

            // Create account
            public static void CreateAccount()
            {
                // Create ADLS account
                var adlsParameters = new DataLakeStoreAccount(location: _location);
                _adlsClient.Account.Create(_resourceGroupName, _adlsAccountName, adlsParameters);
            }

            // Delete account
            public static void DeleteAccount()
            {
                _adlsClient.Account.Delete(_resourceGroupName, _adlsAccountName);
            }

            // List all ADLS accounts within the subscription
            public static List<DataLakeStoreAccount> ListAdlStoreAccounts()
            {
                var response = _adlsClient.Account.List(_adlsAccountName);
                var accounts = new List<DataLakeStoreAccount>(response);

                while (response.NextPageLink != null)
                {
                    response = _adlsClient.Account.ListNext(response.NextPageLink);
                    accounts.AddRange(response);
                }

                return accounts;
            }

            // Upload a file
            public static void UploadFile(string srcFilePath, string destFilePath, bool force = true)
            {
                var parameters = new UploadParameters(srcFilePath, destFilePath, _adlsAccountName, isOverwrite: force);
                var frontend = new DataLakeStoreFrontEndAdapter(_adlsAccountName, _adlsFileSystemClient);
                var uploader = new DataLakeStoreUploader(parameters, frontend);
                uploader.Execute();
            }

            // Concatenate files
            public static void ConcatenateFiles(string[] srcFilePaths, string destFilePath)
            {
                _adlsFileSystemClient.FileSystem.Concat(_adlsAccountName, destFilePath, srcFilePaths);
            }

            // Get file or directory info
            public static FileStatusProperties GetItemInfo(string path)
            {
                return _adlsFileSystemClient.FileSystem.GetFileStatus(_adlsAccountName, path).FileStatus;
            }

            // List files and directories
            public static List<FileStatusProperties> ListItems(string directoryPath)
            {
                return _adlsFileSystemClient.FileSystem.ListFileStatus(_adlsAccountName, directoryPath).FileStatuses.FileStatus.ToList();
            }

            // Download file
            public static void DownloadFile(string srcPath, string destPath)
            {
                var stream = _adlsFileSystemClient.FileSystem.Open(_adlsAccountName, srcPath);
                var fileStream = new FileStream(destPath, FileMode.Create);

                stream.CopyTo(fileStream);
                fileStream.Close();
                stream.Close();
            }

            // Append to file
            public static void AppendToFile(string path, string content)
            {
                var stream = new MemoryStream(Encoding.UTF8.GetBytes(content));

                _adlsFileSystemClient.FileSystem.Append(_adlsAccountName, path, stream);
            }

            // Create a directory
            public static void CreateDirectory(string path)
            {
                _adlsFileSystemClient.FileSystem.Mkdirs(_adlsAccountName, path);
            }
        }
    }


## Další kroky

- [Zabezpečení dat v Data Lake Store](data-lake-store-secure-data.md)
- [Použití Azure Data Lake Analytics s Data Lake Store](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
- [Použití Azure HDInsight s Data Lake Store](data-lake-store-hdinsight-hadoop-use-portal.md)
- [Referenční dokumentace sady SDK rozhraní .NET služby Data Lake Store](https://msdn.microsoft.com/library/mt581387.aspx)
- [Referenční dokumentace architektury REST služby Data Lake Store](https://msdn.microsoft.com/library/mt693424.aspx)



<!--HONumber=sep16_HO2-->


