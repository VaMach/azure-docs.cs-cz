---
title: "Azure rychlý start - objekty přenosu do nebo z Azure Blob storage pomocí Node.js| Microsoft Docs"
description: "Naučte se rychle převést objekty do a z Azure Blob storage pomocí Node.js"
services: storage
documentationcenter: storage
author: georgewallace
manager: timlt
editor: tysonn
ms.assetid: 
ms.custom: mvc
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 10/30/2017
ms.author: gwallace
ms.openlocfilehash: dd4d3abf082767c40760d020c0997b365452e769
ms.sourcegitcommit: e38120a5575ed35ebe7dccd4daf8d5673534626c
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/13/2017
---
# <a name="transfer-objects-tofrom-azure-blob-storage-using-nodejs"></a>Objekty přenosu do nebo z Azure Blob storage pomocí Node.js

V tento rychlý start a zjistěte, jak se použití Node.js pro odesílání, stahování a seznam objektů BLOB bloku v kontejneru v úložiště objektů Blob v Azure.

## <a name="prerequisites"></a>Požadavky

K provedení kroků v tomto kurzu Rychlý start je potřeba:

* Instalovat [Node.js](https://nodejs.org/en/)

Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) před tím, než začnete.

[!INCLUDE [storage-quickstart-tutorial-create-account-portal](../../../includes/storage-quickstart-tutorial-create-account-portal.md)]

## <a name="download-the-sample-application"></a>Stažení ukázkové aplikace

[Ukázkové aplikace](https://github.com/Azure-Samples/storage-blobs-node-quickstart.git) používán tento rychlý start je základní konzolovou aplikaci. 

Použití [git](https://git-scm.com/) stáhnout kopii aplikace na svoje vývojové prostředí.

```bash
git clone https://github.com/Azure-Samples/storage-blobs-node-quickstart.git
```

Tento příkaz provede klonování úložiště do složky místní git. Otevřete aplikaci vzhledu složky úložiště objektů BLOB uzlu quickstart, otevřete ho a poklikejte na index.js.

## <a name="configure-your-storage-connection-string"></a>Konfigurace připojovacího řetězce úložiště

V aplikaci je nutné zadat připojovací řetězec pro váš účet úložiště. Otevřete `index.js` souboru, vyhledejte `connectionString` proměnné. Nahraďte celou hodnotu připojovacího řetězce, který jste uložili z portálu Azure se jeho hodnota. Připojovací řetězec úložiště by měl vypadat takto:

```javascript
// Create a blob client for interacting with the blob service from connection string
// How to create a storage connection string - http://msdn.microsoft.com/library/azure/ee758697.aspx
var connectionString = '<Your connection string here>';
var blobService = storage.createBlobService(connectionString);
```

## <a name="install-required-packages"></a>Nainstalujte požadované balíčky

V adresáři aplikace spustit `npm install` na instalaci kterékoli požadované balíčky uvedené v `package.json` souboru.

```javascript
npm install
```

## <a name="run-the-sample"></a>Spustit ukázku

Tato ukázka vytvoří testovací soubor ve složce Dokumenty, odešle do úložiště objektů Blob, obsahuje seznam objektů BLOB v kontejneru a potom stáhne soubor pod novým názvem, můžete porovnat staré a nové soubory.

Spustit ukázku zadáním `node index.js`. Následující výstup je ze systému Windows.  Podobný výstup s příslušnou cesty dají očekávat při použití systému Linux.

```
Azure Storage Node.js Client Library Blobs Quick Start

1. Creating a container with public access: quickstartcontainer-79a3eea0-bec9-11e7-9a36-614cd00ca63d

2. Creating a file in ~/Documents folder to test the upload and download

   Local File: C:\Users\admin\Documents\HelloWorld-79a3c790-bec9-11e7-9a36-614cd00ca63d.txt

3. Uploading BlockBlob: quickstartblob-HelloWorld-79a3c790-bec9-11e7-9a36-614cd00ca63d.txt

   Uploaded Blob URL: https://mystorageaccount.blob.core.windows.net/quickstartcontainer-79a3eea0-bec9-11e7-9a36-614cd00ca63d/quickstartblob-HelloWorld-79a3c790-bec9-11e7-9a36-614cd00ca63d.txt

4. Listing blobs in container

   - quickstartblob-HelloWorld-79a3c790-bec9-11e7-9a36-614cd00ca63d.txt (type: BlockBlob)


5. Downloading blob

   Downloaded File: C:\Users\admin\Documents\HelloWorld-79a3c790-bec9-11e7-9a36-614cd00ca63d_DOWNLOADED.txt

Sample finished running. When you hit <ENTER> key, the temporary files will be deleted and the sample application will exit.
```

Než budete pokračovat, zkontrolujte MyDocuments dva soubory. Můžete je otevřít a zjistit, že jsou identické.

Můžete také použít nástroj, jako [Azure Storage Explorer](http://storageexplorer.com/?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) k prohlížení souborů v Blob storage. Azure Storage Explorer je bezplatný nástroj napříč platformami, který umožňuje zobrazit informace o účtu úložiště.

Jakmile ověříte soubory, stisknutí libovolné klávesy ukončíte ukázku a odstranit testovací soubory. Teď, když víte, jaké ukázku, otevřete soubor index.js podívejte se na kód. 

## <a name="understand-the-sample-code"></a>Pochopení ukázkový kód

V dalším kroku jsme provede ukázkový kód tak, aby vám pochopit, jak to funguje.

### <a name="get-references-to-the-storage-objects"></a>Získat odkazy na objekty úložiště

Prvním krokem je vytvoření odkazu na `BlobService` umožňuje otvírat a spravovat úložiště objektů Blob. Tyto objekty sestavení na sobě navzájem--používá každý další jeden v seznamu.

* Vytvoření instance  **[BlobService](/nodejs/api/azure-storage/blobservice?view=azure-node-2.2.0#azure_storage_BlobService__ctor)**  objekt, který odkazuje na službu objektů Blob v účtu úložiště.

* Vytvořit nový kontejner a poté nastavit oprávnění na kontejner, aby objekty BLOB jsou veřejné a je přístupný pomocí jenom adresy URL. Kontejner začíná **quickstartcontainer -**.

Tento příklad používá [createContainerCreateIfNotExists](/nodejs/api/azure-storage/blobservice#azure_storage_BlobService_createContainerIfNotExists) vzhledem k tomu, že chcete vytvořit nový kontejner pokaždé, když se spustí vzorku. V produkčním prostředí, kde používáte stejný kontejner v celé aplikaci je lepší zvykem volat CreateIfNotExists pouze jednou. Alternativně můžete vytvořit kontejner předem, takže není nutné vytvořit v kódu.

```javascript
// Create a container for organizing blobs within the storage account.
console.log('1. Creating a Container with Public Access:', blockBlobContainerName, '\n');
blobService.createContainerIfNotExists(blockBlobContainerName, { 'publicAccessLevel': 'blob' }, function (error) {
    if (error) return callback(error);
```

### <a name="upload-blobs-to-the-container"></a>Odešlete do kontejneru objektů BLOB

Úložiště objektů blob podporuje objekty blob bloku, doplňovací objekty blob a objekty blob stránky. Objekty BLOB bloku se nejčastěji používají. Jsou ideální pro ukládání textu a binárních dat, což je z důvodu, které jsou použity v tento rychlý start.

Chcete-li nahrát soubor do objektu blob, použijte [createBlockBlobFromLocalFile](/nodejs/api/azure-storage/blobservice?view=azure-node-2.2.0#azure_storage_BlobService_createBlockBlobFromLocalFile) metoda. Tato operace vytvoří objekt blob, pokud ještě neexistuje, nebo ho přepíše, pokud již existuje.

Ukázkový kód vytvoří místní soubor má být použit pro nahrávání a stahování, uložení souboru k odeslání jako **Místnícesta** a název objektu blob v **localFileToUpload**. Následující příklad odešle soubor do vašeho kontejneru, který bude začínat **quickstartcontainer -**.

```javascript
console.log('2. Creating a file in ~/Documents folder to test the upload and download\n');
console.log('   Local File:', LOCAL_FILE_PATH, '\n');
fs.writeFileSync(LOCAL_FILE_PATH, 'Greetings from Microsoft!');

console.log('3. Uploading BlockBlob:', BLOCK_BLOB_NAME, '\n');
blobService.createBlockBlobFromLocalFile(CONTAINER_NAME, BLOCK_BLOB_NAME, LOCAL_FILE_PATH, function (error) {
handleError(error);
console.log('   Uploaded Blob URL:', blobService.getUrl(CONTAINER_NAME, BLOCK_BLOB_NAME), '\n');
```

Existuje několik metod nahrávání, které můžete použít pomocí úložiště objektů Blob. Například pokud máte datový proud paměti, můžete použít [createBlockBlobFromStream](/nodejs/api/azure-storage/blobservice?view=azure-node-2.2.0#azure_storage_BlobService_createBlockBlobFromStream) metoda místo [createBlockBlobFromLocalFile](/nodejs/api/azure-storage/blobservice?view=azure-node-2.2.0#azure_storage_BlobService_createBlockBlobFromLocalFile).

### <a name="list-the-blobs-in-a-container"></a>Zobrazí seznam objektů blob v kontejneru

Další, aplikace získá seznam souborů v kontejneru pomocí [listBlobsSegmented](/nodejs/api/azure-storage/blobservice?view=azure-node-2.2.0#azure_storage_BlobService_listBlobsSegmented). Následující kód načte seznam objektů BLOB a potom je, prochází zobrazující identifikátory URI objektů BLOB nalezené. Můžete zkopírovat identifikátor URI z příkazové okno a vložte ho do prohlížeče a prohlédněte soubor.

Pokud máte 5 000 nebo méně objektů BLOB v kontejneru, jsou v jednoho volání načíst všechny názvy objektů blob [listBlobsSegmented](/nodejs/api/azure-storage/blobservice?view=azure-node-2.2.0#azure_storage_BlobService_listBlobsSegmented). Pokud máte více než 5 000 objektů BLOB v kontejneru, služba načte seznam v sadách 5 000, dokud načíst všechny názvy objektů blob. Takže prvním toto rozhraní API je volána, vrátí první 5 000 objektů blob názvy a token pokračování. Podruhé, zadejte token, služba načte další sadu názvy objektů blob a tak dále, dokud token pro pokračování je č. hodnota null, což znamená, že všechny názvy objektů blob byly získány.

```javascript
console.log('4. Listing blobs in container\n');
blobService.listBlobsSegmented(CONTAINER_NAME, null, function (error, data) {
    handleError(error);

    for (var i = 0; i < data.entries.length; i++) {
    console.log(util.format('   - %s (type: %s)'), data.entries[i].name, data.entries[i].blobType);
    }
    console.log('\n');
```

### <a name="download-blobs"></a>Stáhnout objekty blob

Stáhnout objekty BLOB do vašeho místního disku pomocí [getBlobToLocalFile](/nodejs/api/azure-storage/blobservice?view=azure-node-2.2.0#azure_storage_BlobService_getBlobToLocalFile).

Následující kód stáhne objekt blob nahráli v předchozí části, přidání příponu "_DOWNLOADED" na název objektu blob, abyste viděli oba soubory na místním disku. 

```javascript
console.log('5. Downloading blob\n');
blobService.getBlobToLocalFile(CONTAINER_NAME, BLOCK_BLOB_NAME, DOWNLOADED_FILE_PATH, function (error) {
handleError(error);
console.log('   Downloaded File:', DOWNLOADED_FILE_PATH, '\n');
```

### <a name="clean-up-resources"></a>Vyčištění prostředků

Pokud již nepotřebujete objekty BLOB nahrát tento rychlý start, můžete odstranit celou kontejneru pomocí [deleteBlobIfExists](/nodejs/api/azure-storage/blobservice#azure_storage_BlobService_deleteBlobIfExists) a [deleteContainerIfExists](/nodejs/api/azure-storage/blobservice#azure_storage_BlobService_deleteContainerIfExists). Také odstraňte soubory vytvoří, pokud už nejsou potřeba. To se stará v aplikaci po stisknutí klávesy enter aplikaci ukončíte.

```javascript
console.log('6. Deleting block Blob\n');
    blobService.deleteBlobIfExists(CONTAINER_NAME, BLOCK_BLOB_NAME, function (error) {
        handleError(error);

    console.log('7. Deleting container\n');
    blobService.deleteContainerIfExists(CONTAINER_NAME, function (error) {
        handleError(error);
            
        fs.unlinkSync(LOCAL_FILE_PATH);
        fs.unlinkSync(DOWNLOADED_FILE_PATH);
```

## <a name="next-steps"></a>Další kroky

V tento rychlý start zjistili, jak pro přenos souborů mezi místní disk a Azure Blob storage pomocí Node.js. Další informace o práci s úložišti objektů Blob, pokračujte do úložiště objektů Blob postupy.

> [!div class="nextstepaction"]
> [Postupy operace úložiště objektů BLOB](storage-nodejs-how-to-use-blob-storage.md)

Další informace o Storage Explorer a objekty BLOB najdete v tématu [prostředků úložiště Azure Blob spravovat pomocí Storage Exploreru](../../vs-azure-tools-storage-explorer-blobs.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).
