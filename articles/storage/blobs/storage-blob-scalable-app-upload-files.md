---
title: "Nahrát velkých objemů náhodná data souběžně do služby Azure Storage | Microsoft Docs"
description: "Naučte se používat sadu Azure SDK pro nahrání velkých objemů náhodná data paralelně k účtu Azure Storage"
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
ms.openlocfilehash: 98f3f69c6025d61caac20e13b573651854952432
ms.sourcegitcommit: 4256ebfe683b08fedd1a63937328931a5d35b157
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/23/2017
---
# <a name="upload-large-amounts-of-random-data-in-parallel-to-azure-storage"></a>Nahrát velkých objemů náhodná data souběžně do úložiště Azure

V tomto kurzu je součástí série, dva. Tento kurz ukazuje, že nasazujete aplikaci, která odešle velké množství náhodná data do účtu úložiště Azure.

V rámci dvě řady, zjistíte, jak:

> [!div class="checklist"]
> * Konfigurovat připojovací řetězec
> * Sestavení aplikace
> * Spuštění aplikace
> * Ověření počet připojení

Úložiště objektů blob Azure poskytuje škálovatelné službu pro ukládání dat. Aby vaše aplikace je jako původce nejdříve představu o tom, jak se doporučuje objekt blob úložiště funguje. Znalost omezení pro objekty BLOB Azure je důležité, další informace o těchto omezeních najdete: [cíle škálovatelnosti úložiště objektů blob](../common/storage-scalability-targets.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json#azure-blob-storage-scale-targets).

[Pojmenování oddílu](../common/storage-performance-checklist.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json#subheading47) je dalším důležitým faktorem při navrhování vysoce provádění aplikace pomocí objektů BLOB. Azure storage používá schéma rozdělení oddílů na základě rozsahu vyrovnávání škálování a zatížení. Tato konfigurace znamená, že soubory s podobnou zásady vytváření názvů nebo předpony přejděte do stejného oddílu. Tato logika obsahuje název kontejneru, který k odesílání souborů. V tomto kurzu použijete soubory, které mají identifikátory GUID pro názvy jako dobře jako náhodně generované obsah. Jsou poté odeslány do pěti různé kontejnery s náhodné názvy.

## <a name="prerequisites"></a>Požadavky

K dokončení tohoto kurzu, je nutné provést předchozí kurzu úložiště: [vytvořte virtuální počítač a účet úložiště pro škálovatelná aplikaci][previous-tutorial].

## <a name="remote-into-your-virtual-machine"></a>Vzdálené do virtuálního počítače

Použijte následující příkaz na místním počítači vytvořit relaci vzdálené plochy s virtuálním počítačem. Nahraďte IP adresu publicIPAddress virtuálního počítače. Po zobrazení výzvy zadejte přihlašovací údaje, které jste použili při vytváření virtuálního počítače.

```
mstsc /v:<publicIpAddress>
```

## <a name="configure-the-connection-string"></a>Konfigurovat připojovací řetězec

Na portálu Azure přejděte do účtu úložiště. Vyberte **přístupové klíče** pod **nastavení** ve vašem účtu úložiště. Kopírování **připojovací řetězec** z primární nebo sekundární klíč. Přihlaste se k virtuálnímu počítači, který jste vytvořili v předchozí kurzu. Otevřete **příkazového řádku** jako správce a spusťte `setx` s `/m` přepínače, tento příkaz uloží proměnné prostředí nastavení počítače. Proměnné prostředí není k dispozici, dokud opětovném načtení **příkazového řádku**. Nahraďte  **\<storageConnectionString\>**  následující ukázka:

```
setx storageconnectionstring "<storageConnectionString>" /m
```

Po dokončení otevřete jiné **příkazového řádku**, přejděte na `D:\git\storage-dotnet-perf-scale-app` a typ `dotnet build` pro sestavení aplikace.

## <a name="run-the-application"></a>Spuštění aplikace

Přejděte na `D:\git\storage-dotnet-perf-scale-app`.

Typ `dotnet run` ke spuštění aplikace. Při prvním spuštění `dotnet` vyplní balíček místní mezipaměť, a zvýšit rychlost obnovení a povolte přístup v režimu offline. Tento příkaz zabírají několik minut na dokončení a pouze se stane jednou.

```
dotnet run
```

Aplikace vytvoří pět náhodným názvem kontejnery a zahájí se nahrávání souborů v adresáři pracovní k účtu úložiště. Aplikace nastaví minimální počet vláken činí na 100 a [DefaultConnectionLimit](https://msdn.microsoft.com/library/system.net.servicepointmanager.defaultconnectionlimit(v=vs.110).aspx) do 100 zajistit, že velký počet souběžných připojení při spuštění aplikace.

Kromě nastavení nastavení omezení připojení a dělení na vlákna [BlobRequestOptions](/dotnet/api/microsoft.windowsazure.storage.blob.blobrequestoptions?view=azure-dotnet) pro [UploadFromStreamAsync](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblockblob.uploadfromstreamasync?view=azure-dotnet) metoda jsou nakonfigurována pro používání paralelismus a zakázat ověření hodnoty hash MD5. Soubory jsou odeslány v blocích 100 mb, tato konfigurace poskytuje lepší výkon, ale může být drahé, pokud používáte nedostatečně provádění je opakovat sítě jako, pokud dojde k selhání celý blok 100 mb.

|Vlastnost|Hodnota|Popis|
|---|---|---|
|[ParallelOperationThreadCount](/dotnet/api/microsoft.windowsazure.storage.blob.blobrequestoptions.paralleloperationthreadcount?view=azure-dotnet)| 8| Nastavení dělí do bloků blob, při nahrávání. Pro nejvyšší výkon tato hodnota by měla být 8 časy počet jader. |
|[DisableContentMD5Validation](/dotnet/api/microsoft.windowsazure.storage.blob.blobrequestoptions.disablecontentmd5validation?view=azure-dotnet)| true (pravda)| Tato vlastnost zakáže kontrola obsah odesílaný hodnotu hash MD5. Zakázání ověřování MD5 vytváří rychlejší přenos. Ale nevyžaduje potvrzení platnosti nebo integritu přenosu souborů.   |
|[StorBlobContentMD5](/dotnet/api/microsoft.windowsazure.storage.blob.blobrequestoptions.storeblobcontentmd5?view=azure-dotnet#Microsoft_WindowsAzure_Storage_Blob_BlobRequestOptions_StoreBlobContentMD5)| false (nepravda)| Tato vlastnost určuje, pokud se hodnota hash MD5 vypočítat a uloží se souborem.   |
| [RetryPolicy](/dotnet/api/microsoft.windowsazure.storage.blob.blobrequestoptions.retrypolicy?view=azure-dotnet#Microsoft_WindowsAzure_Storage_Blob_BlobRequestOptions_RetryPolicy)| omezení rychlosti 2 sekundu s 10 maximální počet opakování |Určuje zásady opakování požadavků. Počet selhání připojení jsou opakovat, v tomto příkladu [ExponentialRetry](/dotnet/api/microsoft.windowsazure.storage.retrypolicies.exponentialretry?view=azure-dotnet) zásad je nakonfigurovaná s omezení rychlosti sekundu 2 a maximální počet opakování 10. Toto nastavení je důležité, když vaše aplikace získá blízko stiskne [cíle škálovatelnosti úložiště objektů blob](../common/storage-scalability-targets.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json#azure-blob-storage-scale-targets).  |

`UploadFilesAsync` Úloh je znázorněno v následujícím příkladu:

```csharp
private static async Task UploadFilesAsync()
{
    // Create random 5 characters containers to upload files to.
    CloudBlobContainer[] containers = await GetRandomContainersAsync();
    var currentdir = System.IO.Directory.GetCurrentDirectory();

    // path to the directory to upload
    string uploadPath = currentdir + "\\upload";
    Stopwatch time = Stopwatch.StartNew();
    try
    {
        Console.WriteLine("Iterating in directory: {0}", uploadPath);
        int count = 0;
        int max_outstanding = 100;
        int completed_count = 0;

        // Define the BlobRequestionOptions on the upload.
        // This includes defining an exponential retry policy to ensure that failed connections are retried with a backoff policy. As multiple large files are being uploaded
        // large block sizes this can cause an issue if an exponential retry policy is not defined.  Additionally parallel operations are enabled with a thread count of 8
        // This could be should be multiple of the number of cores that the machine has. Lastly MD5 hash validation is disabled for this example, this improves the upload speed.
        BlobRequestOptions options = new BlobRequestOptions
        {
            ParallelOperationThreadCount = 8,
            DisableContentMD5Validation = true,
            StoreBlobContentMD5 = false
        };
        // Create a new instance of the SemaphoreSlim class to define the number of threads to use in the application.
        SemaphoreSlim sem = new SemaphoreSlim(max_outstanding, max_outstanding);

        List<Task> tasks = new List<Task>();
        Console.WriteLine("Found {0} file(s)", Directory.GetFiles(uploadPath).Count());

        // Iterate through the files
        foreach (string path in Directory.GetFiles(uploadPath))
        {
            // Create random file names and set the block size that is used for the upload.
            var container = containers[count % 5];
            string fileName = Path.GetFileName(path);
            Console.WriteLine("Uploading {0} to container {1}.", path, container.Name);
            CloudBlockBlob blockBlob = container.GetBlockBlobReference(fileName);

            // Set block size to 100MB.
            blockBlob.StreamWriteSizeInBytes = 100 * 1024 * 1024;
            await sem.WaitAsync();

            // Create tasks for each file that is uploaded. This is added to a collection that executes them all asyncronously.  
            tasks.Add(blockBlob.UploadFromFileAsync(path, null, options, null).ContinueWith((t) =>
            {
                sem.Release();
                Interlocked.Increment(ref completed_count);
            }));
            count++;
        }

        // Creates an asynchronous task that completes when all the uploads complete.
        await Task.WhenAll(tasks);

        time.Stop();

        Console.WriteLine("Upload has been completed in {0} seconds. Press any key to continue", time.Elapsed.TotalSeconds.ToString());

        Console.ReadLine();
    }
    catch (DirectoryNotFoundException ex)
    {
        Console.WriteLine("Error parsing files in the directory: {0}", ex.Message);
    }
    catch (Exception ex)
    {
        Console.WriteLine(ex.Message);
    }
}
```

V následujícím příkladu je výstupu zkrácený aplikace spuštěna v systému Windows.

```
Created container https://mystorageaccount.blob.core.windows.net/9efa7ecb-2b24-49ff-8e5b-1d25e5481076
Created container https://mystorageaccount.blob.core.windows.net/bbe5f0c8-be9e-4fc3-bcbd-2092433dbf6b
Created container https://mystorageaccount.blob.core.windows.net/9ac2f71c-6b44-40e7-b7be-8519d3ba4e8f
Created container https://mystorageaccount.blob.core.windows.net/47646f1a-c498-40cd-9dae-840f46072180
Created container https://mystorageaccount.blob.core.windows.net/38b2cdab-45fa-4cf9-94e7-d533837365aa
Iterating in directory: D:\git\storage-dotnet-perf-scale-app\upload
Found 50 file(s)
Starting upload of D:\git\storage-dotnet-perf-scale-app\upload\1d596d16-f6de-4c4c-8058-50ebd8141e4d.txt to container 9efa7ecb-2b24-49ff-8e5b-1d25e5481076.
Starting upload of D:\git\storage-dotnet-perf-scale-app\upload\242ff392-78be-41fb-b9d4-aee8152a6279.txt to container bbe5f0c8-be9e-4fc3-bcbd-2092433dbf6b.
Starting upload of D:\git\storage-dotnet-perf-scale-app\upload\38d4d7e2-acb4-4efc-ba39-f9611d0d55ef.txt to container 9ac2f71c-6b44-40e7-b7be-8519d3ba4e8f.
Starting upload of D:\git\storage-dotnet-perf-scale-app\upload\45930d63-b0d0-425f-a766-cda27ff00d32.txt to container 47646f1a-c498-40cd-9dae-840f46072180.
Starting upload of D:\git\storage-dotnet-perf-scale-app\upload\5129b385-5781-43be-8bac-e2fbb7d2bd82.txt to container 38b2cdab-45fa-4cf9-94e7-d533837365aa.
...
Upload has been completed in 142.0429536 seconds. Press any key to continue
```

### <a name="validate-the-connections"></a>Ověření připojení

Při odesílání souborů, můžete ověřit počet souběžných připojení k vašemu účtu úložiště. Otevřete **příkazového řádku** a typ `netstat -a | find /c "blob:https"`. Tento příkaz zobrazí počet připojení, které jsou aktuálně otevřít pomocí `netstat`. Následující příklad ukazuje podobný výstup na co se zobrazí při spuštění tohoto kurzu sami. Jak je vidět z příkladu 800 připojení byly otevřeny při nahrávání náhodných souborů do účtu úložiště. Tato hodnota se změní v průběhu nahrávání systémem. Tím, že nahrajete v bloku parallel bloky dat, se výrazně snižuje množství času, které jsou vyžadovány k přenosu obsahu.

```
C:\>netstat -a | find /c "blob:https"
800

C:\>
```

## <a name="next-steps"></a>Další postup

V rámci dvě řady jste se dozvěděli o nahrávání velkých objemů náhodná data na účet úložiště paralelně, například jak:

> [!div class="checklist"]
> * Konfigurovat připojovací řetězec
> * Sestavení aplikace
> * Spuštění aplikace
> * Ověření počet připojení

Přechodu na tři součástí řady stažení velkých objemů dat z účtu úložiště.

> [!div class="nextstepaction"]
> [Nahrát velkých objemů velkých souborů paralelně na účet úložiště](storage-blob-scalable-app-download-files.md)

[previous-tutorial]: storage-blob-scalable-app-create-vm.md
