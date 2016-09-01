<properties
    pageTitle="Začínáme s Azure Blob Storage (úložiště objektů) pomocí rozhraní .NET | Microsoft Azure"
    description="Ukládejte nestrukturovaná data v cloudu pomocí Azure Blob Storage (úložiště objektů)."
    services="storage"
    documentationCenter=".net"
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="07/22/2016"
    ms.author="tamram"/>


# Začínáme s úložištěm Azure Blob pomocí rozhraní .NET

[AZURE.INCLUDE [storage-selector-blob-include](../../includes/storage-selector-blob-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-blobs](../../includes/storage-try-azure-tools-blobs.md)]

## Přehled

Úložiště objektů blob v Azure je služba, která ukládá nestrukturovaná data v cloudu jako objekty nebo objekty blob. Do Blob storage se dá ukládat jakýkoli druh textu nebo binárních dat, jako je dokument, soubor médií nebo instalátor aplikace. Blob storage se také nazývá úložiště objektů.

### O tomto kurzu

Tenhle kurz ukazuje, jak napsat kód .NET pro některé běžné scénáře s využitím Úložiště objektů blob v Azure. Mezi zahrnuté scénáře patří odesílání, výpis, stahování a odstraňování objektů blob.

**Odhadovaný čas dokončení:** 45 minut

**Požadavky:**

- [Microsoft Visual Studio](https://www.visualstudio.com/en-us/visual-studio-homepage-vs.aspx)
- [Klientská knihovna pro úložiště Azure pro .NET](https://www.nuget.org/packages/WindowsAzure.Storage/)
- [Azure Configuration Manager for .NET](https://www.nuget.org/packages/Microsoft.WindowsAzure.ConfigurationManager/)
- [Účet úložiště Azure](storage-create-storage-account.md#create-a-storage-account)


[AZURE.INCLUDE [storage-dotnet-client-library-version-include](../../includes/storage-dotnet-client-library-version-include.md)]

### Další ukázky

Další příklady použití Blob Storage najdete v článku [Začínáme s Azure Blob Storage v rozhraní .NET](https://azure.microsoft.com/documentation/samples/storage-blob-dotnet-getting-started/). Můžete si stáhnout a spustit ukázkovou aplikaci nebo si prohlédnout kód na GitHubu. 


[AZURE.INCLUDE [storage-blob-concepts-include](../../includes/storage-blob-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

[AZURE.INCLUDE [storage-development-environment-include](../../includes/storage-development-environment-include.md)]

### Přidání deklarací oboru názvů

Přidejte do horní části souboru `program.cs` následující příkazy `using`:

    using Microsoft.Azure; // Namespace for CloudConfigurationManager
    using Microsoft.WindowsAzure.Storage; // Namespace for CloudStorageAccount
    using Microsoft.WindowsAzure.Storage.Blob; // Namespace for Blob storage types

### Analýza připojovacího řetězce

[AZURE.INCLUDE [storage-cloud-configuration-manager-include](../../includes/storage-cloud-configuration-manager-include.md)]

### Vytvoření klienta služby objektu blob

Třída **CloudBlobClient** umožňuje načíst kontejnery a objekty blob, které jsou uloženy v Blob storage. Tady je jeden ze způsobů, jak vytvořit klienta služby:

    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

Teď můžete napsat kód, který bude číst data z Blob storage a bude je tam také zapisovat.

## Vytvoření kontejneru

[AZURE.INCLUDE [storage-container-naming-rules-include](../../includes/storage-container-naming-rules-include.md)]

Tento příklad ukazuje, jak vytvořit kontejner, pokud ještě neexistuje:

    // Retrieve storage account from connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the blob client.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    // Retrieve a reference to a container.
    CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");

    // Create the container if it doesn't already exist.
    container.CreateIfNotExists();

Ve výchozím nastavení je nový kontejner privátní, což znamená, že ke stažení objektů blob z tohoto kontejneru musíte zadat přístupový klíč úložiště. Když chcete, aby soubory v kontejneru byly k dispozici všem uživatelům, můžete jej pomocí následujícího kódu nastavit jako veřejný:

    container.SetPermissions(
        new BlobContainerPermissions { PublicAccess = BlobContainerPublicAccessType.Blob });

Kdokoli na Internetu může vidět objekty blob ve veřejném kontejneru, upravit nebo odstranit je ale můžete jenom v případě, že máte příslušný přístupový klíč k účtu nebo sdílený přístupový podpis.

## Nahrání objektu blob do kontejneru

Úložiště objektů blob v Azure podporuje objekty blob bloku a objekty blob stránky.  Ve většině případů se jako vhodný typ k použití doporučuje objekt blob bloku.

Když chcete nahrát soubor do objektu blob bloku, získejte odkaz na kontejner a použijte ho k získání odkazu objektu blob bloku. Jakmile získáte odkaz na objekt blob, můžete k němu nahrát jakýkoli proud dat voláním metody **UploadFromStream**. Tahle operace vytvoří objekt blob, pokud už dříve neexistoval, nebo ho přepíše, pokud už existoval.

Následující příklad ukazuje, jak nahrát objekt blob do kontejneru, zároveň předpokládá, že kontejner byl již vytvořen.

    // Retrieve storage account from connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the blob client.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    // Retrieve reference to a previously created container.
    CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");

    // Retrieve reference to a blob named "myblob".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference("myblob");

    // Create or overwrite the "myblob" blob with contents from a local file.
    using (var fileStream = System.IO.File.OpenRead(@"path\myfile"))
    {
        blockBlob.UploadFromStream(fileStream);
    }

## Zobrazí seznam objektů blob v kontejneru

Pokud chcete mít seznam objektů blob v kontejneru, nejdřív získejte odkaz na kontejner. Pak můžete použít metodu kontejneru **ListBlobs** a načíst objekty blob a/nebo obsažené adresáře. Pro přístup k bohaté sadě vlastností a metod vrácené položky **IListBlobItem** musíte vysílat na objekt **CloudBlockBlob**, **CloudPageBlob** nebo **CloudBlobDirectory**.  Pokud je typ neznámý, můžete použít kontrolu typu a zjistit, na který typ vysílat.  Následující kód ukazuje, jak načíst a získat výstup URI pro každou položku v `photos` kontejneru:

    // Retrieve storage account from connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the blob client.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    // Retrieve reference to a previously created container.
    CloudBlobContainer container = blobClient.GetContainerReference("photos");

    // Loop over items within the container and output the length and URI.
    foreach (IListBlobItem item in container.ListBlobs(null, false))
    {
        if (item.GetType() == typeof(CloudBlockBlob))
        {
            CloudBlockBlob blob = (CloudBlockBlob)item;

            Console.WriteLine("Block blob of length {0}: {1}", blob.Properties.Length, blob.Uri);

        }
        else if (item.GetType() == typeof(CloudPageBlob))
        {
            CloudPageBlob pageBlob = (CloudPageBlob)item;

            Console.WriteLine("Page blob of length {0}: {1}", pageBlob.Properties.Length, pageBlob.Uri);

        }
        else if (item.GetType() == typeof(CloudBlobDirectory))
        {
            CloudBlobDirectory directory = (CloudBlobDirectory)item;

            Console.WriteLine("Directory: {0}", directory.Uri);
        }
    }

Jak se uvádí výše, můžete pojmenovat objekty blob tak, aby jejich názvy obsahovaly informace o cestě. Tím se vytvoří struktura virtuálního adresáře, kterou můžete organizovat a měnit podle potřeby jako u tradičních systémů souborů. Všimněte si, že struktura adresářů je jenom virtuální - jediné prostředky, které jsou dostupné v úložišti objektů Blob,  jsou kontejnery a objekty blob. Klientská knihovna pro úložiště nabízí objekt **CloudBlobDirectory**, který má odkazovat na virtuální adresář a zjednodušit tak práci s objekty blob, které jsou tímto způsobem uspořádány.

Můžete zvolit například následující sadu objektů blob bloku v kontejneru nazvaném `photos`:

    photo1.jpg
    2010/architecture/description.txt
    2010/architecture/photo3.jpg
    2010/architecture/photo4.jpg
    2011/architecture/photo5.jpg
    2011/architecture/photo6.jpg
    2011/architecture/description.txt
    2011/photo7.jpg

Při volání **ListBlobs** na kontejneru 'fotografie' (viz ukázka výše) se vrátí hierarchický výpis. Obsahuje oba objekty **CloudBlobDirectory** a **CloudBlockBlob**, které představují adresáře a objekty blob v kontejneru, v uvedeném pořadí. Výsledný výstup vypadá takto:

    Directory: https://<accountname>.blob.core.windows.net/photos/2010/
    Directory: https://<accountname>.blob.core.windows.net/photos/2011/
    Block blob of length 505623: https://<accountname>.blob.core.windows.net/photos/photo1.jpg


Volitelně můžete nastavit parametr **UseFlatBlobListing** metody **ListBlobs** na hodnotu **pravda**. V takovém případě bude každý objekt blob v kontejneru vrácen jako objekt **CloudBlockBlob**. Volání **ListBlobs** s vráceným plochým výpisem vypadá takhle:

    // Loop over items within the container and output the length and URI.
    foreach (IListBlobItem item in container.ListBlobs(null, true))
    {
       ...
    }

a výsledky vypadají takhle:

    Block blob of length 4: https://<accountname>.blob.core.windows.net/photos/2010/architecture/description.txt
    Block blob of length 314618: https://<accountname>.blob.core.windows.net/photos/2010/architecture/photo3.jpg
    Block blob of length 522713: https://<accountname>.blob.core.windows.net/photos/2010/architecture/photo4.jpg
    Block blob of length 4: https://<accountname>.blob.core.windows.net/photos/2011/architecture/description.txt
    Block blob of length 419048: https://<accountname>.blob.core.windows.net/photos/2011/architecture/photo5.jpg
    Block blob of length 506388: https://<accountname>.blob.core.windows.net/photos/2011/architecture/photo6.jpg
    Block blob of length 399751: https://<accountname>.blob.core.windows.net/photos/2011/photo7.jpg
    Block blob of length 505623: https://<accountname>.blob.core.windows.net/photos/photo1.jpg


## Stáhnout objekty blob

Když chcete stáhnout objekty blob, nejdřív načtěte odkaz objektu blob a potom spusťte volání metody **DownloadToStream**. Následující příklad používá metodu **DownloadToStream** k přenosu obsahu objektu blob na objekt proudu, který potom můžete zachovat trvale v místním souboru.

    // Retrieve storage account from connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the blob client.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    // Retrieve reference to a previously created container.
    CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");

    // Retrieve reference to a blob named "photo1.jpg".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference("photo1.jpg");

    // Save blob contents to a file.
    using (var fileStream = System.IO.File.OpenWrite(@"path\myfile"))
    {
        blockBlob.DownloadToStream(fileStream);
    }

Můžete také použít metodu **DownloadToStream** a stáhnout obsah objektu blob jako textový řetězec.

    // Retrieve storage account from connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the blob client.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    // Retrieve reference to a previously created container.
    CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");

    // Retrieve reference to a blob named "myblob.txt"
    CloudBlockBlob blockBlob2 = container.GetBlockBlobReference("myblob.txt");

    string text;
    using (var memoryStream = new MemoryStream())
    {
        blockBlob2.DownloadToStream(memoryStream);
        text = System.Text.Encoding.UTF8.GetString(memoryStream.ToArray());
    }

## Odstranění objektů blob

Pokud chcete odstranit objekt blob, nejdřív získejte odkaz na objekt blob a potom na něm spusťte volání metody **Odstranit**.

    // Retrieve storage account from connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the blob client.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    // Retrieve reference to a previously created container.
    CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");

    // Retrieve reference to a blob named "myblob.txt".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference("myblob.txt");

    // Delete the blob.
    blockBlob.Delete();


## Asynchronní zobrazení seznamu objektů blob na stránkách

Když provádíte výpis velkého počtu objektů blob nebo chcete mít přehled o počtu výsledků, které vrátíte v rámci jedné operace výpisu, můžete vytvořit seznam objektů blob na stránkách s výsledky. Tento příklad ukazuje, jak vracet výsledky na stránkách asynchronně tak, aby čekání na vrácení velké sady výsledků neblokovalo provádění.

Tento příklad ukazuje výpis plochého objektu blob, můžete ale také provést hierarchický výpis nastavením `useFlatBlobListing` parametru metody **ListBlobsSegmentedAsync** na `false`.

Vzhledem k tomu, že metoda ukázky volá asynchronní metodu, musí být uvedena `async` klíčovým slovem a musí vrátit objekt **Úloha**. Klíčové slovo await, určené pro metodu **ListBlobsSegmentedAsync** pozastaví spuštění metody ukázky až do dokončení úlohy vytváření seznamu.

    async public static Task ListBlobsSegmentedInFlatListing(CloudBlobContainer container)
    {
        //List blobs to the console window, with paging.
        Console.WriteLine("List blobs in pages:");

        int i = 0;
        BlobContinuationToken continuationToken = null;
        BlobResultSegment resultSegment = null;

        //Call ListBlobsSegmentedAsync and enumerate the result segment returned, while the continuation token is non-null.
        //When the continuation token is null, the last page has been returned and execution can exit the loop.
        do
        {
            //This overload allows control of the page size. You can return all remaining results by passing null for the maxResults parameter,
            //or by calling a different overload.
            resultSegment = await container.ListBlobsSegmentedAsync("", true, BlobListingDetails.All, 10, continuationToken, null, null);
            if (resultSegment.Results.Count<IListBlobItem>() > 0) { Console.WriteLine("Page {0}:", ++i); }
            foreach (var blobItem in resultSegment.Results)
            {
                Console.WriteLine("\t{0}", blobItem.StorageUri.PrimaryUri);
            }
            Console.WriteLine();

            //Get the continuation token.
            continuationToken = resultSegment.ContinuationToken;
        }
        while (continuationToken != null);
    }

## Zápis do doplňovacího objektu blob

Doplňovací objekt blob je nový typ objektu blob, představený poprvé ve verzi 5.x klientské knihovny pro úložiště Azure pro .NET. Doplňovací objekt blob je optimalizován pro operace připojení, například protokolování. Podobně jako objekt blob bloku se doplňovací objekt blob skládá z bloků, ale když chcete do doplňovacího objektu blob připojit nový blok, je připojen vždy na konec objektu blob. Existující blok v doplňovacím objektu blob se nedá aktualizovat ani odstranit. ID bloku pro doplňovací objekt blob nejsou vystavená, protože jsou určená pro objekt blob bloku.

Každý blok v doplňovacím objektu blob může mít různou velikost až do 4 MB, každý doplňovací objekt blob může obsahovat maximálně 50 000 bloků. Maximální velikost doplňovacího objektu blob je proto o něco větší než 195 GB (4 MB × 50 000 bloků).

Následující příklad vytvoří nový doplňovací objekt blob a připojí některá data pro simulaci jednoduché operace protokolování.

    //Parse the connection string for the storage account.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        Microsoft.Azure.CloudConfigurationManager.GetSetting("StorageConnectionString"));

    //Create service client for credentialed access to the Blob service.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    //Get a reference to a container.
    CloudBlobContainer container = blobClient.GetContainerReference("my-append-blobs");

    //Create the container if it does not already exist.
    container.CreateIfNotExists();

    //Get a reference to an append blob.
    CloudAppendBlob appendBlob = container.GetAppendBlobReference("append-blob.log");

    //Create the append blob. Note that if the blob already exists, the CreateOrReplace() method will overwrite it.
    //You can check whether the blob exists to avoid overwriting it by using CloudAppendBlob.Exists().
    appendBlob.CreateOrReplace();

    int numBlocks = 10;

    //Generate an array of random bytes.
    Random rnd = new Random();
    byte[] bytes = new byte[numBlocks];
    rnd.NextBytes(bytes);

    //Simulate a logging operation by writing text data and byte data to the end of the append blob.
    for (int i = 0; i < numBlocks; i++)
    {
        appendBlob.AppendText(String.Format("Timestamp: {0:u} \tLog Entry: {1}{2}",
            DateTime.UtcNow, bytes[i], Environment.NewLine));
    }

    //Read the append blob to the console window.
    Console.WriteLine(appendBlob.DownloadText());

Další informace o rozdílech mezi třemi typy objektů blob získáte v části [ Vysvětlení objektů blob bloku, objektů blob stránky a doplňovacích objektů blob](https://msdn.microsoft.com/library/azure/ee691964.aspx).

## Správa zabezpečení pro objekty blob

Úložiště Azure ve výchozím nastavení zajišťuje ochranu dat omezením přístupu k majiteli účtu, který vlastní klíče pro přístup k účtu. Když budete chtít sdílet data objektu blob na svém účtu úložiště, je důležité zajistit, aby nedošlo k ohrožení zabezpečení vašich klíčů pro přístup k účtu. Navíc můžete šifrovat data objektů blob tak, aby byl zaručen jejich zabezpečený přenos přes síť nebo úložiště Azure.

[AZURE.INCLUDE [storage-account-key-note-include](../../includes/storage-account-key-note-include.md)]

### Kontrola přístupu k datům objektu blob

Ve výchozím nastavení jsou data objektu blob ve vašem účtu úložiště dostupná pouze majiteli účtu úložiště. Ve výchozím nastavení vyžaduje ověřování požadavků na Blob storage přístupový klíč účtu. Určitá data objektu blob ale můžete zpřístupnit ostatním uživatelům. Máte dvě možnosti:

- **Anonymní přístup:** můžete nastavit kontejner nebo jeho objekty blob na veřejně dostupné pro anonymní přístup. Další informace viz [Správa anonymního přístupu pro čtení ke kontejnerům a objektům blob](storage-manage-access-to-resources.md).
- **Sdílený přístupový podpis:** Klientům můžete zajistit sdílený přístupový podpis (SAS), který poskytuje delegovaný přístup k prostředku ve vašem účtu úložiště s oprávněními, která jste zadali a v intervalu, který určíte. Další informace najdete v tématu [Sdílené přístupové podpisy: vysvětlení modelu SAS](storage-dotnet-shared-access-signature-part-1.md).

### Šifrování dat objektů blob

Úložiště Azure podporuje šifrování dat objektů blob na straně klienta i na serveru:

- **Šifrování na straně klienta:** Klientská knihovna pro úložiště pro .NET podporuje šifrování dat v rámci klientské aplikace před nahráním do úložiště Azure a dešifrování dat při stahování do klienta. Knihovna také podporuje integraci se službou Azure Key Vault pro správu klíčů účtu úložiště. Další informace viz [Šifrování na straně klienta s .NET pro úložiště Microsoft Azure](storage-client-side-encryption.md). Viz také [Kurz: Šifrování a dešifrování objektů blob v úložišti Microsoft Azure pomocí služby Azure Key Vault](storage-encrypt-decrypt-blobs-key-vault.md).
- **Šifrování na straně serveru**: Úložiště Azure nyní podporuje šifrování na straně serveru. Viz [Šifrování služby Azure Storage Service pro Neaktivní uložená data (Náhled)](storage-service-encryption.md). 

## Další kroky

Teď, když jste se naučili základy používání Blob storage, podívejte se na následující odkazy a získejte další informace.

### Microsoft Azure Storage Explorer
- [Microsoft Azure Storage Explorer (MASE)](../vs-azure-tools-storage-manage-with-storage-explorer.md) je bezplatná samostatná aplikace od Microsoftu, která umožňuje vizuálně pracovat s daty Azure Storage ve Windows, OS X a Linuxu.

### Ukázky Blob Storage

- [Začínáme s úložištěm Azure Blob Storage pomocí rozhraní .NET](https://azure.microsoft.com/documentation/samples/storage-blob-dotnet-getting-started/)

### Odkazy Blob storage

- [Klientská knihovna pro úložiště pro .NET – referenční informace](http://go.microsoft.com/fwlink/?LinkID=390731&clcid=0x409)
- [REST API – referenční informace](http://msdn.microsoft.com/library/azure/dd179355)

### Koncepční vodítka

- [Přenos dat pomocí nástroje příkazového řádku AzCopy](storage-use-azcopy.md)
- [Začněte používat službu File storage for .NET](storage-dotnet-how-to-use-files.md)
- [Jak používat Úložiště objektů blob v Azure pomocí WebJobs SDK](../app-service-web/websites-dotnet-webjobs-sdk-storage-blobs-how-to.md)

  [Blob5]: ./media/storage-dotnet-how-to-use-blobs/blob5.png
  [Blob6]: ./media/storage-dotnet-how-to-use-blobs/blob6.png
  [Blob7]: ./media/storage-dotnet-how-to-use-blobs/blob7.png
  [Blob8]: ./media/storage-dotnet-how-to-use-blobs/blob8.png
  [Blob9]: ./media/storage-dotnet-how-to-use-blobs/blob9.png

  [Blog týmu Azure Storage]: http://blogs.msdn.com/b/windowsazurestorage/
  [Konfigurace připojovacích řetězců]: http://msdn.microsoft.com/library/azure/ee758697.aspx
  [Klientská knihovna pro .NET – referenční informace]: http://go.microsoft.com/fwlink/?LinkID=390731&clcid=0x409
  [REST API – referenční informace]: http://msdn.microsoft.com/library/azure/dd179355



<!--HONumber=Aug16_HO4-->


