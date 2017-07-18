---
title: "Vývoj pro Azure File Storage pomocí .NET | Dokumentace Microsoftu"
description: "Zjistěte, jak vyvíjet aplikace a služby .NET, které používají službu Azure File Storage k ukládání dat souborů."
services: storage
documentationcenter: .net
author: RenaShahMSFT
manager: aungoo
editor: tysonn
ms.assetid: 6a889ee1-1e60-46ec-a592-ae854f9fb8b6
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 05/27/2017
ms.author: renash
ms.translationtype: HT
ms.sourcegitcommit: 2ad539c85e01bc132a8171490a27fd807c8823a4
ms.openlocfilehash: e26da88ef1803d47d7286c5ae836ac9c041dadd1
ms.contentlocale: cs-cz
ms.lasthandoff: 07/12/2017

---

# <a name="develop-for-azure-file-storage-with-net"></a>Vývoj pro službu Azure File Storage pomocí .NET 
> [!NOTE]
> Tento článek ukazuje, jak spravovat službu Azure File Storage pomocí kódu .NET. Další informace o službě Azure File Storage najdete v tématu [Úvod do služby Azure File Storage](storage-files-introduction.md).
>

[!INCLUDE [storage-selector-file-include](../../includes/storage-selector-file-include.md)]

[!INCLUDE [storage-check-out-samples-dotnet](../../includes/storage-check-out-samples-dotnet.md)]

## <a name="about-this-tutorial"></a>O tomto kurzu
Tento kurz vám ukáže základy používání technologie .NET k vývoji aplikací a služeb, které používají službu Azure File Storage k ukládání dat souborů. V tomto kurzu vytvoříme jednoduchou konzolovou aplikaci a ukážeme si, jak provádět základní akce s technologií .NET a službou Azure File Storage:

* Získání obsahu souboru
* Nastavte kvótu (maximální velikost) pro sdílenou složku.
* Vytvořte sdílený přístupový podpis (klíč SAS) pro soubor, který používá zásady sdíleného přístupu definované ve sdílené složce.
* Zkopírujte soubor do jiného souboru ve stejném účtu úložiště.
* Zkopírujte soubor do objektu blob ve stejném účtu úložiště.
* Použijte Azure Storage Metrics pro řešení potíží.

> [!Note]  
> Protože je služba Azure File Storage přístupná přes protokol SMB, je možné psát jednoduché aplikace, které přistupují ke sdílené složce Azure pomocí standardních tříd System.IO pro vstupně-výstupní operace se soubory. Tento článek popisuje, jak psát aplikace, které používají sadu .NET SDK pro Azure Storage, která se službou Azure File Storage komunikuje pomocí rozhraní [REST API pro Azure File Storage](https://docs.microsoft.com/rest/api/storageservices/fileservices/file-service-rest-api). 


## <a name="create-the-console-application-and-obtain-the-assembly"></a>Vytvoření konzolové aplikace a získání sestavení
V sadě Visual Studio vytvořte novou konzolovou aplikaci pro Windows. Následující kroky ukazují, jak vytvořit konzolovou aplikaci v sadě Visual Studio 2017, ale kroky v jiných verzích sady Visual Studio se podobají.

1. Vyberte **Soubor**  >  **Nový**  >  **Projekt**.
2. Vyberte **Instalováno** > **Šablony** > **Visual C#** > **Klasická plocha Windows**.
3. Vyberte **Aplikace konzoly (.NET Framework)**.
4. Do pole **Název** zadejte název vaší aplikace.
5. Vyberte **OK**.

Všechny příklady kódu v tomto kurzu můžete přidat do metody `Main()` v souboru `Program.cs` vaší konzolové aplikace.

Můžete použít knihovnu klienta služby Azure Storage z libovolného typu aplikace .NET, včetně webové aplikace nebo cloudové služby Azure, desktopové nebo mobilní aplikace. V této příručce použijeme konzolovou aplikaci kvůli zjednodušení.

## <a name="use-nuget-to-install-the-required-packages"></a>Použití balíčku NuGet k instalaci požadovaných balíčků
Abyste mohli tento kurz dokončit, potřebujete ze svého projektu odkazovat na dva balíčky:

* [Microsoft Azure Storage Client Library for .NET](https://www.nuget.org/packages/WindowsAzure.Storage/): Tento balíček zajišťuje programový přístup k datovým prostředkům na účtu úložiště.
* [Microsoft Azure Configuration Manager library for .NET:](https://www.nuget.org/packages/Microsoft.WindowsAzure.ConfigurationManager/) Tento balíček poskytuje třídu pro potřeby analýzy připojovacího řetězce v konfiguračním souboru bez ohledu na to, kde je aplikace spuštěná.

K získání obou balíčků můžete použít balíček NuGet. Postupujte následovně:

1. Klikněte v **Průzkumníku řešení** pravým tlačítkem na projekt a vyberte **Spravovat balíčky NuGet**.
2. Proveďte online hledání textu „WindowsAzure.Storage“ a kliknutím na **Instalovat** nainstalujte balíček knihovny klienta úložiště a jeho závislosti.
3. Proveďte online hledání textu „WindowsAzure.ConfigurationManager“ a kliknutím na **Instalovat** nainstalujete správce konfigurace Azure.

## <a name="save-your-storage-account-credentials-to-the-appconfig-file"></a>Uložení přihlašovacích údajů pro účet úložiště do souboru app.config
Dál uložte svoje přihlašovací údaje do souboru app.config vašeho projektu. Upravte soubor app.config tak, aby vypadal podobně jako v následujícím příkladu, `myaccount` nahraďte názvem svého účtu úložiště a `mykey` nahraďte svým klíčem účtu úložiště.

```xml
<?xml version="1.0" encoding="utf-8" ?>
<configuration>
    <startup>
        <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.5" />
    </startup>
    <appSettings>
        <add key="StorageConnectionString" value="DefaultEndpointsProtocol=https;AccountName=myaccount;AccountKey=StorageAccountKeyEndingIn==" />
    </appSettings>
</configuration>
```

> [!NOTE]
> Nejnovější verze emulátoru úložiště Azure nepodporuje službu Azure File Storage. Aby váš připojovací řetězec mohl pracovat se službou Azure File Storage, musí jako cíl mít účet služby Azure Storage v cloudu.

## <a name="add-using-directives"></a>Přidání direktiv using
V Průzkumníku řešení otevřete soubor `Program.cs` a na začátek souboru přidejte následující direktivy using.

```csharp
using Microsoft.Azure; // Namespace for Azure Configuration Manager
using Microsoft.WindowsAzure.Storage; // Namespace for Storage Client Library
using Microsoft.WindowsAzure.Storage.Blob; // Namespace for Azure Blobs
using Microsoft.WindowsAzure.Storage.File; // Namespace for Azure File storage
```

[!INCLUDE [storage-cloud-configuration-manager-include](../../includes/storage-cloud-configuration-manager-include.md)]

## <a name="access-the-file-share-programmatically"></a>Programový přístup ke sdílené složce
Dál přidejte následující kód k metodě `Main()` (po výše uvedeném kódu) pro získání připojovacího řetězce. Tento kód získá odkaz na soubor, který jsme vytvořili předtím, a vypíše obsah do okna konzoly.

```csharp
// Create a CloudFileClient object for credentialed access to Azure File storage.
CloudFileClient fileClient = storageAccount.CreateCloudFileClient();

// Get a reference to the file share we created previously.
CloudFileShare share = fileClient.GetShareReference("logs");

// Ensure that the share exists.
if (share.Exists())
{
    // Get a reference to the root directory for the share.
    CloudFileDirectory rootDir = share.GetRootDirectoryReference();

    // Get a reference to the directory we created previously.
    CloudFileDirectory sampleDir = rootDir.GetDirectoryReference("CustomLogs");

    // Ensure that the directory exists.
    if (sampleDir.Exists())
    {
        // Get a reference to the file we created previously.
        CloudFile file = sampleDir.GetFileReference("Log1.txt");

        // Ensure that the file exists.
        if (file.Exists())
        {
            // Write the contents of the file to the console window.
            Console.WriteLine(file.DownloadTextAsync().Result);
        }
    }
}
```

Výstup zobrazíte spuštěním konzolové aplikace.

## <a name="set-the-maximum-size-for-a-file-share"></a>Nastavení maximální velikosti sdílené složky
Klientská knihovna pro úložiště Azure od verze 5.x umožňuje nastavit kvótu (maximální velikost) sdílené složky v gigabajtech. Můžete se taky podívat, kolik data je aktuálně uloženo ve sdílené složce.

Pokud nastavíte kvótu sdílené složky, můžete omezit celkovou velikost souborů uložených ve sdílené složce. Pokud celková velikost souborů ve sdílené složce překročí kvótu nastavenou pro sdílenou složku, klienti nebudou moct zvyšovat velikost existujících souborů, s výjimkou situace, když je velikost souborů nulová.

Dole uvedený příklad ukazuje, jak můžete zkontrolovat aktuální využití sdílené složky a jak nastavit kvótu pro sdílenou složku.

```csharp
// Parse the connection string for the storage account.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    Microsoft.Azure.CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create a CloudFileClient object for credentialed access to Azure File storage.
CloudFileClient fileClient = storageAccount.CreateCloudFileClient();

// Get a reference to the file share we created previously.
CloudFileShare share = fileClient.GetShareReference("logs");

// Ensure that the share exists.
if (share.Exists())
{
    // Check current usage stats for the share.
    // Note that the ShareStats object is part of the protocol layer for the File service.
    Microsoft.WindowsAzure.Storage.File.Protocol.ShareStats stats = share.GetStats();
    Console.WriteLine("Current share usage: {0} GB", stats.Usage.ToString());

    // Specify the maximum size of the share, in GB.
    // This line sets the quota to be 10 GB greater than the current usage of the share.
    share.Properties.Quota = 10 + stats.Usage;
    share.SetProperties();

    // Now check the quota for the share. Call FetchAttributes() to populate the share's properties.
    share.FetchAttributes();
    Console.WriteLine("Current share quota: {0} GB", share.Properties.Quota);
}
```

### <a name="generate-a-shared-access-signature-for-a-file-or-file-share"></a>Vygenerování sdíleného přístupového podpisu pro soubor nebo sdílenou složku
Klientská knihovna pro úložiště Azure od verze 5.x umožňuje vygenerovat sdílený přístupový podpis (SAS) pro sdílenou složku nebo konkrétní soubor. Můžete taky vytvořit sdílené zásady přístupu pro sdílenou složku ke správě sdílených přístupových podpisů. Vytvoření sdílených zásad přístupu se doporučuje, protože se tím nabízí způsob odvolání SAS v případě narušení jeho integrity nebo důvěryhodnosti.

V následujícím příkladu se vytvoří sdílená zásada přístupu pro sdílenou složku a ta se pak použije k omezení pro SAS na souboru ve sdílené složce.

```csharp
// Parse the connection string for the storage account.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    Microsoft.Azure.CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create a CloudFileClient object for credentialed access to Azure File storage.
CloudFileClient fileClient = storageAccount.CreateCloudFileClient();

// Get a reference to the file share we created previously.
CloudFileShare share = fileClient.GetShareReference("logs");

// Ensure that the share exists.
if (share.Exists())
{
    string policyName = "sampleSharePolicy" + DateTime.UtcNow.Ticks;

    // Create a new shared access policy and define its constraints.
    SharedAccessFilePolicy sharedPolicy = new SharedAccessFilePolicy()
        {
            SharedAccessExpiryTime = DateTime.UtcNow.AddHours(24),
            Permissions = SharedAccessFilePermissions.Read | SharedAccessFilePermissions.Write
        };

    // Get existing permissions for the share.
    FileSharePermissions permissions = share.GetPermissions();

    // Add the shared access policy to the share's policies. Note that each policy must have a unique name.
    permissions.SharedAccessPolicies.Add(policyName, sharedPolicy);
    share.SetPermissions(permissions);

    // Generate a SAS for a file in the share and associate this access policy with it.
    CloudFileDirectory rootDir = share.GetRootDirectoryReference();
    CloudFileDirectory sampleDir = rootDir.GetDirectoryReference("CustomLogs");
    CloudFile file = sampleDir.GetFileReference("Log1.txt");
    string sasToken = file.GetSharedAccessSignature(null, policyName);
    Uri fileSasUri = new Uri(file.StorageUri.PrimaryUri.ToString() + sasToken);

    // Create a new CloudFile object from the SAS, and write some text to the file.
    CloudFile fileSas = new CloudFile(fileSasUri);
    fileSas.UploadText("This write operation is authenticated via SAS.");
    Console.WriteLine(fileSas.DownloadText());
}
```

Další informace o vytváření a používání sdílených přístupových podpisů najdete v tématech [Použití sdílených přístupových podpisů (SAS)](storage-dotnet-shared-access-signature-part-1.md) a [Vytvoření a použití SAS s objekty blob Azure](storage-dotnet-shared-access-signature-part-2.md).

## <a name="copy-files"></a>Kopírování souborů
Klientská knihovna pro úložiště Azure od verze 5.x umožňuje kopírovat soubor do jiného souboru, soubor do objektu nebo objekt blob do souboru. V dalších kapitolách ukážeme, jak se operace kopírovaní dají provést programově.

Pomocí nástroje AzCopy taky můžete zkopírovat jeden soubor do jiného nebo zkopírovat objekt blob do souboru a obráceně. Viz téma [Přenos dat pomocí nástroje příkazového řádku AzCopy](storage-use-azcopy.md).

> [!NOTE]
> Pokud kopírujete objekt blob do souboru nebo soubor do objektu blob, musíte použít sdílený přístupový klíč (SAS) k ověření zdroje objektu, a to i když kopírujete v rámci jednoho účtu úložiště.
> 
> 

**Kopírování souboru do jiného souboru** V následujícím příkladu se zkopíruje soubor do jiného souboru ve stejné sdílené složce. Protože tato operace kopírování kopíruje soubory ve stejném účtu úložiště, můžete pro kopírování použít ověření Sdíleným klíčem.

```csharp
// Parse the connection string for the storage account.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    Microsoft.Azure.CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create a CloudFileClient object for credentialed access to Azure File storage.
CloudFileClient fileClient = storageAccount.CreateCloudFileClient();

// Get a reference to the file share we created previously.
CloudFileShare share = fileClient.GetShareReference("logs");

// Ensure that the share exists.
if (share.Exists())
{
    // Get a reference to the root directory for the share.
    CloudFileDirectory rootDir = share.GetRootDirectoryReference();

    // Get a reference to the directory we created previously.
    CloudFileDirectory sampleDir = rootDir.GetDirectoryReference("CustomLogs");

    // Ensure that the directory exists.
    if (sampleDir.Exists())
    {
        // Get a reference to the file we created previously.
        CloudFile sourceFile = sampleDir.GetFileReference("Log1.txt");

        // Ensure that the source file exists.
        if (sourceFile.Exists())
        {
            // Get a reference to the destination file.
            CloudFile destFile = sampleDir.GetFileReference("Log1Copy.txt");

            // Start the copy operation.
            destFile.StartCopy(sourceFile);

            // Write the contents of the destination file to the console window.
            Console.WriteLine(destFile.DownloadText());
        }
    }
}
```

**Kopírování souboru do objektu blob** V následujícím příkladu se vytvoří soubor a zkopíruje se do objektu blob ve stejném účtu úložiště. V příkladu se vytvoří SAS pro zdrojový soubor a služba ho při kopírování použije k ověření přístupu ke zdrojovému souboru.

```csharp
// Parse the connection string for the storage account.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    Microsoft.Azure.CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create a CloudFileClient object for credentialed access to Azure File storage.
CloudFileClient fileClient = storageAccount.CreateCloudFileClient();

// Create a new file share, if it does not already exist.
CloudFileShare share = fileClient.GetShareReference("sample-share");
share.CreateIfNotExists();

// Create a new file in the root directory.
CloudFile sourceFile = share.GetRootDirectoryReference().GetFileReference("sample-file.txt");
sourceFile.UploadText("A sample file in the root directory.");

// Get a reference to the blob to which the file will be copied.
CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
CloudBlobContainer container = blobClient.GetContainerReference("sample-container");
container.CreateIfNotExists();
CloudBlockBlob destBlob = container.GetBlockBlobReference("sample-blob.txt");

// Create a SAS for the file that's valid for 24 hours.
// Note that when you are copying a file to a blob, or a blob to a file, you must use a SAS
// to authenticate access to the source object, even if you are copying within the same
// storage account.
string fileSas = sourceFile.GetSharedAccessSignature(new SharedAccessFilePolicy()
{
    // Only read permissions are required for the source file.
    Permissions = SharedAccessFilePermissions.Read,
    SharedAccessExpiryTime = DateTime.UtcNow.AddHours(24)
});

// Construct the URI to the source file, including the SAS token.
Uri fileSasUri = new Uri(sourceFile.StorageUri.PrimaryUri.ToString() + fileSas);

// Copy the file to the blob.
destBlob.StartCopy(fileSasUri);

// Write the contents of the file to the console window.
Console.WriteLine("Source file contents: {0}", sourceFile.DownloadText());
Console.WriteLine("Destination blob contents: {0}", destBlob.DownloadText());
```

Stejným způsobem můžete kopírovat objekt blob do souboru. Pokud je zdrojovým objektem objekt blob, vytvořte SAS k ověření přístupu k tomuto objektu blob při kopírování.

## <a name="troubleshooting-azure-file-storage-using-metrics"></a>Řešení potíží se službou Azure File Storage pomocí metrik
Azure Storage Analytics teď podporuje metriky pro službu Azure File Storage. S údaji z metriky můžete sledovat žádosti a diagnostikovat potíže.


Metriky pro službu Azure File Storage můžete povolit na webu [Azure Portal](https://portal.azure.com). Metriky taky můžete zapnout programově zavoláním operace Set File Service Properties přes REST API nebo některou z podobných operací v Klientské knihovně pro úložiště.


Následující příklad kódu ukazuje, jak můžete použít Klientskou knihovnu pro úložiště pro .NET k zapnutí metrik pro službu Azure File Storage.

Nejdříve do souboru `Program.cs` mimo těch, které jste přidali výše, přidejte následující direktivy `using`:

```csharp
using Microsoft.WindowsAzure.Storage.File.Protocol;
using Microsoft.WindowsAzure.Storage.Shared.Protocol;
```

Pamatujte, že zatímco objekty blob Azure, Tabulky Azure a Fronty Azure používají sdílený typ `ServiceProperties` v oboru názvů `Microsoft.WindowsAzure.Storage.Shared.Protocol`, Azure File Storage používá vlastní typ `FileServiceProperties` v oboru názvů `Microsoft.WindowsAzure.Storage.File.Protocol`. Aby se ale následující kód mohl zkompilovat, musí se z vašeho kódu odkazovat oba obory názvů.

```csharp
// Parse your storage connection string from your application's configuration file.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        Microsoft.Azure.CloudConfigurationManager.GetSetting("StorageConnectionString"));
// Create the File service client.
CloudFileClient fileClient = storageAccount.CreateCloudFileClient();

// Set metrics properties for File service.
// Note that the File service currently uses its own service properties type,
// available in the Microsoft.WindowsAzure.Storage.File.Protocol namespace.
fileClient.SetServiceProperties(new FileServiceProperties()
{
    // Set hour metrics
    HourMetrics = new MetricsProperties()
    {
        MetricsLevel = MetricsLevel.ServiceAndApi,
        RetentionDays = 14,
        Version = "1.0"
    },
    // Set minute metrics
    MinuteMetrics = new MetricsProperties()
    {
        MetricsLevel = MetricsLevel.ServiceAndApi,
        RetentionDays = 7,
        Version = "1.0"
    }
});

// Read the metrics properties we just set.
FileServiceProperties serviceProperties = fileClient.GetServiceProperties();
Console.WriteLine("Hour metrics:");
Console.WriteLine(serviceProperties.HourMetrics.MetricsLevel);
Console.WriteLine(serviceProperties.HourMetrics.RetentionDays);
Console.WriteLine(serviceProperties.HourMetrics.Version);
Console.WriteLine();
Console.WriteLine("Minute metrics:");
Console.WriteLine(serviceProperties.MinuteMetrics.MetricsLevel);
Console.WriteLine(serviceProperties.MinuteMetrics.RetentionDays);
Console.WriteLine(serviceProperties.MinuteMetrics.Version);
```

Podrobné pokyny, jak postupovat při řešení potíží, najdete v článku [Azure File storage Troubleshooting Article](storage-troubleshoot-file-connection-problems.md) (Azure File Storage – řešení problémů).

## <a name="next-steps"></a>Další kroky
Další informace o úložišti Azure File jsou dostupné na těchto odkazech.

### <a name="conceptual-articles-and-videos"></a>Koncepční články a videa
* [Azure File Storage: hladký cloudový souborový systém SMB pro Windows a Linux](https://azure.microsoft.com/documentation/videos/azurecon-2015-azure-files-storage-a-frictionless-cloud-smb-file-system-for-windows-and-linux/)
* [Jak používat Azure File Storage s Linuxem](storage-how-to-use-files-linux.md)

### <a name="tooling-support-for-file-storage"></a>Podpora nástrojů pro úložiště File
* [Použití Azure Powershell s Azure Storage](storage-powershell-guide-full.md)
* [Použití nástroje AzCopy s Microsoft Azure Storage](storage-use-azcopy.md)
* [Použití Azure CLI s Azure Storage](storage-azure-cli.md#create-and-manage-file-shares)
* [Řešení potíží se službou Azure File Storage](https://docs.microsoft.com/azure/storage/storage-troubleshoot-file-connection-problems)

### <a name="reference"></a>Referenční informace
* [Klientská knihovna Storage pro .NET – referenční informace](https://msdn.microsoft.com/library/azure/dn261237.aspx)
* [REST API služby File – referenční informace](http://msdn.microsoft.com/library/azure/dn167006.aspx)

### <a name="blog-posts"></a>Příspěvky na blozích
* [Úložiště Azure File je nyní dostupné pro veřejnost](https://azure.microsoft.com/blog/azure-file-storage-now-generally-available/)
* [Uvnitř Azure File Storage](https://azure.microsoft.com/blog/inside-azure-file-storage/)
* [Představujeme službu Microsoft Azure File](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/05/12/introducing-microsoft-azure-file-service.aspx)
* [Nastavení trvalých připojení ke službě Microsoft Azure File Storage](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/05/27/persisting-connections-to-microsoft-azure-files.aspx)
