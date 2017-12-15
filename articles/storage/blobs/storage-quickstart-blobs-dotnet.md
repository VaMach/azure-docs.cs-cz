---
title: "Rychlý start Azure – Přenos objektů do a z úložiště objektů blob v Azure pomocí .NET | Dokumentace Microsoftu"
description: "Rychle se naučíte, jak přenášet objekty do a z úložiště objektů blob v Azure pomocí .NET."
services: storage
documentationcenter: storage
author: tamram
manager: jeconnoc
ms.custom: mvc
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 11/10/2017
ms.author: tamram
ms.openlocfilehash: ca4cb2dea9cdd2e46c3aef042e525acdfc09de8e
ms.sourcegitcommit: 8aa014454fc7947f1ed54d380c63423500123b4a
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/23/2017
---
# <a name="transfer-objects-tofrom-azure-blob-storage-using-net"></a>Přenos objektů do a z úložiště objektů blob v Azure pomocí .NET

V tomto rychlém startu zjistíte, jak pomocí .NET nahrávat, stahovat a vypisovat objekty blob bloku v kontejneru v úložišti objektů blob v Azure.

## <a name="prerequisites"></a>Požadavky

K dokončení tohoto rychlého startu je potřeba nainstalovat sadu [Visual Studio 2017](https://www.visualstudio.com/visual-studio-homepage-vs.aspx) s následující sadou funkcí:

- **Azure – vývoj**

Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) před tím, než začnete.

[!INCLUDE [storage-quickstart-tutorial-create-account-portal](../../../includes/storage-quickstart-tutorial-create-account-portal.md)]

## <a name="download-the-sample-application"></a>Stažení ukázkové aplikace

Ukázková aplikace použitá v tomto rychlém startu je základní konzolová aplikace. 

Pomocí [gitu](https://git-scm.com/) stáhněte kopii aplikace do svého vývojového prostředí. 

```bash
git clone https://github.com/Azure-Samples/storage-blobs-dotnet-quickstart.git
```

Tento příkaz naklonuje úložiště do vaší místní složky gitu. Pokud chcete otevřít řešení sady Visual Studio, vyhledejte složku storage-blobs-dotnet-quickstart, otevřete ji a dvakrát klikněte na soubor storage-blobs-dotnet-quickstart.sln. 

## <a name="configure-your-storage-connection-string"></a>Konfigurace připojovacího řetězce úložiště

V aplikaci je potřeba zadat připojovací řetězec pro váš účet úložiště. Z Průzkumníku řešení v sadě Visual Studio otevřete soubor `app.config`. Vyhledejte položku `StorageConnectionString`. Nahraďte její **hodnotu** úplnou hodnotou připojovacího řetězce, který jste si uložili z webu Azure Portal. Váš připojovací řetězec `storageConnectionString` by měl vypadat nějak takto:

```xml
<?xml version="1.0" encoding="utf-8" ?>
<configuration>
    <startup> 
        <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.5.2" />
    </startup>
  <appSettings>
    <add key="StorageConnectionString" value="DefaultEndpointsProtocol=https;
    AccountName=<NameHere>;
    AccountKey=<KeyHere>" />
  </appSettings>
</configuration>
```

## <a name="run-the-sample"></a>Spuštění ukázky

Tato ukázka vytvoří testovací soubor ve složce Dokumenty, nahraje ho do úložiště objektů blob, vypíše objekty blob v kontejneru a pak soubor stáhne s novým názvem, abyste mohli porovnat starý a nový soubor. 

Spusťte ukázku stisknutím klávesy F5. V okně konzoly se zobrazí výstup podobný následujícímu: 

```
Temp file = C:\Users\azureuser\Documents\QuickStart_cbd5f95c-6ab8-4cbf-b8d2-a58e85d7a8e8.txt
Uploading to Blob storage as blob 'QuickStart_cbd5f95c-6ab8-4cbf-b8d2-a58e85d7a8e8.txt'
List blobs in container.
https://mystorage.blob.core.windows.net/quickstartblobs/QuickStart_cbd5f95c-6ab8-4cbf-b8d2-a58e85d7a8e8.txt
Downloading blob to C:\Users\azureuser\Documents\QuickStart_cbd5f95c-6ab8-4cbf-b8d2-a58e85d7a8e8_DOWNLOADED.txt
```

Když budete pokračovat stisknutím libovolné klávesy, odstraní se kontejner úložiště i soubory. Než budete pokračovat, zkontrolujte, jestli složka Dokumenty obsahuje příslušné dva soubory. Můžete je otevřít a podívat se, že jsou identické. Obsah souboru v úložišti objektů blob můžete zobrazit zkopírováním adresy URL objektu blob z okna konzoly a jejím vložením do prohlížeče.

K zobrazení souborů v úložišti objektů blob můžete použít také nástroj, jako je [Průzkumník služby Azure Storage](http://storageexplorer.com/?toc=%2fazure%2fstorage%2fblobs%2ftoc.json). Průzkumník služby Azure Storage je bezplatný nástroj pro více platforem, který umožňuje přístup k informacím o účtu úložiště. 

Po ověření souborů stiskněte libovolnou klávesu a dokončete ukázku a odstraňte testovací soubory. Když teď víte, co ukázka dělá, otevřete soubor Program.cs a prohlédněte si kód. 

## <a name="understand-the-sample-code"></a>Vysvětlení vzorového kódu

Dále si projdeme vzorový kód, abyste pochopili, jak funguje.

### <a name="get-references-to-the-storage-objects"></a>Získání odkazů na objekty úložiště

První věc, kterou je potřeba udělat, je vytvořit odkazy na objekty sloužící k přístupu k úložišti objektů blob a jeho správě. Tyto objekty se vzájemně využívají a každý z nich je využívaný dalším objektem v seznamu.

* Vytvořte instanci objektu **CloudStorageAccount**, která odkazuje na účet úložiště. 

* Vytvořte instanci objektu **CloudBlobClient**, která odkazuje na službu Blob ve vašem účtu úložiště. 

* Vytvořte instanci objektu **CloudBlobContainer**, která představuje kontejner, ke kterému přistupujete. Kontejnery slouží k uspořádání objektů blob podobně jako složky na počítači k uspořádání souborů.

Jakmile budete mít **CloudBlobContainer**, můžete vytvořit instanci objektu **CloudBlockBlob**, která odkazuje na konkrétní objekt blob, který vás zajímá, a provést operaci nahrání, stažení, kopírování atd.

> [!IMPORTANT]
> Názvy kontejnerů musí být malými písmeny. Další informace o pojmenování kontejnerů a objektů blob najdete v tématu [Názvy kontejnerů, objektů blob a metadat a odkazování na ně](https://docs.microsoft.com/rest/api/storageservices/naming-and-referencing-containers--blobs--and-metadata).

V této části vytvoříte instance objektů, vytvoříte nový kontejner a pak nastavíte oprávnění ke kontejneru tak, aby objekty blob byly veřejné a přístupné přes pouhou adresu URL. Kontejner má název **quickstartblobs**. 

Tento příklad používá **CreateIfNotExists**, protože chceme při každém spuštění ukázky vytvořit nový kontejner. V produkčním prostředí, kde používáte stejný kontejner v celé aplikaci, je lepší volat **CreateIfNotExists** pouze jednou. Případně můžete kontejner vytvořit předem, abyste ho nemuseli vytvářet v kódu.

```csharp
// Create a CloudStorageAccount instance pointing to your storage account.
CloudStorageAccount storageAccount =
    CloudStorageAccount.Parse(CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create the CloudBlobClient that is used to call the Blob Service for that storage account.
CloudBlobClient cloudBlobClient = storageAccount.CreateCloudBlobClient();

// Create a container called 'quickstartblobs'. 
CloudBlobContainer cloudBlobContainer = 
    cloudBlobClient.GetContainerReference("quickstartblobs");
await cloudBlobContainer.CreateIfNotExistsAsync();

// Set the permissions so the blobs are public. 
BlobContainerPermissions permissions = new BlobContainerPermissions();
permissions.PublicAccess = BlobContainerPublicAccessType.Blob;
await cloudBlobContainer.SetPermissionsAsync(permissions);
```

### <a name="upload-blobs-to-the-container"></a>Nahrání objektů blob do kontejneru

Úložiště objektů blob podporuje objekty blob bloku, doplňovací objekty blob a objekty blob stránky. Nejčastěji používané jsou objekty blob bloku, které se používají také v tomto rychlém startu. 

Pokud chcete do objektu blob nahrát soubor, získejte odkaz na objekt blob v cílovém kontejneru. Jakmile budete mít tento odkaz na objekt blob, můžete do něj nahrát data pomocí [CloudBlockBlob.UploadFromFileAsync](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblockblob.uploadfromfileasync). Tato operace vytvoří objekt blob, pokud ještě neexistuje, nebo ho přepíše, pokud už existuje.

Vzorový kód vytvoří místní soubor, který se použije k nahrání a stažení. Soubor k nahrání uloží do proměnné **fileAndPath** a název objektu blob do proměnné **localFileName**. Následující příklad nahraje soubor do kontejneru **quickstartblobs**.

```csharp
// Create a file in MyDocuments to test the upload and download.
string localPath = Environment.GetFolderPath(Environment.SpecialFolder.MyDocuments);
string localFileName = "QuickStart_" + Guid.NewGuid().ToString() + ".txt";
string fileAndPath = Path.Combine(localPath, localFileName);
File.WriteAllText(fileAndPath, "Hello, World!");

//Upload the file.
CloudBlockBlob blockBlob = container.GetBlockBlobReference(localFileName);
await blockBlob.UploadFromFileAsync(fileAndPath);
```

V případě úložiště objektů blob můžete k nahrání použít několik metod. Například pokud máte paměťový proud, můžete místo UploadFromFileAsync použít metodu UploadFromStreamAsync. 

Objekty blob bloku můžou být jakýmkoli typem textového nebo binárního souboru. Objekty blob stránky se používají hlavně pro soubory VHD využívané virtuálními počítači IaaS. Doplňovací objekty blob se používají k protokolování, například když chcete zapisovat do souboru a pak přidávat další informace. Většina objektů uložených v úložišti objektů blob je objekty blob bloku.

### <a name="list-the-blobs-in-a-container"></a>Zobrazí seznam objektů blob v kontejneru

Seznam souborů v kontejneru můžete získat pomocí [CloudBlobContainer.ListBlobsSegmentedAsync](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblobcontainer.listblobssegmentedasync). Následující kód načte seznam objektů blob, pak je ve smyčce projde a zobrazí identifikátory URI nalezených objektů blob. Soubor můžete zobrazit zkopírováním identifikátoru URI z příkazového okna a jeho vložením do prohlížeče.

Pokud máte v kontejneru 5 000 nebo méně objektů blob, všechny názvy objektů blob se načtou jedním zavoláním ListBlobsSegmentedAsync. Pokud máte v kontejneru více než 5 000 objektů blob, služba načítá seznam ve skupinách po 5 000, dokud se nenačtou názvy všech objektů blob. Takže při prvním volání rozhraní API se vrátí názvy prvních 5 000 objektů blob a token pro pokračování. Při druhém volání zadáte token a služba načte další sadu názvů objektů blob a tak dále, dokud token pro pokračování nemá hodnotu null, která indikuje, že se už načetly všechny názvy objektů blob. 

```csharp
// List the blobs in the container.
Console.WriteLine("List blobs in container.");
BlobContinuationToken blobContinuationToken = null;
do
{
    var results = await cloudBlobContainer.ListBlobsSegmentedAsync(null, blobContinuationToken);
    foreach(IListBlobItem item in results.Results)
    {
        Console.WriteLine(item.Uri);
    }
} while (blobContinuationToken != null);
```

### <a name="download-blobs"></a>Stáhnout objekty blob

Stáhněte všechny objekty blob na místní disk pomocí [CloudBlob.DownloadToFileAsync](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblob.downloadtofileasync).

Následující kód stáhne objekt blob nahraný v předchozí části a k názvu objektu blob přidá příponu „_DOWNLOADED“, takže na místním disku uvidíte oba soubory. 

```csharp
// Download blob. In most cases, you would have to retrieve the reference
//   to cloudBlockBlob here. However, we created that reference earlier, and 
//   haven't changed the blob we're interested in, so we can reuse it. 
// First, add a _DOWNLOADED before the .txt so you can see both files in MyDocuments.
string fileAndPath2 = fileAndPath.Replace(".txt", "_DOWNLOADED.txt");
Console.WriteLine("Downloading blob to {0}", fileAndPath2);
await cloudBlockBlob.DownloadToFileAsync(fileAndPath2, FileMode.Create);
```

### <a name="clean-up-resources"></a>Vyčištění prostředků

Pokud už nepotřebujete objekty blob nahrané v rámci tohoto rychlého startu, můžete celý kontejner odstranit pomocí [CloudBlobContainer.DeleteAsync](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblobcontainer.deleteasync). Odstraňte také vytvořené soubory, pokud je už nepotřebujete.

```csharp
await cloudBlobContainer.DeleteAsync();
File.Delete(fileAndPath);
File.Delete(fileAndPath2);    
```

## <a name="next-steps"></a>Další kroky

V tomto rychlém startu jste zjistili, jak přenášet soubory mezi místním diskem a úložištěm objektů blob v Azure pomocí .NET. Další informace o práci s úložištěm objektů blob najdete v postupech pro úložiště objektů blob.

> [!div class="nextstepaction"]
> [Operace s úložištěm objektů blob – postupy](storage-dotnet-how-to-use-blobs.md)

Další vzorové kódy pro Azure Storage, které si můžete stáhnout a použít, najdete v seznamu [ukázek pro Azure Storage s použitím .NET](../common/storage-samples-dotnet.md).

Další informace o Průzkumníku služby Storage a objektech blob najdete v tématu [Správa prostředků úložiště objektů blob v Azure pomocí Průzkumníka služby Storage](../../vs-azure-tools-storage-explorer-blobs.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).
