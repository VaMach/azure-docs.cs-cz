---
title: "Začínáme s Azure Blob Storage (úložiště objektů) pomocí rozhraní .NET | Dokumentace Microsoftu"
description: "Ukládejte nestrukturovaná data v cloudu pomocí Azure Blob Storage (úložiště objektů)."
services: storage
documentationcenter: .net
author: tamram
manager: timlt
editor: tysonn
ms.assetid: d18a8fc8-97cb-4d37-a408-a6f8107ea8b3
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 03/27/2017
ms.author: tamram
ms.openlocfilehash: 87594d2688e3cd01f5e7db8f5be8ca513969e774
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="get-started-with-azure-blob-storage-using-net"></a>Začínáme s úložištěm Azure Blob pomocí rozhraní .NET

[!INCLUDE [storage-selector-blob-include](../../../includes/storage-selector-blob-include.md)]

[!INCLUDE [storage-check-out-samples-dotnet](../../../includes/storage-check-out-samples-dotnet.md)]

Úložiště objektů blob v Azure je služba, která ukládá nestrukturovaná data v cloudu jako objekty nebo objekty blob. Do Blob storage se dá ukládat jakýkoli druh textu nebo binárních dat, jako je dokument, soubor médií nebo instalátor aplikace. Blob storage se také nazývá úložiště objektů.

### <a name="about-this-tutorial"></a>O tomto kurzu
Tenhle kurz ukazuje, jak napsat kód .NET pro některé běžné scénáře s využitím Úložiště objektů blob v Azure. Mezi zahrnuté scénáře patří odesílání, výpis, stahování a odstraňování objektů blob.

**Požadavky:**

* [Microsoft Visual Studio](https://www.visualstudio.com/)
* [Klientská knihovna Azure Storage pro .NET](https://www.nuget.org/packages/WindowsAzure.Storage/)
* [Azure Configuration Manager for .NET](https://www.nuget.org/packages/Microsoft.WindowsAzure.ConfigurationManager/)
* [Účet úložiště Azure](../common/storage-create-storage-account.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json#create-a-storage-account)

[!INCLUDE [storage-dotnet-client-library-version-include](../../../includes/storage-dotnet-client-library-version-include.md)]

### <a name="more-samples"></a>Další ukázky
Další příklady použití Blob Storage najdete v článku [Začínáme s Azure Blob Storage v rozhraní .NET](https://azure.microsoft.com/documentation/samples/storage-blob-dotnet-getting-started/). Můžete si stáhnout a spustit ukázkovou aplikaci nebo si prohlédnout kód na GitHubu.

[!INCLUDE [storage-blob-concepts-include](../../../includes/storage-blob-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../../includes/storage-create-account-include.md)]

[!INCLUDE [storage-development-environment-include](../../../includes/storage-development-environment-include.md)]

### <a name="add-using-directives"></a>Přidání direktiv using
Na začátek souboru `Program.cs` přidejte následující direktivy **using**:

```csharp
using Microsoft.WindowsAzure; // Namespace for CloudConfigurationManager
using Microsoft.WindowsAzure.Storage; // Namespace for CloudStorageAccount
using Microsoft.WindowsAzure.Storage.Blob; // Namespace for Blob storage types
```

### <a name="parse-the-connection-string"></a>Analýza připojovacího řetězce
[!INCLUDE [storage-cloud-configuration-manager-include](../../../includes/storage-cloud-configuration-manager-include.md)]

### <a name="create-the-blob-service-client"></a>Vytvoření klienta služby objektu blob
Třída **CloudBlobClient** umožňuje načíst kontejnery a objekty blob, které jsou uloženy v Blob storage. Tady je jeden ze způsobů, jak vytvořit klienta služby:

```csharp
CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
```
Teď můžete napsat kód, který bude číst data z Blob storage a bude je tam také zapisovat.

## <a name="create-a-container"></a>Vytvoření kontejneru
[!INCLUDE [storage-container-naming-rules-include](../../../includes/storage-container-naming-rules-include.md)]

Tento příklad ukazuje, jak vytvořit kontejner, pokud ještě neexistuje:

```csharp
// Retrieve storage account from connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create the blob client.
CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

// Retrieve a reference to a container.
CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");

// Create the container if it doesn't already exist.
container.CreateIfNotExists();
```

Ve výchozím nastavení je nový kontejner privátní, což znamená, že ke stažení objektů blob z tohoto kontejneru musíte zadat přístupový klíč úložiště. Když chcete, aby soubory v kontejneru byly k dispozici všem uživatelům, můžete jej pomocí následujícího kódu nastavit jako veřejný:

```csharp
container.SetPermissions(
    new BlobContainerPermissions { PublicAccess = BlobContainerPublicAccessType.Blob });
```

Kdokoli na internetu může vidět objekty blob ve veřejném kontejneru. Upravit nebo odstranit je ale můžete jenom v případě, že máte příslušný přístupový klíč k účtu nebo sdílený přístupový podpis.

## <a name="upload-a-blob-into-a-container"></a>Nahrání objektu blob do kontejneru
Úložiště objektů blob v Azure podporuje objekty blob bloku a objekty blob stránky.  Ve většině případů se jako vhodný typ k použití doporučuje objekt blob bloku.

Když chcete nahrát soubor do objektu blob bloku, získejte odkaz na kontejner a použijte ho k získání odkazu objektu blob bloku. Jakmile získáte odkaz na objekt blob, můžete k němu nahrát jakýkoli proud dat voláním metody **UploadFromStream**. Tahle operace vytvoří objekt blob, pokud ještě dříve neexistoval, nebo ho přepíše, pokud už existoval.

Následující příklad ukazuje, jak nahrát objekt blob do kontejneru, zároveň předpokládá, že kontejner byl již vytvořen.

```csharp
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
```

## <a name="list-the-blobs-in-a-container"></a>Zobrazí seznam objektů blob v kontejneru
Pokud chcete mít seznam objektů blob v kontejneru, nejdřív získejte odkaz na kontejner. Pak můžete použít metodu kontejneru **ListBlobs** a načíst objekty blob a/nebo obsažené adresáře. Pro přístup k bohaté sadě vlastností a metod vrácené položky **IListBlobItem** musíte vysílat na objekt **CloudBlockBlob**, **CloudPageBlob** nebo **CloudBlobDirectory**. Pokud je typ neznámý, můžete použít kontrolu typu a zjistit, na který typ vysílat. Následující kód ukazuje, jak načíst a na výstupu zobrazit identifikátor URI pro každou položku v kontejneru _photos_:

```csharp
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
```

Zahrnutím informací o cestě do názvu objektu blob můžete vytvořit virtuální adresářovou strukturu, kterou můžete organizovat a procházet podle potřeby jako u tradičních systémů souborů. Adresářová struktura je jenom virtuální – jediné prostředky, které jsou dostupné v Blob Storage, jsou kontejnery a objekty blob. Klientská knihovna pro úložiště nabízí objekt **CloudBlobDirectory**, který má odkazovat na virtuální adresář a zjednodušit tak práci s objekty blob, které jsou tímto způsobem uspořádány.

Můžete zvolit například následující sadu objektů blob bloku v kontejneru s názvem *photos*:

```
photo1.jpg
2010/architecture/description.txt
2010/architecture/photo3.jpg
2010/architecture/photo4.jpg
2011/architecture/photo5.jpg
2011/architecture/photo6.jpg
2011/architecture/description.txt
2011/photo7.jpg
```

Při volání **ListBlobs** na kontejneru *photos* (viz předchozí fragment kódu) se vrátí hierarchický výpis. Obsahuje oba objekty **CloudBlobDirectory** a **CloudBlockBlob**, které představují adresáře a objekty blob v kontejneru, v uvedeném pořadí. Výsledný výstup vypadá takto:

```
Directory: https://<accountname>.blob.core.windows.net/photos/2010/
Directory: https://<accountname>.blob.core.windows.net/photos/2011/
Block blob of length 505623: https://<accountname>.blob.core.windows.net/photos/photo1.jpg
```

Volitelně můžete nastavit parametr **UseFlatBlobListing** metody **ListBlobs** na hodnotu **true**. V takovém případě bude každý objekt blob v kontejneru vrácen jako objekt **CloudBlockBlob**. Volání **ListBlobs** s vráceným plochým výpisem vypadá takhle:

```csharp
// Loop over items within the container and output the length and URI.
foreach (IListBlobItem item in container.ListBlobs(null, true))
{
    ...
}
```

a výsledky vypadají takhle:

```
Block blob of length 4: https://<accountname>.blob.core.windows.net/photos/2010/architecture/description.txt
Block blob of length 314618: https://<accountname>.blob.core.windows.net/photos/2010/architecture/photo3.jpg
Block blob of length 522713: https://<accountname>.blob.core.windows.net/photos/2010/architecture/photo4.jpg
Block blob of length 4: https://<accountname>.blob.core.windows.net/photos/2011/architecture/description.txt
Block blob of length 419048: https://<accountname>.blob.core.windows.net/photos/2011/architecture/photo5.jpg
Block blob of length 506388: https://<accountname>.blob.core.windows.net/photos/2011/architecture/photo6.jpg
Block blob of length 399751: https://<accountname>.blob.core.windows.net/photos/2011/photo7.jpg
Block blob of length 505623: https://<accountname>.blob.core.windows.net/photos/photo1.jpg
```

## <a name="download-blobs"></a>Stáhnout objekty blob
Když chcete stáhnout objekty blob, nejdřív načtěte odkaz objektu blob a potom spusťte volání metody **DownloadToStream**. Následující příklad používá metodu **DownloadToStream** k přenosu obsahu objektu blob na objekt proudu, který potom můžete zachovat trvale v místním souboru.

```csharp
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
```

Můžete také použít metodu **DownloadToStream** a stáhnout obsah objektu blob jako textový řetězec.

```csharp
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
```

## <a name="delete-blobs"></a>Odstranění objektů blob
Pokud chcete odstranit objekt blob, nejdřív získejte odkaz na objekt blob a potom na něm spusťte volání metody **Odstranit**.

```csharp
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
```

## <a name="list-blobs-in-pages-asynchronously"></a>Asynchronní zobrazení seznamu objektů blob na stránkách
Když provádíte výpis velkého počtu objektů blob nebo chcete mít přehled o počtu výsledků, které vrátíte v rámci jedné operace výpisu, můžete vytvořit seznam objektů blob na stránkách s výsledky. Tento příklad ukazuje, jak vracet výsledky na stránkách asynchronně tak, aby čekání na vrácení velké sady výsledků neblokovalo provádění.

Tento příklad ukazuje plochý výpis objektu blob, můžete ale také provést hierarchický výpis nastavením parametru _useFlatBlobListing_ metody **ListBlobsSegmentedAsync** na _false_.

Vzhledem k tomu, že metoda ukázky volá asynchronní metodu, musí být uvedena klíčovým slovem _async_ a musí vrátit objekt **Úloha**. Klíčové slovo await, určené pro metodu **ListBlobsSegmentedAsync** pozastaví spuštění metody ukázky až do dokončení úlohy vytváření seznamu.

```csharp
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
```

## <a name="writing-to-an-append-blob"></a>Zápis do doplňovacího objektu blob
Doplňovací objekt blob je optimalizován pro operace připojení, například protokolování. Podobně jako objekt blob bloku se doplňovací objekt blob skládá z bloků, ale když chcete do doplňovacího objektu blob připojit nový blok, je připojen vždy na konec objektu blob. Existující blok v doplňovacím objektu blob se nedá aktualizovat ani odstranit. ID bloku pro doplňovací objekt blob nejsou vystavená, protože jsou určená pro objekt blob bloku.

Každý blok v doplňovacím objektu blob může mít různou velikost až do 4 MB, každý doplňovací objekt blob může obsahovat maximálně 50 000 bloků. Maximální velikost doplňovacího objektu blob je proto o něco větší než 195 GB (4 MB × 50 000 bloků).

Následující příklad vytvoří nový doplňovací objekt blob a připojí některá data pro simulaci jednoduché operace protokolování.

```csharp
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
```

Další informace o rozdílech mezi třemi typy objektů blob získáte v části [ Vysvětlení objektů blob bloku, objektů blob stránky a doplňovacích objektů blob](/rest/api/storageservices/Understanding-Block-Blobs--Append-Blobs--and-Page-Blobs).

## <a name="managing-security-for-blobs"></a>Správa zabezpečení pro objekty blob
Úložiště Azure ve výchozím nastavení zajišťuje ochranu dat omezením přístupu k majiteli účtu, který vlastní klíče pro přístup k účtu. Když budete chtít sdílet data objektu blob na svém účtu úložiště, je důležité zajistit, aby nedošlo k ohrožení zabezpečení vašich klíčů pro přístup k účtu. Navíc můžete šifrovat data objektů blob tak, aby byl zaručen jejich zabezpečený přenos přes síť nebo úložiště Azure.

[!INCLUDE [storage-account-key-note-include](../../../includes/storage-account-key-note-include.md)]

### <a name="controlling-access-to-blob-data"></a>Kontrola přístupu k datům objektu blob
Ve výchozím nastavení jsou data objektu blob ve vašem účtu úložiště dostupná pouze majiteli účtu úložiště. Ve výchozím nastavení vyžaduje ověřování požadavků na Blob storage přístupový klíč účtu. Určitá data objektu blob ale můžete zpřístupnit ostatním uživatelům. Máte dvě možnosti:

* **Anonymní přístup:** můžete nastavit kontejner nebo jeho objekty blob na veřejně dostupné pro anonymní přístup. Další informace viz [Správa anonymního přístupu pro čtení ke kontejnerům a objektům blob](storage-manage-access-to-resources.md).
* **Sdílený přístupový podpis:** Klientům můžete zajistit sdílený přístupový podpis (SAS), který poskytuje delegovaný přístup k prostředku ve vašem účtu úložiště s oprávněními, která jste zadali a v intervalu, který určíte. Další informace najdete v tématu [Použití sdílených přístupových podpisů (SAS)](../common/storage-dotnet-shared-access-signature-part-1.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).

### <a name="encrypting-blob-data"></a>Šifrování dat objektů blob
Úložiště Azure podporuje šifrování dat objektů blob na straně klienta i na serveru:

* **Šifrování na straně klienta:** Klientská knihovna pro úložiště pro .NET podporuje šifrování dat v rámci klientské aplikace před nahráním do úložiště Azure a dešifrování dat při stahování do klienta. Knihovna také podporuje integraci se službou Azure Key Vault pro správu klíčů účtu úložiště. Další informace viz [Šifrování na straně klienta s .NET pro úložiště Microsoft Azure](../common/storage-client-side-encryption.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json). Viz také [Kurz: Šifrování a dešifrování objektů blob v úložišti Microsoft Azure pomocí služby Azure Key Vault](storage-encrypt-decrypt-blobs-key-vault.md).
* **Šifrování na straně serveru**: Úložiště Azure nyní podporuje šifrování na straně serveru. Viz [Šifrování služby Azure Storage Service pro Neaktivní uložená data (Náhled)](../common/storage-service-encryption.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).

## <a name="next-steps"></a>Další kroky
Teď, když jste se naučili základy používání Blob storage, podívejte se na následující odkazy a získejte další informace.

### <a name="microsoft-azure-storage-explorer"></a>Microsoft Azure Storage Explorer
* [Microsoft Azure Storage Explorer (MASE)](../../vs-azure-tools-storage-manage-with-storage-explorer.md) je bezplatná samostatná aplikace od Microsoftu, která umožňuje vizuálně pracovat s daty Azure Storage ve Windows, macOS a Linuxu.

### <a name="blob-storage-samples"></a>Ukázky Blob Storage
* [Začínáme s úložištěm Azure Blob Storage pomocí rozhraní .NET](https://azure.microsoft.com/documentation/samples/storage-blob-dotnet-getting-started/)

### <a name="blob-storage-reference"></a>Odkazy Blob storage
* [Klientská knihovna pro úložiště pro .NET – referenční informace](https://msdn.microsoft.com/library/azure/mt347887.aspx)
* [REST API – referenční informace](/rest/api/storageservices/azure-storage-services-rest-api-reference)

### <a name="conceptual-guides"></a>Koncepční vodítka
* [Přenos dat pomocí nástroje příkazového řádku AzCopy](../common/storage-use-azcopy.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)
* [Začínáme se službou File Storage for .NET](../files/storage-dotnet-how-to-use-files.md)
* [Použití úložiště objektů blob v Azure pomocí WebJobs SDK](https://github.com/Azure/azure-webjobs-sdk/wiki)
