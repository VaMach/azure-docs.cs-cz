---
title: "Azure rychlý start - objekty přenosu do nebo z Azure Blob storage pomocí rozhraní .NET | Microsoft Docs"
description: "Naučte se rychle převést objekty do a z Azure Blob storage pomocí rozhraní .NET"
services: storage
documentationcenter: storage
author: robinsh
manager: timlt
editor: tysonn
ms.assetid: 
ms.custom: mvc
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 08/01/2017
ms.author: robinsh
ms.openlocfilehash: fdba4588fbb2c46efb3fc4de1a9e53414264444a
ms.sourcegitcommit: dfd49613fce4ce917e844d205c85359ff093bb9c
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/31/2017
---
# <a name="transfer-objects-tofrom-azure-blob-storage-using-net"></a>Objekty přenosu do nebo z Azure Blob storage pomocí rozhraní .NET

V tento rychlý start zjistíte, jak používat C# .NET odesílání, stahování a seznamu objektů BLOB bloku v kontejneru v úložiště objektů Blob v Azure v systému Windows.

## <a name="prerequisites"></a>Požadavky

K provedení kroků v tomto kurzu Rychlý start je potřeba:

* Nainstalujte [Visual Studio 2017](https://www.visualstudio.com/visual-studio-homepage-vs.aspx) s následující úlohy:
    - **Azure – vývoj**

Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) před tím, než začnete.

## <a name="create-a-storage-account-using-the-azure-portal"></a>Vytvořit účet úložiště pomocí portálu Azure

Nejdřív vytvořte nový účet úložiště pro obecné účely pro tento rychlý start. 

1. Přejděte na [portál Azure](https://portal.azure.com) a přihlaste se pomocí účtu Azure. 
2. V nabídce centra vyberte **nový** > **úložiště** > **účet úložiště – objekt blob, soubor, tabulka, fronta**. 
3. Zadejte název účtu úložiště. Název musí být v rozmezí 3 až 24 znaků a může obsahovat jenom číslice a malá písmena. Také musí být jedinečný.
4. Nastavit `Deployment model` k **správce prostředků**.
5. Nastavit `Account kind` k **obecné účely**.
6. Nastavit `Performance` k **standardní**. 
7. Nastavit `Replication` k **místně redundantní úložiště (LRS)**.
8. Nastavit `Storage service encryption` k **zakázané**.
9. Nastavit `Secure transfer required` k **zakázané**.
10. Vyberte své předplatné. 
11. Pro `resource group`, vytvořte novou a zadat jedinečný název. 
12. Vyberte `Location` pro váš účet úložiště.
13. Zkontrolujte **připnout na řídicí panel** a klikněte na tlačítko **vytvořit** k vytvoření účtu úložiště. 

Po vytvoření účtu úložiště je připnutá na řídicí panel. Klikněte na ho otevřete. V části nastavení, klikněte na tlačítko **přístupové klíče**. Vyberte klíč a zkopírujte PŘIPOJOVACÍ řetězec do schránky a vložte ho do textového editoru pro pozdější použití.

## <a name="download-the-sample-application"></a>Stažení ukázkové aplikace

Ukázková aplikace používá v tento rychlý start je základní konzolovou aplikaci. 

Použití [git](https://git-scm.com/) stáhnout kopii aplikace na svoje vývojové prostředí. 

```bash
git clone https://github.com/Azure-Samples/storage-blobs-dotnet-quickstart.git
```

Tento příkaz provede klonování úložiště do složky místní git. Otevřete řešení sady Visual Studio, vyhledejte složku úložiště objektů BLOB dotnet quickstart, otevřete ho a dvakrát klikněte na úložiště objektů BLOB dotnet quickstart.sln. 

## <a name="configure-your-storage-connection-string"></a>Konfigurace připojovacího řetězce úložiště

V aplikaci je nutné zadat připojovací řetězec pro váš účet úložiště. Otevřete `app.config` soubor v Průzkumníku řešení v sadě Visual Studio. Najít `StorageConnectionString` položku. Pro **hodnotu**, nahraďte celou hodnotu připojovacího řetězce, jaké jste uložili z portálu Azure. Vaše `storageConnectionString` by měl vypadat podobně jako následující:

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

## <a name="run-the-sample"></a>Spustit ukázku

Tato ukázka vytvoří testovací soubor ve složce Dokumenty, odešle do úložiště objektů Blob, obsahuje seznam objektů BLOB v kontejneru a potom stáhne soubor pod novým názvem, můžete porovnat staré a nové soubory. 

Spusťte ukázku stisknutím klávesy F5. Výstup se zobrazuje v okně konzoly, který je podobný následujícímu: 

```
Temp file = C:\Users\azureuser\Documents\QuickStart_cbd5f95c-6ab8-4cbf-b8d2-a58e85d7a8e8.txt
Uploading to Blob storage as blob 'QuickStart_cbd5f95c-6ab8-4cbf-b8d2-a58e85d7a8e8.txt'
List blobs in container.
https://mystorage.blob.core.windows.net/quickstartblobs/QuickStart_cbd5f95c-6ab8-4cbf-b8d2-a58e85d7a8e8.txt
Downloading blob to C:\Users\azureuser\Documents\QuickStart_cbd5f95c-6ab8-4cbf-b8d2-a58e85d7a8e8_DOWNLOADED.txt
```

Pokud jste stisknutím libovolné klávesy, chcete-li pokračovat, odstraní kontejner úložiště a soubory. Než budete pokračovat, zkontrolujte MyDocuments dva soubory. Můžete je otevřít a zjistit, že jsou identické. Zkopírujte adresu URL pro tento objekt blob z okna konzoly a vložte ho do prohlížeče a prohlédněte obsah souboru v úložišti objektů Blob.

Můžete také použít nástroj, jako [Azure Storage Explorer](http://storageexplorer.com/?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) k prohlížení souborů v Blob storage. Azure Storage Explorer je bezplatný nástroj napříč platformami, který umožňuje zobrazit informace o účtu úložiště. 

Jakmile ověříte soubory, stisknutí libovolné klávesy ukončíte ukázku a odstranit testovací soubory. Teď, když víte, jaké ukázku, otevřete soubor Program.cs a podívejte se na kód. 

## <a name="get-references-to-the-storage-objects"></a>Získat odkazy na objekty úložiště

První věc udělat, je vytvořit odkazy na objekty používané pro přístup k a spravovat úložiště objektů Blob. Tyto objekty sestavení na sobě navzájem--používá každý další jeden v seznamu.

* Vytvoření instance **CloudStorageAccount** objekt odkazující na účet úložiště. 

* Vytvoření instance **CloudBlobClient** objekt, který odkazuje na službu objektů Blob v účtu úložiště. 

* Vytvoření instance **CloudBlobContainer** objekt, který reprezentuje kontejneru se připojujete. Kontejnery se používají k uspořádání objektů BLOB, jako jsou použít složek ve vašem počítači k uspořádání souborů.

Jakmile máte **CloudBlobContainer**, můžete vytvořit instanci **CloudBlockBlob** objekt, který odkazuje na konkrétní objekt blob, ve kterém zajímá a provést odesílání, stahování, kopírovat atd. operace.

V této části Vytvoření instance objektů, vytvořte nový kontejner a potom nastavit oprávnění na kontejner, aby objekty BLOB jsou veřejné a je přístupný pomocí jenom adresy URL. Kontejner se nazývá **quickstartblobs**. 

Tento příklad používá **CreateIfNotExists** vzhledem k tomu, že chcete vytvořit nový kontejner pokaždé, když se spustí vzorku. V produkčním prostředí, kde používáte stejný kontejner v celé aplikaci, je lepší zvykem volat pouze **CreateIfNotExists** po. Jinak vytvoření kontejneru předem, takže není nutné vytvořit v kódu.

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

## <a name="upload-blobs-to-the-container"></a>Odešlete do kontejneru objektů BLOB

Úložiště objektů blob podporuje objekty blob bloku, doplňovací objekty blob a objekty blob stránky. Objekty BLOB bloku se nejčastěji používají, a který se bude používat v tento rychlý start. 

Pokud chcete nahrát soubor do objektu blob, získáte odkaz na objekt blob v kontejneru cíl. Až budete mít odkaz na objekt blob, můžete nahrát data do něj s použitím [CloudBlockBlob.UploadFromFileAsync](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblockblob.uploadfromfileasync). Tato operace vytvoří objekt blob, pokud ještě neexistuje, nebo ho přepíše, pokud již existuje.

Ukázkový kód vytvoří místní soubor má být použit pro nahrávání a stahování, uložení souboru k odeslání jako **fileAndPath** a název objektu blob v **Název_místního_souboru**. Následující příklad odešle soubor do kontejneru s názvem **quickstartblobs**.

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

Existuje několik metod nahrávání, které můžete použít pomocí úložiště objektů Blob. Například pokud máte datový proud paměti, můžete metodu UploadFromStreamAsync spíše než UploadFromFileAsync. 

Objekty BLOB bloku mohou být jakéhokoli typu textu nebo binárních souborů. Objekty BLOB stránky se primárně používají pro soubory VHD použité pro zálohování virtuálních počítačů IaaS. Append – objekty BLOB jsou použitá pro protokolování, například když chcete zapsat do souboru a potom mít přidání další informace. Většina objekty uložené v úložišti objektů Blob jsou objekty BLOB bloku.

## <a name="list-the-blobs-in-a-container"></a>Zobrazí seznam objektů blob v kontejneru

Můžete získat seznam souborů v kontejneru pomocí [CloudBlobContainer.ListBlobsSegmentedAsync](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblobcontainer.listblobssegmentedasync). Následující kód načte seznam objektů BLOB a potom je, prochází zobrazující identifikátory URI objektů BLOB nalezené. Můžete zkopírovat identifikátor URI z příkazové okno a vložte ho do prohlížeče a prohlédněte soubor.

Pokud máte 5 000 nebo méně objektů BLOB v kontejneru, jsou v jednoho volání ListBlobsSegmentedAsync načíst všechny názvy objektů blob. Pokud máte více než 5 000 objektů BLOB v kontejneru, služba načte seznam v sadách 5 000, dokud načíst všechny názvy objektů blob. Takže prvním toto rozhraní API je volána, vrátí první 5 000 objektů blob názvy a token pokračování. Podruhé, zadejte token, služba načte další sadu názvy objektů blob a tak dále, dokud token pro pokračování je č. hodnota null, což znamená, že všechny názvy objektů blob byly získány. 

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

## <a name="download-blobs"></a>Stáhnout objekty blob

Stáhnout objekty BLOB do vašeho místního disku pomocí [CloudBlob.DownloadToFileAsync](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblob.downloadtofileasync).

Následující kód stáhne objekt blob nahráli v předchozí části, přidání příponu "_DOWNLOADED" na název objektu blob, abyste viděli oba soubory na místním disku. 

```csharp
// Download blob. In most cases, you would have to retrieve the reference
//   to cloudBlockBlob here. However, we created that reference earlier, and 
//   haven't changed the blob we're interested in, so we can reuse it. 
// First, add a _DOWNLOADED before the .txt so you can see both files in MyDocuments.
string fileAndPath2 = fileAndPath.Replace(".txt", "_DOWNLOADED.txt");
Console.WriteLine("Downloading blob to {0}", fileAndPath2);
await cloudBlockBlob.DownloadToFileAsync(fileAndPath2, FileMode.Create);
```

## <a name="clean-up-resources"></a>Vyčištění prostředků

Pokud již nepotřebujete objekty BLOB nahrát tento rychlý start, můžete odstranit celou kontejneru pomocí [CloudBlobContainer.DeleteAsync](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblobcontainer.deleteasync). Také odstraňte soubory vytvoří, pokud už nejsou potřeba.

```csharp
await cloudBlobContainer.DeleteAsync();
File.Delete(fileAndPath);
File.Delete(fileAndPath2);    
```

## <a name="next-steps"></a>Další kroky

V tento rychlý start zjistili, jak pro přenos souborů mezi místní disk a Azure Blob storage pomocí rozhraní .NET. Další informace o práci s úložišti objektů Blob, pokračujte do úložiště objektů Blob postupy.

> [!div class="nextstepaction"]
> [Postupy operace úložiště objektů BLOB](storage-dotnet-how-to-use-blobs.md)

Další informace o Storage Explorer a objekty BLOB najdete v tématu [prostředků úložiště Azure Blob spravovat pomocí Storage Exploreru](../../vs-azure-tools-storage-explorer-blobs.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).
