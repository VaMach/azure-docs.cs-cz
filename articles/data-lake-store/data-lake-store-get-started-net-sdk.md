---
title: "Vývoj aplikací pro Azure Data Lake Store pomocí .NET SDK | Dokumentace Microsoftu"
description: "Přečtěte si, jak pomocí Azure Data Lake Store .NET SDK vytvořit účet Data Lake Store a provádět další základní operace."
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: ea57d5a9-2929-4473-9d30-08227912aba7
ms.service: data-lake-store
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 02/28/2017
ms.author: nitinme
translationtype: Human Translation
ms.sourcegitcommit: 1e6ae31b3ef2d9baf578b199233e61936aa3528e
ms.openlocfilehash: 2ab4e2be8509bb264f496e7ebc6b4b50187c0151
ms.lasthandoff: 03/03/2017


---
# <a name="get-started-with-azure-data-lake-store-using-net-sdk"></a>Začínáme s Azure Data Lake Store pomocí sady .NET SDK
> [!div class="op_single_selector"]
> * [Azure Portal](data-lake-store-get-started-portal.md)
> * [PowerShell](data-lake-store-get-started-powershell.md)
> * [.NET SDK](data-lake-store-get-started-net-sdk.md)
> * [Java SDK](data-lake-store-get-started-java-sdk.md)
> * [REST API](data-lake-store-get-started-rest-api.md)
> * [Azure CLI](data-lake-store-get-started-cli.md)
> * [Node.js](data-lake-store-manage-use-nodejs.md)
> * [Python](data-lake-store-get-started-python.md)
>
> 

Naučte se používat sadu [.NET SDK pro Azure Data Lake Store](https://msdn.microsoft.com/library/mt581387.aspx) k provádění základních operací, jako je vytváření složek, nahrávání a stahování datových souborů atd. Další informace týkající se Data Lake najdete v tématu [Azure Data Lake Store](data-lake-store-overview.md).

## <a name="prerequisites"></a>Požadavky
* **Visual Studio 2013 nebo 2015**. Níže uvedené pokyny používají Visual Studio 2015.

* **Předplatné Azure**. Viz [Získání bezplatné zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/).

* **Účet Azure Data Lake Store**. Pokyny k vytvoření účtu najdete v tématu [Začínáme s Azure Data Lake Store](data-lake-store-get-started-portal.md).

* **Vytvoření aplikace Azure Active Directory**. Aplikaci Azure AD použijete k ověření aplikace Data Lake Store ve službě Azure AD. Existují různé přístupy k ověřování ve službě Azure AD, jsou to **ověřování koncového uživatele** nebo **ověřování služba-služba**. Pokyny a další informace o ověřování najdete v tématu [Ověření ve službě Data Lake Store pomocí služby Azure Active Directory](data-lake-store-authenticate-using-active-directory.md).

## <a name="create-a-net-application"></a>Vytvoření aplikace .NET
1. Otevřete Visual Studio a vytvořte konzolovou aplikaci.
2. V nabídce **Soubor** klikněte na položku **Nový** a potom klikněte na položku **Projekt**.
3. V části **Nový projekt** zadejte nebo vyberte tyto hodnoty:
   
   | Vlastnost | Hodnota |
   | --- | --- |
   | Kategorie |Šablony/Visual C#/Windows |
   | Šablona |Konzolová aplikace |
   | Name (Název) |VytvořeníAplikaceADL |
4. Kliknutím na tlačítko **OK** vytvořte projekt.
5. Přidejte do projektu balíčky Nuget.
   
   1. V Průzkumníku řešení klikněte pravým tlačítkem na název projektu a klikněte na možnost **Správa balíčků NuGet**.
   2. Ujistěte se, že na kartě **Správce balíčků Nuget** je položka **Zdroj balíčku** nastavena na možnost **nuget.org** a je zaškrtnuto políčko **Zahrnout předběžné verze**.
   3. Vyhledejte a nainstalujte následující balíčky NuGet:
      
      * `Microsoft.Azure.Management.DataLake.Store` – Tento kurz používá verzi v1.0.4.
      * `Microsoft.Azure.Management.DataLake.StoreUploader` – Tento kurz používá verzi v1.0.1-preview.
      * `Microsoft.Rest.ClientRuntime.Azure.Authentication` – Tento kurz používá verzi v2.2.11.
        
        ![Přidání zdroje Nuget](./media/data-lake-store-get-started-net-sdk/ADL.Install.Nuget.Package.png "Vytvoření nového účtu Azure Data Lake")
   4. Zavřete **Správce balíčků Nuget**.
6. Otevřete soubor **Program.cs**, odstraňte stávající kód a potom vložte následující příkazy, čímž přidáte odkazy na obory názvů.
   
        using System;
        using System.Threading;
   
        using Microsoft.Rest.Azure.Authentication;
        using Microsoft.Azure.Management.DataLake.Store;
        using Microsoft.Azure.Management.DataLake.Store.Models;
        using Microsoft.Azure.Management.DataLake.StoreUploader;
        using Microsoft.IdentityModel.Clients.ActiveDirectory;
        using System.Security.Cryptography.X509Certificates; //Required only if you are using an Azure AD application created with certificates

7. Deklarujte proměnné, jak vidíte níže, a zadejte hodnoty názvu služby Data Lake Store a názvu skupiny prostředků, které již existují. Taky se ujistěte, že místní cesta a název souboru, které tady zadáte, existují na počítači. Za deklarace oboru názvů přidejte následující fragment kódu.
   
        namespace SdkSample
        {
            class Program
            {
                private static DataLakeStoreAccountManagementClient _adlsClient;
                private static DataLakeStoreFileSystemManagementClient _adlsFileSystemClient;
   
                private static string _adlsAccountName;
                private static string _resourceGroupName;
                private static string _location;
                private static string _subId;

                private static void Main(string[] args)
                {
                    _adlsAccountName = "<DATA-LAKE-STORE-NAME>"; // TODO: Replace this value with the name of your existing Data Lake Store account.
                    _resourceGroupName = "<RESOURCE-GROUP-NAME>"; // TODO: Replace this value with the name of the resource group containing your Data Lake Store account.
                    _location = "East US 2";
                    _subId = "<SUBSCRIPTION-ID>";

                    string localFolderPath = @"C:\local_path\"; // TODO: Make sure this exists and can be overwritten.
                    string localFilePath = localFolderPath + "file.txt"; // TODO: Make sure this exists and can be overwritten.
                    string remoteFolderPath = "/data_lake_path/";
                    string remoteFilePath = remoteFolderPath + "file.txt";
                }
            }
        }

Ve zbývajících oddílech tohoto článku uvidíte, jak používat dostupné metody rozhraní .NET k provádění operací, jako je ověřování, nahrávání souborů atd.

## <a name="authentication"></a>Authentication

### <a name="if-you-are-using-end-user-authentication-recommended-for-this-tutorial"></a>Pokud používáte ověřování koncového uživatele (doporučeno pro tento kurz)

Tuto metodu použijte se stávající nativní aplikací Azure AD, pokud chcete ověřovat vaši aplikaci **interaktivně**, což znamená, že budete vyzváni k zadání přihlašovacích údajů Azure. 

Následující fragment kódu pro snadnější použití pro ID klienta a identifikátor URI přesměrování používá výchozí hodnoty, které budou fungovat s jakýmkoli předplatným Azure. Chcete-li tento kurz projít rychleji, doporučujeme vám použít tento přístup. V následujícím fragmentu kódu stačí zadat hodnotu vašeho ID tenanta. Můžete jej zjistit pomocí pokynů uvedených v tématu [Vytvoření aplikace Active Directory](data-lake-store-end-user-authenticate-using-active-directory.md).

    // User login via interactive popup
    // Use the client ID of an existing AAD Web application.
    SynchronizationContext.SetSynchronizationContext(new SynchronizationContext());
    var tenant_id = "<AAD_tenant_id>"; // Replace this string with the user's Azure Active Directory tenant ID
    var nativeClientApp_clientId = "1950a258-227b-4e31-a9cf-717495945fc2";
    var activeDirectoryClientSettings = ActiveDirectoryClientSettings.UsePromptOnly(nativeClientApp_clientId, new Uri("urn:ietf:wg:oauth:2.0:oob"));
    var creds = UserTokenProvider.LoginWithPromptAsync(tenant_id, activeDirectoryClientSettings).Result;

Několik věcí, které je vhodné vědět o fragmentu kódu uvedeném výše.

* Abychom vám umožnili projít tento kurz rychleji, využívá tento fragment kódu doménu a ID klienta služby Azure AD, které jsou ve výchozím nastavení dostupné pro všechna předplatná Azure. Můžete tedy **použít ve své aplikaci tento fragment kódu bez jakýchkoli úprav**.
* Pokud však chcete používat vlastní doménu a ID klienta aplikace Azure AD, musíte vytvořit nativní aplikaci Azure AD a pak pro vytvořenou aplikaci použít příslušné ID tenanta, ID klienta a identifikátor URI přesměrování Azure AD. Pokyny najdete v tématu [Vytvoření aplikace Active Directory pro ověřování koncového uživatele pomocí služby Data Lake Store](data-lake-store-end-user-authenticate-using-active-directory.md).

### <a name="if-you-are-using-service-to-service-authentication-with-client-secret"></a>Pokud používáte ověřování služba-služba s tajným klíčem klienta
Následující fragment kódu můžete použít k **neinteraktivnímu** ověřování vaší aplikace pomocí tajného klíče klienta, klíče pro aplikaci nebo instančního objektu. Použijte tento fragment kódu se stávající aplikací Azure AD Webová aplikace. Pokyny k vytvoření webové aplikace Azure AD a zjištění ID a tajného klíče klienta v následujícím fragmentu kódu najdete v tématu [Vytvoření aplikace Active Directory pro ověřování služba-služba pomocí služby Data Lake Store](data-lake-store-authenticate-using-active-directory.md).

    // Service principal / appplication authentication with client secret / key
    // Use the client ID of an existing AAD "Web App" application.
    SynchronizationContext.SetSynchronizationContext(new SynchronizationContext());
    var domain = "<AAD-directory-domain>";
    var webApp_clientId = "<AAD-application-clientid>";
    var clientSecret = "<AAD-application-client-secret>";
    var clientCredential = new ClientCredential(webApp_clientId, clientSecret);
    var creds = ApplicationTokenProvider.LoginSilentAsync(domain, clientCredential).Result;

### <a name="if-you-are-using-service-to-service-authentication-with-certificate"></a>Pokud používáte ověřování služba-služba s certifikátem
Třetí možností je použít následující fragment kódu k **neinteraktivnímu** ověřování vaší aplikace pomocí certifikátu pro aplikaci nebo instanční objekt Azure Active Directory. Použijte tento fragment kódu se stávající [aplikací Azure AD s certifikáty](../azure-resource-manager/resource-group-authenticate-service-principal.md#create-service-principal-with-certificate).

    // Service principal / application authentication with certificate
    // Use the client ID and certificate of an existing AAD "Web App" application.
    SynchronizationContext.SetSynchronizationContext(new SynchronizationContext());
    var domain = "<AAD-directory-domain>";
    var webApp_clientId = "<AAD-application-clientid>";
    System.Security.Cryptography.X509Certificates.X509Certificate2 clientCert = <AAD-application-client-certificate>
    var clientAssertionCertificate = new ClientAssertionCertificate(webApp_clientId, clientCert);
    var creds = ApplicationTokenProvider.LoginSilentWithCertificateAsync(domain, clientAssertionCertificate).Result;

## <a name="create-client-objects"></a>Vytvoření objektů klienta
Následující fragment kódu vytvoří účet Data Lake Store a objekty klientů systému souborů, které slouží k vydávání žádostí na službu.

    // Create client objects and set the subscription ID
    _adlsClient = new DataLakeStoreAccountManagementClient(creds);
    _adlsFileSystemClient = new DataLakeStoreFileSystemManagementClient(creds);

    _adlsClient.SubscriptionId = _subId;

## <a name="list-all-data-lake-store-accounts-within-a-subscription"></a>Zobrazení seznamu všech účtů Data Lake Store v rámci předplatného
Následující fragment kódu zobrazí všechny účty Data Lake Store v rámci daného předplatného Azure.

    // List all ADLS accounts within the subscription
    public static List<DataLakeStoreAccount> ListAdlStoreAccounts()
    {
        var response = _adlsClient.Account.List();
        var accounts = new List<DataLakeStoreAccount>(response);

        while (response.NextPageLink != null)
        {
            response = _adlsClient.Account.ListNext(response.NextPageLink);
            accounts.AddRange(response);
        }

        return accounts;
    }

## <a name="create-a-directory"></a>Vytvoření adresáře
Následující fragment kódu ukazuje metodu `CreateDirectory`, kterou můžete použít k vytvoření adresáře v rámci účtu Data Lake Store.

    // Create a directory
    public static void CreateDirectory(string path)
    {
        _adlsFileSystemClient.FileSystem.Mkdirs(_adlsAccountName, path);
    }

## <a name="upload-a-file"></a>Nahrání souboru
Následující fragment kódu ukazuje metodu `UploadFile`, kterou můžete použít k nahrání souborů do účtu Data Lake Store.

    // Upload a file
    public static void UploadFile(string srcFilePath, string destFilePath, bool force = true)
    {
        var parameters = new UploadParameters(srcFilePath, destFilePath, _adlsAccountName, isOverwrite: force);
        var frontend = new DataLakeStoreFrontEndAdapter(_adlsAccountName, _adlsFileSystemClient);
        var uploader = new DataLakeStoreUploader(parameters, frontend);
        uploader.Execute();
    }

`DataLakeStoreUploader` podporuje rekurzivní nahrávání a stahování mezi místní cestou k souboru a cestou k souboru ve službě Data Lake Store.    

## <a name="get-file-or-directory-info"></a>Získání informací o souboru nebo adresáři
Následující fragment kódu ukazuje metodu `GetItemInfo`, kterou můžete použít k načtení informací o souboru nebo adresáři dostupném v Data Lake Store. 

    // Get file or directory info
    public static FileStatusProperties GetItemInfo(string path)
    {
        return _adlsFileSystemClient.FileSystem.GetFileStatus(_adlsAccountName, path).FileStatus;
    }

## <a name="list-file-or-directories"></a>Zobrazení seznamu souboru nebo adresářů
Následující fragment kódu ukazuje metodu `ListItem`, kterou můžete použít k zobrazení seznamu souboru a adresářů v účtu Data Lake Store.

    // List files and directories
    public static List<FileStatusProperties> ListItems(string directoryPath)
    {
        return _adlsFileSystemClient.FileSystem.ListFileStatus(_adlsAccountName, directoryPath).FileStatuses.FileStatus.ToList();
    }

## <a name="concatenate-files"></a>Řetězení souborů
Následující fragment kódu ukazuje metodu `ConcatenateFiles`, která slouží k řetězení souborů. 

    // Concatenate files
    public static void ConcatenateFiles(string[] srcFilePaths, string destFilePath)
    {
        _adlsFileSystemClient.FileSystem.Concat(_adlsAccountName, destFilePath, srcFilePaths);
    }

## <a name="append-to-a-file"></a>Připojení k souboru
Následující fragment kódu ukazuje metodu `AppendToFile`, která slouží k připojení dat k souboru, který je už uložený v účtu Data Lake Store.

    // Append to file
    public static void AppendToFile(string path, string content)
    {
        var stream = new MemoryStream(Encoding.UTF8.GetBytes(content));

        _adlsFileSystemClient.FileSystem.Append(_adlsAccountName, path, stream);
    }

## <a name="download-a-file"></a>Stažení souboru
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

## <a name="next-steps"></a>Další kroky
* [Zabezpečení dat ve službě Data Lake Store](data-lake-store-secure-data.md)
* [Použití Azure Data Lake Analytics se službou Data Lake Store](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
* [Použití Azure HDInsight se službou Data Lake Store](data-lake-store-hdinsight-hadoop-use-portal.md)
* [Referenční dokumentace sady SDK rozhraní .NET služby Data Lake Store](https://msdn.microsoft.com/library/mt581387.aspx)
* [Referenční dokumentace architektury REST služby Data Lake Store](https://msdn.microsoft.com/library/mt693424.aspx)


