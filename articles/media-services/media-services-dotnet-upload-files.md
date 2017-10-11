---
title: "Nahrání souborů do účtu Media Services pomocí rozhraní .NET | Microsoft Docs"
description: "Další informace o získání mediálního obsahu ve službě Media Services pomocí vytvoření a odeslání prostředky."
services: media-services
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.assetid: c9c86380-9395-4db8-acea-507c52066f73
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/12/2017
ms.author: juliako
ms.openlocfilehash: ec8c1da633374ba684f6a0a895c542ee76ef73b8
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="upload-files-into-a-media-services-account-using-net"></a>Nahrání souborů do účtu Media Services pomocí rozhraní .NET
> [!div class="op_single_selector"]
> * [.NET](media-services-dotnet-upload-files.md)
> * [REST](media-services-rest-upload-files.md)
> * [Azure Portal](media-services-portal-upload-files.md)
> 
> 

Ve službě Media Services můžete digitální soubory nahrát (nebo ingestovat) do prostředku. **Asset** entita může obsahovat video, zvuk, obrázky, kolekci miniatur, text sleduje a titulků soubory (a metadata o těchto souborech.)  Jakmile soubory odešlete, bude váš obsah bezpečně uložen v cloudu pro další zpracování a streamování.

Soubory v prostředku se nazývají **soubory prostředku**. **AssetFile** instance a samotný mediální soubor jsou dva odlišné objekty. AssetFile instance obsahuje metadata o souboru média, zatímco souboru média obsahuje samotný mediální obsah.

> [!NOTE]
> Platí následující aspekty:
> 
> * Služba Media Services použije hodnotu vlastnosti IAssetFile.Name při sestavování adresy URL pro streamování obsah (například http://{AMSAccount}.origin.mediaservices.windows.net/{GUID}/{IAssetFile.Name}/streamingParameters.) Z tohoto důvodu není povoleno kódování v procentech. Hodnota **název** vlastnost nemůže mít žádné z následujících [procent kódování vyhrazené znaky](http://en.wikipedia.org/wiki/Percent-encoding#Percent-encoding_reserved_characters):! *' ();: @& = + $, /? % # [] ". Navíc může existovat pouze jedna '.' pro příponu názvu souboru.
> * Délka názvu nesmí být větší než 260 znaků.
> * Maximální velikost souboru podporovaná při zpracování ve službě Media Services je omezená. Podrobnosti o omezení velikosti souboru najdete [tady](media-services-quotas-and-limitations.md).
> * Je stanovený limit 1 000 000 různých zásad AMS (třeba zásady lokátoru nebo ContentKeyAuthorizationPolicy). Pokud vždy používáte stejné dny / přístupová oprávnění, například zásady pro lokátory, které mají zůstat na místě po dlouhou dobu (zásady bez odeslání), měli byste použít stejné ID zásad. Další informace najdete v [tomto](media-services-dotnet-manage-entities.md#limit-access-policies) tématu.
> 

Při vytváření prostředků, můžete zadat následující možnosti šifrování. 

* **Žádné** – nepoužívá se žádné šifrování. Toto je výchozí hodnota. Všimněte si, že při použití této možnosti není váš obsah chráněný během přenosu ani umístěná v úložišti.
  Pokud chcete pomocí progresivního stahování dodávat obsah ve formátu MP4, použijte tuto možnost. 
* **CommonEncryption** – tuto možnost použijte, pokud nahráváte obsah, který byl zašifrován a chráněný běžným šifrováním nebo DRM s technologií PlayReady (například Smooth Streaming chráněná pomocí DRM s technologií PlayReady).
* **EnvelopeEncrypted** – tuto možnost použijte, pokud odesíláte HLS se šifrováním pomocí standardu AES. Pamatujte, že soubory musí být zakódované a zašifrované pomocí správce transformací.
* **StorageEncrypted** – šifruje vaše nešifrovaného obsahu pomocí 256bitového šifrování AES 256 a odešle ji do Azure Storage kde bude uložený v zašifrované podobě. Prostředky chráněné pomocí šifrování úložiště jsou před kódováním automaticky bez šifrování umístěny do systému souborů EFS a volitelně se znovu zašifrují před jejich odesláním zpět v podobě nového výstupního prostředku. Případem primárního použití šifrování úložiště je, když chcete zabezpečit vysoké kvality souborů vstupními médii pomocí silného šifrování v klidovém stavu na disku.
  
    Služba Media Services poskytuje šifrování úložiště na disku pro vaše prostředky, ne přes přenosu jako správce digitální práv (DRM).
  
    Pokud váš asset používá šifrování úložiště, musíte nakonfigurovat zásady doručení assetu. Další informace najdete v části [konfigurace zásad doručení assetu](media-services-dotnet-configure-asset-delivery-policy.md).

Pokud zadáte pro váš asset mají být šifrována pomocí **CommonEncrypted** možnost, nebo **EnvelopeEncypted** možnost, budete muset přiřadit asset s **ContentKey**. Další informace najdete v tématu [postup vytvoření ContentKey](media-services-dotnet-create-contentkey.md). 

Pokud zadáte pro váš asset mají být šifrována pomocí **StorageEncrypted** možnost, sady Media Services SDK pro .NET vytvoří **StorateEncrypted** **ContentKey** pro vaše Asset.

Toto téma ukazuje, jak používat sadu Media Services .NET SDK, jakož i rozšíření sady Media Services .NET SDK k nahrání souborů do asset Media Services.

## <a name="upload-a-single-file-with-media-services-net-sdk"></a>Nahrát jeden soubor pomocí sady Media Services .NET SDK
Následující ukázkový kód používá .NET SDK k nahrát jeden soubor. AccessPolicy a Lokátor vytvořen a zničen odesílání funkce. 


        static public IAsset CreateAssetAndUploadSingleFile(AssetCreationOptions assetCreationOptions, string singleFilePath)
        {
            if (!File.Exists(singleFilePath))
            {
                Console.WriteLine("File does not exist.");
                return null;
            }

            var assetName = Path.GetFileNameWithoutExtension(singleFilePath);
            IAsset inputAsset = _context.Assets.Create(assetName, assetCreationOptions);

            var assetFile = inputAsset.AssetFiles.Create(Path.GetFileName(singleFilePath));

            Console.WriteLine("Upload {0}", assetFile.Name);

            assetFile.Upload(singleFilePath);
            Console.WriteLine("Done uploading {0}", assetFile.Name);

            return inputAsset;
        }


## <a name="upload-multiple-files-with-media-services-net-sdk"></a>Uložení více souborů pomocí sady Media Services .NET SDK
Následující kód ukazuje, jak vytvořit prostředek a odeslat více souborů.

Kód provede následující akce:

* Vytvoří prázdný majetku pomocí metody CreateEmptyAsset definované v předchozím kroku.
* Vytvoří **AccessPolicy** instanci, která definuje oprávnění a doba trvání přístupu pro daný prostředek.
* Vytvoří **Lokátor** instance, který poskytuje přístup k prostředku.
* Vytvoří **BlobTransferClient** instance. Tento typ reprezentuje klienta, který funguje na Azure BLOB. V tomto příkladu používáme klienta pro monitorování průběhu nahrávání. 
* Vytvoří výčet prostřednictvím souborů v adresáři zadaný a vytvoří **AssetFile** instance pro každý soubor.
* Nahrávání souborů do aplikace pomocí služby Media Services **UploadAsync** metoda. 

> [!NOTE]
> Pomocí této metody UploadAsync zajistěte, aby volání neblokují a soubory odešlete paralelně.
> 
> 

        static public IAsset CreateAssetAndUploadMultipleFiles(AssetCreationOptions assetCreationOptions, string folderPath)
        {
            var assetName = "UploadMultipleFiles_" + DateTime.UtcNow.ToString();

            IAsset asset = _context.Assets.Create(assetName, assetCreationOptions);

            var accessPolicy = _context.AccessPolicies.Create(assetName, TimeSpan.FromDays(30),
                                                                AccessPermissions.Write | AccessPermissions.List);

            var locator = _context.Locators.CreateLocator(LocatorType.Sas, asset, accessPolicy);

            var blobTransferClient = new BlobTransferClient();
            blobTransferClient.NumberOfConcurrentTransfers = 20;
            blobTransferClient.ParallelTransferThreadCount = 20;

            blobTransferClient.TransferProgressChanged += blobTransferClient_TransferProgressChanged;

            var filePaths = Directory.EnumerateFiles(folderPath);

            Console.WriteLine("There are {0} files in {1}", filePaths.Count(), folderPath);

            if (!filePaths.Any())
            {
                throw new FileNotFoundException(String.Format("No files in directory, check folderPath: {0}", folderPath));
            }

            var uploadTasks = new List<Task>();
            foreach (var filePath in filePaths)
            {
                var assetFile = asset.AssetFiles.Create(Path.GetFileName(filePath));
                Console.WriteLine("Created assetFile {0}", assetFile.Name);

                // It is recommended to validate AccestFiles before upload. 
                Console.WriteLine("Start uploading of {0}", assetFile.Name);
                uploadTasks.Add(assetFile.UploadAsync(filePath, blobTransferClient, locator, CancellationToken.None));
            }

            Task.WaitAll(uploadTasks.ToArray());
            Console.WriteLine("Done uploading the files");

            blobTransferClient.TransferProgressChanged -= blobTransferClient_TransferProgressChanged;

            locator.Delete();
            accessPolicy.Delete();

            return asset;
        }

    static void  blobTransferClient_TransferProgressChanged(object sender, BlobTransferProgressChangedEventArgs e)
    {
        if (e.ProgressPercentage > 4) // Avoid startup jitter, as the upload tasks are added.
        {
            Console.WriteLine("{0}% upload competed for {1}.", e.ProgressPercentage, e.LocalFile);
        }
    }



Při nahrávání velký počet prostředků, zvažte následující.

* Vytvořte novou **CloudMediaContext** objektu na vlákno. **CloudMediaContext** třída není bezpečná pro přístup z více vláken.
* Zvýšit NumberOfConcurrentTransfers z výchozí hodnotu 2 na vyšší hodnotu, jako je 5. Nastavení této vlastnosti ovlivní všechny instance **CloudMediaContext**. 
* Zachovat ParallelTransferThreadCount na výchozí hodnotu 10.

## <a id="ingest_in_bulk"></a>Příjem prostředky hromadně pomocí sady Media Services .NET SDK
Nahrávání souborů velké prostředek může být kritický bod během vytváření asset. Příjem prostředky v hromadné nebo "Hromadné příjem", zahrnuje vytvoření prostředku z procesu nahrávání oddělení. Pokud chcete používat hromadné příjem přístup, vytvořte manifestu (IngestManifest), který popisuje asset a jeho přidružené soubory. Potom použijte metodu nahrávání podle svého výběru k nahrání přidružené soubory do kontejneru objektů blob v manifestu. Microsoft Azure Media Services sleduje kontejneru objektů blob přidružený manifest. Po odeslání souboru do kontejneru objektů blob Microsoft Azure Media Services dokončení vytvoření prostředku, na základě konfigurace prostředku manifestu (IngestManifestAsset).

Chcete-li vytvořit nové volání IngestManifest vystavené kolekci IngestManifests na CloudMediaContext metodu Create. Tato metoda vytvoří nový IngestManifest manifestu názvem, který zadáte.

    IIngestManifest manifest = context.IngestManifests.Create(name);

Vytvořte prostředky, které budou přidruženy k hromadné IngestManifest. Konfigurujte možnosti požadované šifrování na asset pro příjem hromadně.

    // Create the assets that will be associated with this bulk ingest manifest
    IAsset destAsset1 = _context.Assets.Create(name + "_asset_1", AssetCreationOptions.None);
    IAsset destAsset2 = _context.Assets.Create(name + "_asset_2", AssetCreationOptions.None);

IngestManifestAsset přidruží hromadné IngestManifest pro příjem hromadné prostředek. Také přidruží AssetFiles, které budou použity k vytvoření každého prostředku. Pokud chcete vytvořit IngestManifestAsset, použijte metodu Create na kontext serveru.

Následující příklad ukazuje, přidávání dvě nové IngestManifestAssets, které spojují dva prostředky předtím vytvořili pro hromadným ingestování manifestu. Každý IngestManifestAsset také přidruží sadu souborů, které budou odeslány, pro každý prostředek během hromadné příjem.  

    string filename1 = _singleInputMp4Path;
    string filename2 = _primaryFilePath;
    string filename3 = _singleInputFilePath;

    IIngestManifestAsset bulkAsset1 =  manifest.IngestManifestAssets.Create(destAsset1, new[] { filename1 });
    IIngestManifestAsset bulkAsset2 =  manifest.IngestManifestAssets.Create(destAsset2, new[] { filename2, filename3 });

Můžete použít libovolná aplikace klienta vysokorychlostní schopná nahrávání souborů asset ke kontejneru úložiště objektů blob URI poskytované **IIngestManifest.BlobStorageUriForUpload** vlastnost IngestManifest. Je jedna služba nahrávání významné vysokorychlostní [Aspera na vyžádání pro aplikaci Azure](https://datamarket.azure.com/application/2cdbc511-cb12-4715-9871-c7e7fbbb82a6). Můžete taky napsat kód k nahrání souborů prostředků, jak je znázorněno v následujícím příkladu kódu.

    static void UploadBlobFile(string destBlobURI, string filename)
    {
        Task copytask = new Task(() =>
        {
            var storageaccount = new CloudStorageAccount(new StorageCredentials(_storageAccountName, _storageAccountKey), true);
            CloudBlobClient blobClient = storageaccount.CreateCloudBlobClient();
            CloudBlobContainer blobContainer = blobClient.GetContainerReference(destBlobURI);

            string[] splitfilename = filename.Split('\\');
            var blob = blobContainer.GetBlockBlobReference(splitfilename[splitfilename.Length - 1]);

            using (var stream = System.IO.File.OpenRead(filename))
                blob.UploadFromStream(stream);

            lock (consoleWriteLock)
            {
                Console.WriteLine("Upload for {0} completed.", filename);
            }
        });

        copytask.Start();
    }

V následujícím příkladu kódu se zobrazí kód pro nahrávání souborů asset pro ukázku použitým v tomto tématu.

    UploadBlobFile(manifest.BlobStorageUriForUpload, filename1);
    UploadBlobFile(manifest.BlobStorageUriForUpload, filename2);
    UploadBlobFile(manifest.BlobStorageUriForUpload, filename3);


Můžete určit průběh hromadné příjem pro všechny prostředky přidružené **IngestManifest** pomocí cyklického dotazování vlastnost statistiky **IngestManifest**. Chcete-li aktualizovat informace o průběhu, je nutné použít novou **CloudMediaContext** pokaždé, když dotazovat vlastnost statistiky.

Následující příklad ukazuje, dotazování IngestManifest podle jeho **Id**.

    static void MonitorBulkManifest(string manifestID)
    {
       bool bContinue = true;
       while (bContinue)
       {
          CloudMediaContext context = GetContext();
          IIngestManifest manifest = context.IngestManifests.Where(m => m.Id == manifestID).FirstOrDefault();

          if (manifest != null)
          {
             lock(consoleWriteLock)
             {
                Console.WriteLine("\nWaiting on all file uploads.");
                Console.WriteLine("PendingFilesCount  : {0}", manifest.Statistics.PendingFilesCount);
                Console.WriteLine("FinishedFilesCount : {0}", manifest.Statistics.FinishedFilesCount);
                Console.WriteLine("{0}% complete.\n", (float)manifest.Statistics.FinishedFilesCount / (float)(manifest.Statistics.FinishedFilesCount + manifest.Statistics.PendingFilesCount) * 100);

                if (manifest.Statistics.PendingFilesCount == 0)
                {
                   Console.WriteLine("Completed\n");
                   bContinue = false;
                }
             }

             if (manifest.Statistics.FinishedFilesCount < manifest.Statistics.PendingFilesCount)
                Thread.Sleep(60000);
          }
          else // Manifest is null
             bContinue = false;
       }
    }



## <a name="upload-files-using-net-sdk-extensions"></a>Nahrát soubory pomocí rozšíření sady SDK pro .NET
Následující příklad ukazuje, jak nahrát jeden soubor pomocí rozšíření sady SDK pro .NET. V takovém případě **CreateFromFile** metoda se používá, ale o asynchronní verzi je také k dispozici (**CreateFromFileAsync**). **CreateFromFile** metoda můžete zadat název souboru, možnost šifrování a zpětného volání za účelem hlášení průběhu odesílání souboru.

    static public IAsset UploadFile(string fileName, AssetCreationOptions options)
    {
        IAsset inputAsset = _context.Assets.CreateFromFile(
            fileName,
            options,
            (af, p) =>
            {
                Console.WriteLine("Uploading '{0}' - Progress: {1:0.##}%", af.Name, p.Progress);
            });

        Console.WriteLine("Asset {0} created.", inputAsset.Id);

        return inputAsset;
    }

V následujícím příkladu volání funkce UploadFile a určuje šifrování úložiště jako možnost vytvoření prostředku.  

    var asset = UploadFile(@"C:\VideoFiles\BigBuckBunny.mp4", AssetCreationOptions.StorageEncrypted);

## <a name="next-steps"></a>Další kroky

Nyní můžete kódovat nahrané assety. Další informace najdete v tématu [Kódování assetů](media-services-portal-encode.md).

Můžete také použít službu Azure Functions k aktivaci úlohy kódování při příchodu souboru do nakonfigurovaného kontejneru. Další informace najdete v [této ukázce](https://azure.microsoft.com/resources/samples/media-services-dotnet-functions-integration/ ).

## <a name="media-services-learning-paths"></a>Mapy kurzů ke službě Media Services
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Poskytnutí zpětné vazby
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="next-step"></a>Další krok
Teď, když jste nahráli prostředek ke službě Media Services, přejděte na [jak získat procesor médií] [ How to Get a Media Processor] tématu.

[How to Get a Media Processor]: media-services-get-media-processor.md

