---
title: ".NET SDK: Operace systému souborů v Azure Data Lake Store | Dokumentace Microsoftu"
description: "Použití sady .NET SDK pro Azure Data Lake Store k provádění operací systému souborů ve službě Data Lake Store, jako je například vytváření složek atd."
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.service: data-lake-store
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 09/28/2017
ms.author: nitinme
ms.openlocfilehash: f525bd81ebbc27a4730bdced5c8778b6f2ac69b0
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="filesystem-operations-on-azure-data-lake-store-using-net-sdk"></a>Operace systému souborů v Azure Data Lake Store pomocí sady .NET SDK
> [!div class="op_single_selector"]
> * [.NET SDK](data-lake-store-data-operations-net-sdk.md)
> * [Java SDK](data-lake-store-get-started-java-sdk.md)
> * [REST API](data-lake-store-data-operations-rest-api.md)
> * [Python](data-lake-store-data-operations-python.md)
>
>

V tomto článku se dozvíte, jak provádět operace systému souborů ve službě Data Lake Store pomocí sady .NET SDK. Mezi operace systému souborů patří vytváření složek v účtu Data Lake Store, nahrávání souborů, stahování souborů atd.

Pokyny k provádění operací správy účtů ve službě Data Lake Store pomocí sady .NET SDK najdete v tématu [Operace správy účtů ve službě Data Lake Store pomocí sady .NET SDK](data-lake-store-get-started-net-sdk.md).

## <a name="prerequisites"></a>Požadavky
* **Visual Studio 2013, 2015 nebo 2017**. Níže uvedené pokyny používají sadu Visual Studio 2017.

* **Předplatné Azure**. Viz [Získání bezplatné zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/).

* **Účet Azure Data Lake Store**. Pokyny k vytvoření účtu najdete v tématu [Začínáme s Azure Data Lake Store](data-lake-store-get-started-portal.md).

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
5. Přidejte do projektu balíčky NuGet.

   1. V Průzkumníku řešení klikněte pravým tlačítkem na název projektu a klikněte na možnost **Správa balíčků NuGet**.
   2. Ujistěte se, že na kartě **Správce balíčků NuGet** je položka **Zdroj balíčku** nastavena na hodnotu **nuget.org** a že je zaškrtnuto políčko **Zahrnout předběžné verze**.
   3. Vyhledejte a nainstalujte následující balíčky NuGet:

      * `Microsoft.Azure.Management.DataLake.Store` – Tento kurz používá verzi v2.1.3-preview.
      * `Microsoft.Rest.ClientRuntime.Azure.Authentication` – Tento kurz používá verzi v2.2.12.

        ![Přidání zdroje NuGet](./media/data-lake-store-get-started-net-sdk/data-lake-store-install-nuget-package.png "Vytvoření nového účtu Azure Data Lake")
   4. Zavřete **Správce balíčků NuGet**.
6. Otevřete soubor **Program.cs**, odstraňte stávající kód a potom vložte následující příkazy, čímž přidáte odkazy na obory názvů.

        using System;
        using System.IO;
        using System.Security.Cryptography.X509Certificates; // Required only if you are using an Azure AD application created with certificates
        using System.Threading;

        using Microsoft.Azure.Management.DataLake.Store;
        using Microsoft.Azure.Management.DataLake.Store.Models;
        using Microsoft.IdentityModel.Clients.ActiveDirectory;
        using Microsoft.Rest.Azure.Authentication;

7. Deklarujte proměnné, jak je znázorněno níže, a nahraďte zástupné symboly příslušnými hodnotami. Také se ujistěte, že na počítači existuje místní cesta a název souboru, které tady zadáte.

        namespace SdkSample
        {
            class Program
            {
                private static DataLakeStoreFileSystemManagementClient _adlsFileSystemClient;

                private static string _adlsAccountName;
                private static string _resourceGroupName;
                private static string _location;

                private static async void Main(string[] args)
                {
                    _adlsAccountName = "<DATA-LAKE-STORE-NAME>"; // TODO: Replace this value with the name of your existing Data Lake Store account.
                    _resourceGroupName = "<RESOURCE-GROUP-NAME>"; // TODO: Replace this value with the name of the resource group containing your Data Lake Store account.
                    _location = "East US 2";

                    string localFolderPath = @"C:\local_path\"; // TODO: Make sure this exists and can be overwritten.
                    string localFilePath = Path.Combine(localFolderPath, "file.txt"); // TODO: Make sure this exists and can be overwritten.
                    string remoteFolderPath = "/data_lake_path/";
                    string remoteFilePath = Path.Combine(remoteFolderPath, "file.txt");
                }
            }
        }

Ve zbývajících oddílech tohoto článku uvidíte, jak používat dostupné metody rozhraní .NET k provádění operací, jako je ověřování, nahrávání souborů atd.

## <a name="authentication"></a>Authentication

* Pokud chcete ve své aplikaci ověřování koncového uživatele, přečtěte si téma [Ověřování koncového uživatele pomocí služby Data Lake Store s použitím sady .NET SDK](data-lake-store-end-user-authenticate-net-sdk.md).
* Pokud chcete ve své aplikaci ověřování služba-služba, přečtěte si téma [Ověřování služba-služba pomocí služby Data Lake Store s použitím sady .NET SDK](data-lake-store-service-to-service-authenticate-net-sdk.md).


## <a name="create-client-objects"></a>Vytvoření objektů klienta
Následující fragment kódu vytvoří účet Data Lake Store a objekty klientů systému souborů, které slouží k vydávání žádostí na službu.

    // Create client objects
    _adlsFileSystemClient = new DataLakeStoreFileSystemManagementClient(creds);

## <a name="create-a-directory"></a>Vytvoření adresáře
Přidejte následující metodu do vaší třídy. Fragment kódu ukazuje metodu `CreateDirectory()`, kterou můžete použít k vytvoření adresáře v rámci účtu Data Lake Store.

    // Create a directory
    public static void CreateDirectory(string path)
    {
            _adlsFileSystemClient.FileSystem.Mkdirs(_adlsAccountName, path);
    }

Do metody `Main()` přidejte následující fragment kódu pro vyvolání metody `CreateDirectory()`.

    CreateDirectory(remoteFolderPath);
    Console.WriteLine("Created a directory in the Data Lake Store account. Press any key to continue ...");
    Console.ReadLine();

## <a name="upload-a-file"></a>Nahrání souboru
Přidejte následující metodu do vaší třídy. Fragment kódu ukazuje metodu `UploadFile()`, kterou můžete použít k nahrání souborů do účtu Data Lake Store. Sada SDK podporuje rekurzivní nahrávání a stahování mezi místní cestou k souboru a cestou k souboru ve službě Data Lake Store.

    // Upload a file
    public static void UploadFile(string srcFilePath, string destFilePath, bool force = true)
    {
        _adlsFileSystemClient.FileSystem.UploadFile(_adlsAccountName, srcFilePath, destFilePath, overwrite:force);
    }

Do metody `Main()` přidejte následující fragment kódu pro vyvolání metody `UploadFile()`.

    UploadFile(localFilePath, remoteFilePath, true);
    Console.WriteLine("Uploaded file in the directory. Press any key to continue...");
    Console.ReadLine();
    

## <a name="get-file-or-directory-info"></a>Získání informací o souboru nebo adresáři
Následující fragment kódu ukazuje metodu `GetItemInfo()`, kterou můžete použít k načtení informací o souboru nebo adresáři dostupném v Data Lake Store.

    public static FileStatusProperties GetItemInfo(string path)
    {
        return _adlsFileSystemClient.FileSystem.GetFileStatus(_adlsAccountName, path).FileStatus;
    }

Do metody `Main()` přidejte následující fragment kódu pro vyvolání metody `GetItemInfo()`.

    
    var fileProperties = GetItemInfo(remoteFilePath);
    Console.WriteLine("The owner of the file at the path is:", fileProperties.Owner);
    Console.WriteLine("Press any key to continue...");
    Console.ReadLine();

## <a name="list-file-or-directories"></a>Zobrazení seznamu souboru nebo adresářů
Následující fragment kódu ukazuje metodu `ListItems()`, kterou můžete použít k zobrazení seznamu souboru a adresářů v účtu Data Lake Store.

    // List files and directories
    public static List<FileStatusProperties> ListItems(string directoryPath)
    {
        return _adlsFileSystemClient.FileSystem.ListFileStatus(_adlsAccountName, directoryPath).FileStatuses.FileStatus.ToList();
    }

Do metody `Main()` přidejte následující fragment kódu pro vyvolání metody `ListItems()`.

    var itemList = ListItems(remoteFolderPath);
    var fileMenuItems = itemList.Select(a => String.Format("{0,15} {1}", a.Type, a.PathSuffix));
    Console.WriteLine(String.Join("\r\n", fileMenuItems));
    Console.WriteLine("Files and directories listed. Press any key to continue ...");
    Console.ReadLine();

## <a name="concatenate-files"></a>Řetězení souborů
Následující fragment kódu ukazuje metodu `ConcatenateFiles()`, která slouží k řetězení souborů.

    // Concatenate files
    public static void ConcatenateFiles(string[] srcFilePaths, string destFilePath)
    {
        _adlsFileSystemClient.FileSystem.Concat(_adlsAccountName, destFilePath, srcFilePaths);
    }

Do metody `Main()` přidejte následující fragment kódu pro vyvolání metody `ConcatenateFiles()`. Tento fragment kódu předpokládá, že jste do účtu Data Lake Store nahráli další soubor a cesta k tomuto souboru je zadaná v řetězci *anotherRemoteFilePath*.

    string[] stringOfFiles = new String[] {remoteFilePath, anotherRemoteFilePath};
    string destFilePath = Path.Combine(remoteFolderPath, "Concatfile.txt");
    ConcatenateFiles(stringOfFiles, destFilePath);
    Console.WriteLine("Files concatinated. Press any key to continue ...");
    Console.ReadLine();

## <a name="append-to-a-file"></a>Připojení k souboru
Následující fragment kódu ukazuje metodu `AppendToFile()`, která slouží k připojení dat k souboru, který je už uložený v účtu Data Lake Store.

    // Append to file
    public static void AppendToFile(string path, string content)
    {
        using (var stream = new MemoryStream(Encoding.UTF8.GetBytes(content)))
        {
            _adlsFileSystemClient.FileSystem.AppendAsync(_adlsAccountName, path, stream);
        }
    }

Do metody `Main()` přidejte následující fragment kódu pro vyvolání metody `AppendToFile()`.

    AppendToFile(remoteFilePath, "123");
    Console.WriteLine("Content appended. Press any key to continue ...");
    Console.ReadLine();

## <a name="download-a-file"></a>Stažení souboru
Následující fragment kódu ukazuje metodu `DownloadFile()`, která slouží k stažení souboru z účtu Data Lake Store.

    // Download file
    public static void DownloadFile(string srcFilePath, string destFilePath)
    {
        _adlsFileSystemClient.FileSystem.DownloadFile(_adlsAccountName, srcFilePath, destFilePath, overwrite:true);
    }

Do metody `Main()` přidejte následující fragment kódu pro vyvolání metody `DownloadFile()`.

    DownloadFile(destFilePath, localFilePath);
    Console.WriteLine("File downloaded. Press any key to continue ...");
    Console.ReadLine();

## <a name="see-also"></a>Viz také
* [Operace správy účtů ve službě Data Lake Store pomocí sady .NET SDK](data-lake-store-get-started-net-sdk.md)
* [Referenční dokumentace sady SDK rozhraní .NET služby Data Lake Store](https://docs.microsoft.com/dotnet/api/overview/azure/data-lake-store?view=azure-dotnet)

## <a name="next-steps"></a>Další kroky
* [Zabezpečení dat ve službě Data Lake Store](data-lake-store-secure-data.md)
