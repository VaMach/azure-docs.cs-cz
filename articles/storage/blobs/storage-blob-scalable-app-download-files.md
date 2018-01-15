---
title: "Stáhnout velkých objemů náhodná data ze služby Azure Storage | Microsoft Docs"
description: "Naučte se používat sadu Azure SDK můžete stáhnout velkých objemů náhodná data z účtu Azure Storage"
services: storage
documentationcenter: 
author: georgewallace
manager: jeconnoc
editor: 
ms.service: storage
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: csharp
ms.topic: tutorial
ms.date: 12/12/2017
ms.author: gwallace
ms.custom: mvc
ms.openlocfilehash: 3842860acb1c0fdd9e07f6d2f678ac5d5304003b
ms.sourcegitcommit: 48fce90a4ec357d2fb89183141610789003993d2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/12/2018
---
# <a name="download-large-amounts-of-random-data-from-azure-storage"></a>Stáhnout velkých objemů náhodná data z úložiště Azure

V tomto kurzu je součástí série, tři. Tento kurz ukazuje, jak stáhnout velkých objemů dat z úložiště Azure.

V rámci tři řady, zjistíte, jak:

> [!div class="checklist"]
> * Aktualizace aplikace
> * Spuštění aplikace
> * Ověření počet připojení

## <a name="prerequisites"></a>Požadavky

K dokončení tohoto kurzu, je nutné provést předchozí kurzu úložiště: [nahrát velkých objemů náhodná data souběžně do úložiště Azure][previous-tutorial].

## <a name="remote-into-your-virtual-machine"></a>Vzdálené do virtuálního počítače

 Chcete-li vytvořit relaci vzdálené plochy s virtuálním počítačem, použijte následující příkaz na místním počítači. Nahraďte IP adresu publicIPAddress virtuálního počítače. Po zobrazení výzvy zadejte přihlašovací údaje, které jste použili při vytváření virtuálního počítače.

```
mstsc /v:<publicIpAddress>
```

## <a name="update-the-application"></a>Aktualizace aplikace

V tomto kurzu předchozí pouze nahrát soubory k účtu úložiště. Otevřete `D:\git\storage-dotnet-perf-scale-app\Program.cs` v textovém editoru. Nahraďte `Main` metoda pomocí následující ukázky. Tento příklad komentáře mimo úlohu odeslání a uncomments stahování úloh a odstraní se obsah v účtu úložiště po dokončení úlohy.

```csharp
public static void Main(string[] args)
{
    Console.WriteLine("Azure Blob storage performance and scalability sample");
    // Set threading and default connection limit to 100 to ensure multiple threads and connections can be opened.
    // This is in addition to parallelism with the storage client library that is defined in the functions below.
    ThreadPool.SetMinThreads(100, 4);
    ServicePointManager.DefaultConnectionLimit = 100; // (Or More)

    bool exception = false;
    try
    {
        // Call the UploadFilesAsync function.
        UploadFilesAsync().GetAwaiter().GetResult();

        // Uncomment the following line to enable downloading of files from the storage account.  This is commented out
        // initially to support the tutorial at https://docs.microsoft.com/azure/storage/blobs/storage-blob-scaleable-app-download-files.
        // DownloadFilesAsync().GetAwaiter().GetResult();
    }
    catch (Exception ex)
    {
        Console.WriteLine(ex.Message);
        exception = true;
    }
    finally
    {
        // The following function will delete the container and all files contained in them.  This is commented out initialy
        // As the tutorial at https://docs.microsoft.com/azure/storage/blobs/storage-blob-scaleable-app-download-files has you upload only for one tutorial and download for the other. 
        if (!exception)
        {
            // DeleteExistingContainersAsync().GetAwaiter().GetResult();
        }
        Console.WriteLine("Press any key to exit the application");
        Console.ReadKey();
    }
}
```

Po aktualizaci aplikace, budete muset vytvořit aplikaci znovu. Otevřete `Command Prompt` a přejděte do `D:\git\storage-dotnet-perf-scale-app`. Znovu sestavte aplikaci spuštěním `dotnet build` jak je vidět v následujícím příkladu:

```
dotnet build
```

## <a name="run-the-application"></a>Spuštění aplikace

Teď, když byla znovu sestavena aplikace je čas ke spuštění aplikace s aktualizovaný kódu. Pokud už otevřený, otevřete `Command Prompt` a přejděte do `D:\git\storage-dotnet-perf-scale-app`.

Typ `dotnet run` ke spuštění aplikace.

```
dotnet run
```

Aplikace načte kontejnery v účtu úložiště, zadaný v **storageconnectionstring**. Iteruje v rámci objektů BLOB 10 čas pomocí [ListBlobsSegmented](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblobcontainer.listblobssegmented?view=azure-dotnet#Microsoft_WindowsAzure_Storage_Blob_CloudBlobContainer_ListBlobsSegmented_System_String_System_Boolean_Microsoft_WindowsAzure_Storage_Blob_BlobListingDetails_System_Nullable_System_Int32__Microsoft_WindowsAzure_Storage_Blob_BlobContinuationToken_Microsoft_WindowsAzure_Storage_Blob_BlobRequestOptions_Microsoft_WindowsAzure_Storage_OperationContext_) metoda v kontejnerech a souborů ke stažení je místní počítač pomocí [DownloadToFileAsync](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblob.downloadtofileasync?view=azure-dotnet#Microsoft_WindowsAzure_Storage_Blob_CloudBlob_DownloadToFileAsync_System_String_System_IO_FileMode_Microsoft_WindowsAzure_Storage_AccessCondition_Microsoft_WindowsAzure_Storage_Blob_BlobRequestOptions_Microsoft_WindowsAzure_Storage_OperationContext_) metoda.
Následující tabulce je zobrazena [BlobRequestOptions](/dotnet/api/microsoft.windowsazure.storage.blob.blobrequestoptions?view=azure-dotnet) , jsou definovány pro každý objekt blob během stahování.

|Vlastnost|Hodnota|Popis|
|---|---|---|
|[DisableContentMD5Validation](/dotnet/api/microsoft.windowsazure.storage.blob.blobrequestoptions.disablecontentmd5validation?view=azure-dotnet)| true (pravda)| Tato vlastnost zakáže kontrola obsah odesílaný hodnotu hash MD5. Zakázání ověřování MD5 vytváří rychlejší přenos. Ale nevyžaduje potvrzení platnosti nebo integritu přenosu souborů. |
|[StorBlobContentMD5](/dotnet/api/microsoft.windowsazure.storage.blob.blobrequestoptions.storeblobcontentmd5?view=azure-dotnet#Microsoft_WindowsAzure_Storage_Blob_BlobRequestOptions_StoreBlobContentMD5)| false (nepravda)| Tato vlastnost určuje, pokud je hodnota hash MD5 vypočítat a uložit.   |

`DownloadFilesAsync` Úloh je znázorněno v následujícím příkladu:

```csharp
private static async Task DownloadFilesAsync()
{
    CloudBlobClient blobClient = GetCloudBlobClient();

    // Define the BlobRequestionOptions on the download, including disabling MD5 hash validation for this example, this improves the download speed.
    BlobRequestOptions options = new BlobRequestOptions
    {
        DisableContentMD5Validation = true,
        StoreBlobContentMD5 = false
    };

    // Retrieve the list of containers in the storage account.  Create a directory and configure variables for use later.
    BlobContinuationToken continuationToken = null;
    List<CloudBlobContainer> containers = new List<CloudBlobContainer>();
    do
    {
        var listingResult = await blobClient.ListContainersSegmentedAsync(continuationToken);
        continuationToken = listingResult.ContinuationToken;
        containers.AddRange(listingResult.Results);
    }
    while (continuationToken != null);

    var directory = Directory.CreateDirectory("download");
    BlobResultSegment resultSegment = null;
    Stopwatch time = Stopwatch.StartNew();

    // Download the blobs
    try
    {
        List<Task> tasks = new List<Task>();
        int max_outstanding = 100;
        int completed_count = 0;

        // Create a new instance of the SemaphoreSlim class to define the number of threads to use in the application.
        SemaphoreSlim sem = new SemaphoreSlim(max_outstanding, max_outstanding);

        // Iterate through the containers
        foreach (CloudBlobContainer container in containers)
        {
            do
            {
                // Return the blobs from the container lazily 10 at a time.
                resultSegment = await container.ListBlobsSegmentedAsync(null, true, BlobListingDetails.All, 10, continuationToken, null, null);
                continuationToken = resultSegment.ContinuationToken;
                {
                    foreach (var blobItem in resultSegment.Results)
                    {

                        if (((CloudBlob)blobItem).Properties.BlobType == BlobType.BlockBlob)
                        {
                            // Get the blob and add a task to download the blob asynchronously from the storage account.
                            CloudBlockBlob blockBlob = container.GetBlockBlobReference(((CloudBlockBlob)blobItem).Name);
                            Console.WriteLine("Downloading {0} from container {1}", blockBlob.Name, container.Name);
                            await sem.WaitAsync();
                            tasks.Add(blockBlob.DownloadToFileAsync(directory.FullName + "\\" + blockBlob.Name, FileMode.Create, null, options, null).ContinueWith((t) =>
                            {
                                sem.Release();
                                Interlocked.Increment(ref completed_count);
                            }));

                        }
                    }
                }
            }
            while (continuationToken != null);
        }

        // Creates an asynchronous task that completes when all the downloads complete.
        await Task.WhenAll(tasks);
    }
    catch (Exception e)
    {
        Console.WriteLine("\nError encountered during transfer: {0}", e.Message);
    }

    time.Stop();
    Console.WriteLine("Download has been completed in {0} seconds. Press any key to continue", time.Elapsed.TotalSeconds.ToString());
    Console.ReadLine();
}
```

### <a name="validate-the-connections"></a>Ověření připojení

Během stahování souborů, můžete ověřit počet souběžných připojení k vašemu účtu úložiště. Otevřete `Command Prompt` a typ `netstat -a | find /c "blob:https"`. Tento příkaz zobrazí počet připojení, které jsou aktuálně otevřít pomocí `netstat`. Následující příklad ukazuje podobný výstup na co se zobrazí při spuštění tohoto kurzu sami. Jak je vidět z příkladu přes 280 připojení byly otevřeny při stahování náhodných soubory z účtu úložiště.

```
C:\>netstat -a | find /c "blob:https"
289

C:\>
```

## <a name="next-steps"></a>Další postup

V rámci tři řady jste se dozvěděli o stahování velkých objemů náhodná data z účtu úložiště, například jak:

> [!div class="checklist"]
> * Spuštění aplikace
> * Ověření počet připojení

Přejděte k části čtyři řady ověření metriky propustnosti a latence na portálu.

> [!div class="nextstepaction"]
> [Ověřte, propustnosti a latence metriky na portálu](storage-blob-scalable-app-verify-metrics.md)

[previous-tutorial]: storage-blob-scalable-app-upload-files.md