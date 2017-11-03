---
title: "Začínáme s objektem blob storage a Visual Studio připojené služby (ASP.NET Core) | Microsoft Docs"
description: "Jak začít používat úložiště objektů Blob v Azure v projektu Visual Studio ASP.NET Core po vytvoření účtu úložiště pomocí sady Visual Studio připojené služby"
services: storage
documentationcenter: 
author: kraigb
manager: ghogen
editor: 
ms.assetid: 094b596a-c92c-40c4-a0f5-86407ae79672
ms.service: storage
ms.workload: web
ms.tgt_pltfrm: vs-getting-started
ms.devlang: na
ms.topic: article
ms.date: 12/02/2016
ms.author: kraigb
ms.openlocfilehash: 2e8060b44c395ad7c24e7778c0ef65148a3a45de
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="get-started-with-azure-blob-storage-and-visual-studio-connected-services-aspnet-core"></a>Začínáme s Azure Blob storage a Visual Studio připojené služby (ASP.NET Core)
[!INCLUDE [storage-try-azure-tools-blobs](../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a>Přehled
Tento článek popisuje, jak začít používat úložiště objektů Blob v Azure v sadě Visual Studio, po vytvoření a účet úložiště Azure v projektu ASP.NET Core odkazuje v dialogovém okně Visual Studio přidat připojení služby.

Azure Blob storage je služba pro ukládání velkého objemu nestrukturovaných dat, která je přístupná z kdekoliv na světě prostřednictvím protokolu HTTP nebo HTTPS. Jediného objektu blob může mít libovolnou velikost. Objekty BLOB může být třeba bitové kopie, soubory audia a videa, nezpracovaná data a soubory dokumentu. Tento článek popisuje, jak začít pracovat s úložištěm blob po vytvoření účtu úložiště Azure pomocí sady Visual Studio **přidat připojení služby** dialogové okno v projektu ASP.NET Core.

Stejně jako soubory live ve složkách, za provozu v kontejnerech objektů BLOB storage. Po vytvoření úložiště můžete vytvořit jeden nebo více kontejnerů v úložišti. Například v úložiště, který se nazývá "Výstřižků", vytvoříte kontejnerů v úložišti názvem "Image" k ukládání obrázků a jiné názvem "zvuk" k uložení zvukových souborů. Po vytvoření kontejnerů, můžete odeslat soubory jednotlivých objektů blob k nim. V tématu [Začínáme s Azure Blob storage pomocí rozhraní .NET](../storage/blobs/storage-dotnet-how-to-use-blobs.md) Další informace o programu manipulace s objekty BLOB.

## <a name="access-blob-containers-in-code"></a>Kontejnery objektů blob přístup v kódu
K programovému přístupu ke objektů BLOB v projektů ASP.NET Core, musíte přidat následující položky, pokud nejsou již existuje.

1. Přidejte následující deklarace oborů názvů kódu do horní části souboru žádné C# ve kterém chcete získat přístup prostřednictvím kódu programu úložiště Azure.
   
        using Microsoft.Extensions.Configuration;
        using Microsoft.WindowsAzure.Storage;
        using Microsoft.WindowsAzure.Storage.Blob;
        using System.Threading.Tasks;
        using LogLevel = Microsoft.Extensions.Logging.LogLevel;
2. Získání **CloudStorageAccount** objekt, který reprezentuje informace o účtu úložiště. Použijte následující kód k získání připojovacího řetězce úložiště a informace o účtu úložiště z konfigurace služby Azure.
   
         CloudStorageAccount storageAccount = new CloudStorageAccount(
            new Microsoft.WindowsAzure.Storage.Auth.StorageCredentials(
            "<storage-account-name>",
            "<access-key>"), true);
   
    **Poznámka:** používat všechny výše uvedené kódu před kód v následujících částech.
3. Použití **CloudBlobClient** objekt, který chcete získat **CloudBlobContainer** odkaz na existující kontejner ve vašem účtu úložiště.
   
        // Create a blob client.
        CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
   
        // Get a reference to a container named "mycontainer."
        CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");

## <a name="create-a-container-in-code"></a>Vytvořit kontejner v kódu
Můžete také **CloudBlobClient** vytvořit kontejner ve vašem účtu úložiště. Vše je potřeba udělat, je přidáte volání **CreateIfNotExistsAsync** jako v následujícím kódu:

    // Create a blob client.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    // Get a reference to a container named "my-new-container."
    CloudBlobContainer container = blobClient.GetContainerReference("my-new-container");

    // If "mycontainer" doesn't exist, create it.
    await container.CreateIfNotExistsAsync();


**Poznámka:** rozhraní API, které provádět volání do úložiště Azure v ASP.NET Core jsou asynchronní. V tématu [asynchronní programování s Async a Await](http://msdn.microsoft.com/library/hh191443.aspx) Další informace. Následující kód předpokládá, že asynchronní programování metody jsou používány.

Soubory v kontejneru zpřístupnit všem uživatelům, můžete nastavit kontejner, aby se veřejné pomocí následující kód.

    await container.SetPermissionsAsync(new BlobContainerPermissions
    {
        PublicAccess = BlobContainerPublicAccessType.Blob
    });

## <a name="upload-a-blob-into-a-container"></a>Nahrání objektu blob do kontejneru
Nahrát soubor objektu blob do kontejneru, získejte odkaz na kontejner a umožňuje vám získat odkaz na objekt blob. Až budete mít odkaz na objekt blob, můžete nahrát jakýkoli proud dat k němu voláním **UploadFromStreamAsync** metoda. Tato operace vytvoří objekt blob, pokud ještě není, nebo ho přepíše, pokud neexistuje. Následující příklad ukazuje, jak nahrát objekt blob do kontejneru, zároveň předpokládá, že kontejner byl již vytvořen.

    // Get a reference to a blob named "myblob".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference("myblob");

    // Create or overwrite the "myblob" blob with the contents of a local file
    // named "myfile".
    using (var fileStream = System.IO.File.OpenRead(@"path\myfile"))
    {
        await blockBlob.UploadFromStreamAsync(fileStream);
    }

## <a name="list-the-blobs-in-a-container"></a>Zobrazí seznam objektů blob v kontejneru
Pokud chcete mít seznam objektů blob v kontejneru, nejdřív získejte odkaz na kontejner. Potom můžete volat kontejneru **ListBlobsSegmentedAsync** metoda pro načtení objektů BLOB a/nebo obsažené adresáře. Pro přístup k bohaté sadě vlastností a metod vrácené položky **IListBlobItem** musíte vysílat na objekt **CloudBlockBlob**, **CloudPageBlob** nebo **CloudBlobDirectory**. Pokud neznáte typ objektu blob, můžete použít kontrolu typu zjistíte, které vysílat. Následující kód ukazuje, jak načíst a získat výstup URI pro každou položku v kontejneru.

    BlobContinuationToken token = null;
    do
    {
        BlobResultSegment resultSegment = await container.ListBlobsSegmentedAsync(token);
        token = resultSegment.ContinuationToken;

        foreach (IListBlobItem item in resultSegment.Results)
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
    } while (token != null);

Existují jiné způsoby vypíše obsah kontejneru objektů blob. V tématu [Začínáme s Azure Blob storage pomocí rozhraní .NET](../storage/blobs/storage-dotnet-how-to-use-blobs.md#list-the-blobs-in-a-container) Další informace.

## <a name="download-a-blob"></a>Stažení objektu blob
Stažení objekt blob, nejdřív získejte odkaz na objekt blob a potom zavolejte **DownloadToStreamAsync** metoda. Následující příklad používá **DownloadToStreamAsync** způsob přenosu obsahu objektu blob na objekt proudu, který pak můžete uložit jako místního souboru.

    // Get a reference to a blob named "photo1.jpg".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference("photo1.jpg");

    // Save the blob contents to a file named "myfile".
    using (var fileStream = System.IO.File.OpenWrite(@"path\myfile"))
    {
        await blockBlob.DownloadToStreamAsync(fileStream);
    }

Pro uložení objektů BLOB jako soubory i jinými způsoby. V tématu [Začínáme s Azure Blob storage pomocí rozhraní .NET](../storage/blobs/storage-dotnet-how-to-use-blobs.md#download-blobs) Další informace.

## <a name="delete-a-blob"></a>Odstranění objektu blob
Pokud chcete odstranit objekt blob, nejdřív získejte odkaz na objekt blob a potom zavolejte **DeleteAsync** metoda na něm.

    // Get a reference to a blob named "myblob.txt".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference("myblob.txt");

    // Delete the blob.
    await blockBlob.DeleteAsync();

## <a name="next-steps"></a>Další kroky
[!INCLUDE [vs-storage-dotnet-blobs-next-steps](../../includes/vs-storage-dotnet-blobs-next-steps.md)]

