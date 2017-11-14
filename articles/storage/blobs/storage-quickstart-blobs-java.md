---
title: "Azure rychlý start - objekty přenosu do nebo z Azure Blob storage pomocí Java | Microsoft Docs"
description: "Naučte se rychle převést objekty do a z Azure Blob storage pomocí Java"
author: roygara
manager: timlt
services: storage
ms.service: storage
ms.topic: quickstart
ms.date: 11/01/2017
ms.author: v-rogara
ms.custom: mvc
ms.openlocfilehash: 2a825e59093b64ab56110f9a045b0325728b39b2
ms.sourcegitcommit: e38120a5575ed35ebe7dccd4daf8d5673534626c
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/13/2017
---
# <a name="transfer-objects-tofrom-azure-blob-storage-using-java"></a>Objekty přenosu do nebo z Azure Blob storage pomocí Java

V tento rychlý start a zjistěte, jak používat Java k odesílání, stahování a seznam objektů BLOB bloku v kontejneru v úložiště objektů Blob v Azure.

## <a name="prerequisites"></a>Požadavky

K provedení kroků v tomto kurzu Rychlý start je potřeba:

* Nainstalujte rozhraní IDE, který má Maven integrace

* Můžete taky nainstalovat a nakonfigurovat Maven pracovat z příkazového řádku

Tento kurz používá [Eclipse](http://www.eclipse.org/downloads/) s konfigurací "Eclipse IDE pro vývojáře v jazyce Java".

Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) před tím, než začnete.

[!INCLUDE [storage-quickstart-tutorial-create-account-portal](../../../includes/storage-quickstart-tutorial-create-account-portal.md)]

## <a name="download-the-sample-application"></a>Stažení ukázkové aplikace

[Ukázkové aplikace](https://github.com/Azure-Samples/storage-blobs-java-quickstart) používán tento rychlý start je základní konzolovou aplikaci. 

Použití [git](https://git-scm.com/) stáhnout kopii aplikace na svoje vývojové prostředí. 

```bash
git clone https://github.com/Azure-Samples/storage-blobs-java-quickstart.git
```

Tento příkaz provede klonování úložiště do složky místní git. Otevřete projekt, spusťte Eclipse a zavřete na úvodní obrazovce. Vyberte **soubor** pak **otevřít projekty ze systému souborů...** . Zajistěte, aby **zjistit a konfigurace projektu natures** je zaškrtnuté. Vyberte **Directory** pak přejděte k uložení úložišti klonovaný uvnitř ho vyberte **javaBlobsQuickstart** složky. Zajistěte, aby **javaBlobsQuickstarts** projektu se zobrazí jako Eclipse projekt, pak vyberte **Dokončit**.

Po dokončení projektu import, otevřete **AzureApp.java** (umístěné v **blobQuickstart.blobAzureApp** uvnitř **src/main/java**) a nahraďte `accountname`a `accountkey` uvnitř `storageConnectionString` řetězec. Pak spusťte aplikaci.
     

## <a name="configure-your-storage-connection-string"></a>Konfigurace připojovacího řetězce úložiště
    
V aplikaci je nutné zadat připojovací řetězec pro váš účet úložiště. Otevřete **AzureApp.Java** souboru. Najít `storageConnectionString` proměnné. Nahraďte `AccountName` a `AccountKey` hodnoty v připojovacím řetězci s hodnotami, které jste uložili z portálu Azure. Vaše `storageConnectionString` by měl vypadat podobně jako následující:

```java
public static final String storageConnectionString =
"DefaultEndpointsProtocol=https;" +
"AccountName=<account-name>;" +
"AccountKey=<account-key>";
```

## <a name="run-the-sample"></a>Spustit ukázku

Tato ukázka vytvoří testovací soubor ve výchozím adresáři (složka Dokumenty pro uživatele systému windows), odešle do úložiště objektů Blob, obsahuje seznam objektů BLOB v kontejneru a potom stáhne soubor pod novým názvem, můžete porovnat staré a nové soubory. 

Spustit ukázku stisknutím **Ctrl + F11** v prostředí Eclipse.

Pokud chcete spustit ukázku pomocí nástroje Maven v příkazovém řádku, otevřete prostředí a přejděte do **blobAzureApp** uvnitř klonovaný adresáře. Potom zadejte `mvn compile exec:java`.

Následuje příklad výstupu Pokud byste chtěli spuštění aplikace v systému Windows.

```
Location of file: C:\Users\<user>\Documents\results.txt
File has been written
URI of blob is: http://myexamplesacct.blob.core.windows.net/quickstartblobs/results.txt
The program has completed successfully.
Press the 'Enter' key while in the console to delete the sample files, example container, and exit the application.
```

 Než budete pokračovat, zkontrolujte výchozí adresář (složka Dokumenty pro uživatele systému windows) pro dva soubory. Můžete je otevřít a zjistit, že jsou identické. Zkopírujte adresu URL pro tento objekt blob z okna konzoly a vložte ho do prohlížeče a prohlédněte obsah souboru v úložišti objektů Blob. Když stisknete klávesu enter, odstraní kontejner úložiště a soubory.

Můžete také použít nástroj, jako [Azure Storage Explorer](http://storageexplorer.com/?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) k prohlížení souborů v Blob storage. Azure Storage Explorer je bezplatný nástroj napříč platformami, který umožňuje zobrazit informace o účtu úložiště. 

Jakmile ověříte soubory, stiskněte klávesu enter a dokončit ukázku odstranit testovací soubory. Teď, když víte, jaké ukázku, otevřete **AzureApp.java** soubor podívejte se na kód. 

## <a name="understand-the-sample-code"></a>Pochopení ukázkový kód

V dalším kroku jsme provede ukázkový kód tak, aby vám pochopit, jak to funguje.

### <a name="get-references-to-the-storage-objects"></a>Získat odkazy na objekty úložiště

První věc udělat, je vytvořit odkazy na objekty používané pro přístup k a spravovat úložiště objektů Blob. Tyto objekty sestavení na sobě navzájem--používá každý další jeden v seznamu.

* Vytvoření instance **CloudStorageAccount** odkazující na objekt [účet úložiště](/java/api/com.microsoft.azure.management.storage._storage_account).

    **CloudStorageAccount** objektu je reprezentace účtu úložiště a umožňuje vám nastavit a přístup k vlastnosti účtu úložiště prostřednictvím kódu programu. Pomocí **CloudStorageAccount** vytvoříte instanci objektu **CloudBlobClient**, který je nezbytný k přístupu ke službě blob.

* Vytvoření instance **CloudBlobClient** objekt, který odkazuje na [služba objektů Blob](/java/api/com.microsoft.azure.storage.blob._cloud_blob_client) ve vašem účtu úložiště.

    **CloudBlobClient** poskytuje bod přístup ke službě blob, který vám umožní nastavit a přístup k vlastnostem úložiště objektů blob prostřednictvím kódu programu. Pomocí **CloudBlobClient** můžete vytvořit instanci **CloudBlobContainer** objekt, což je potřebné k vytvoření kontejnerů.

* Vytvoření instance **CloudBlobContainer** objekt, který reprezentuje [kontejneru](/java/api/com.microsoft.azure.storage.blob._cloud_blob_container) se připojujete. Kontejnery se používají k uspořádání objektů BLOB, jako jsou použít složek ve vašem počítači k uspořádání souborů.    

    Jakmile máte **CloudBlobContainer**, můžete vytvořit instanci **CloudBlockBlob** objekt, který odkazuje na konkrétní [objektů blob](/java/api/com.microsoft.azure.storage.blob._cloud_block_blob) v která vás zajímá, a Proveďte odesílání, stahování, kopírování a operace atd.

> [!IMPORTANT]
> Názvy kontejnerů musí být malé. V tématu [pojmenování a odkazování na kontejnerů, objektů BLOB a metadat](https://docs.microsoft.com/rest/api/storageservices/naming-and-referencing-containers--blobs--and-metadata) Další informace o názvech kontejnerů a objektů blob.

### <a name="create-a-container"></a>Vytvoření kontejneru 

V této části Vytvoření instance objektů, vytvořte nový kontejner a potom nastavit oprávnění na kontejner, aby objekty BLOB jsou veřejné a je přístupný pomocí jenom adresy URL. Kontejner se nazývá **quickstartblobs**. 

Tento příklad používá **CreateIfNotExists** vzhledem k tomu, že chcete vytvořit nový kontejner pokaždé, když se spustí vzorku. V produkčním prostředí, kde používáte stejný kontejner v celé aplikaci, je lepší zvykem volat pouze **CreateIfNotExists** po. Alternativně můžete vytvořit kontejner předem, takže není nutné vytvořit v kódu.

```java
// Parse the connection string and create a blob client to interact with Blob storage
storageAccount = CloudStorageAccount.parse(storageConnectionString);
blobClient = storageAccount.createCloudBlobClient();
container = blobClient.getContainerReference("quickstartcontainer");

// Create the container if it does not exist with public access.
System.out.println("Creating container: " + container.getName());
container.createIfNotExists(BlobContainerPublicAccessType.CONTAINER, new BlobRequestOptions(), new OperationContext());
```

### <a name="upload-blobs-to-the-container"></a>Odešlete do kontejneru objektů BLOB

Úložiště objektů blob podporuje objekty blob bloku, doplňovací objekty blob a objekty blob stránky. Objekty BLOB bloku se nejčastěji používají, a který se bude používat v tento rychlý start. 

Pokud chcete nahrát soubor do objektu blob, získáte odkaz na objekt blob v kontejneru cíl. Až budete mít odkaz na objekt blob, můžete nahrát data do něj s použitím [CloudBlockBlob.Upload](https://docs.microsoft.com/java/api/com.microsoft.azure.storage.blob._cloud_block_blob.upload#com_microsoft_azure_storage_blob__cloud_block_blob_upload_final_InputStream_final_long). Tato operace vytvoří objekt blob, pokud ještě neexistuje, nebo ho přepíše, pokud již existuje.

Ukázkový kód vytvoří místní soubor použije pro nahrávání a stahování. uložení souboru k odeslání jako **zdroj** a název objektu blob v **blob**. Následující příklad odešle soubor do kontejneru s názvem **quickstartblobs**.

```java
//Creating a sample file
sourceFile = File.createTempFile("sampleFile", ".txt");
System.out.println("Creating a sample file at: " + sourceFile.toString());
Writer output = new BufferedWriter(new FileWriter(sourceFile));
output.write("Hello Azure!");
output.close();

//Getting a blob reference
CloudBlockBlob blob = container.getBlockBlobReference(sourceFile.getName());

//Creating blob and uploading file to it
System.out.println("Uploading the sample file ");
blob.uploadFromFile(sourceFile.getAbsolutePath());
```

Existuje několik [nahrát metody](/java/api/com.microsoft.azure.storage.blob._cloud_block_blob) , můžete použít s úložiště objektů Blob. Například pokud je řetězec, můžete metodu UploadText spíše než metoda nahrávání. 

Objekty BLOB bloku mohou být jakéhokoli typu textu nebo binárních souborů. Objekty BLOB stránky se primárně používají pro soubory VHD použité pro zálohování virtuálních počítačů IaaS. Append – objekty BLOB jsou použitá pro protokolování, například když chcete zapsat do souboru a potom mít přidání další informace. Většina objekty uložené v úložišti objektů Blob jsou objekty BLOB bloku.

### <a name="list-the-blobs-in-a-container"></a>Zobrazí seznam objektů blob v kontejneru

Můžete získat seznam souborů v kontejneru pomocí [CloudBlobContainer.ListBlobs](https://docs.microsoft.com/java/api/com.microsoft.azure.storage.blob._cloud_blob_container.listblobs#com_microsoft_azure_storage_blob__cloud_blob_container_listBlobs). Následující kód načte seznam objektů BLOB a potom je, prochází zobrazující identifikátory URI objektů BLOB nalezené. Můžete zkopírovat identifikátor URI z příkazové okno a vložte ho do prohlížeče a prohlédněte soubor.

```java
//Listing contents of container
for (ListBlobItem blobItem : container.listBlobs()) {
    System.out.println("URI of blob is: " + blobItem.getUri());
}
```

### <a name="download-blobs"></a>Stáhnout objekty blob

Stáhnout objekty BLOB do vašeho místního disku pomocí [CloudBlob.DownloadToFile](https://docs.microsoft.com/java/api/com.microsoft.azure.storage.blob._cloud_blob.downloadtofile#com_microsoft_azure_storage_blob__cloud_blob_downloadToFile_final_String).

Následující kód stáhne objekt blob nahráli v předchozí části, přidání příponu "_DOWNLOADED" na název objektu blob, abyste viděli oba soubory na místním disku. 

```java
// Download blob. In most cases, you would have to retrieve the reference
// to cloudBlockBlob here. However, we created that reference earlier, and 
// haven't changed the blob we're interested in, so we can reuse it. 
// Here we are creating a new file to download to. Alternatively you can also pass in the path as a string into downloadToFile method: blob.downloadToFile("/path/to/new/file").
downloadedFile = new File(sourceFile.getParentFile(), "downloadedFile.txt");
blob.downloadToFile(downloadedFile.getAbsolutePath());
```

### <a name="clean-up-resources"></a>Vyčištění prostředků

Pokud již nepotřebujete objekty BLOB nahrát tento rychlý start, můžete odstranit celou kontejneru pomocí [CloudBlobContainer.DeleteIfExists](https://docs.microsoft.com/java/api/com.microsoft.azure.storage.blob._cloud_blob_container.deleteifexists#com_microsoft_azure_storage_blob__cloud_blob_container_deleteIfExists). To také odstraní soubory v kontejneru.

```java
try {
if(container != null)
    container.deleteIfExists();
} catch (StorageException ex) {
System.out.println(String.format("Service error. Http code: %d and error code: %s", ex.getHttpStatusCode(), ex.getErrorCode()));
}

System.out.println("Deleting the source, and downloaded files");

if(downloadedFile != null)
downloadedFile.deleteOnExit();
        
if(sourceFile != null)
sourceFile.deleteOnExit();
```

## <a name="next-steps"></a>Další kroky

V tento rychlý start zjistili, jak pro přenos souborů mezi místní disk a Azure Blob storage pomocí Java. Další informace o práci s úložišti objektů Blob, pokračujte do úložiště objektů Blob postupy.

> [!div class="nextstepaction"]
> [Postupy operace úložiště objektů BLOB](storage-java-how-to-use-blob-storage.md)

Další informace o Storage Explorer a objekty BLOB najdete v tématu [prostředků úložiště Azure Blob spravovat pomocí Storage Exploreru](../../vs-azure-tools-storage-explorer-blobs.md).
