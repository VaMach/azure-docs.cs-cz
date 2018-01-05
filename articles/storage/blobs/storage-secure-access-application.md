---
title: "Zabezpečený přístup k datům aplikace v cloudu s Azure Storage | Microsoft Docs"
description: "Použijte tokeny SAS, šifrování a HTTPS k zabezpečení dat vaší aplikace v cloudu"
services: storage
documentationcenter: 
author: georgewallace
manager: timlt
editor: 
ms.service: storage
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: csharp
ms.topic: tutorial
ms.date: 09/19/2017
ms.author: gwallace
ms.custom: mvc
ms.openlocfilehash: c43165e230a00b6a4408637fd2290a21800d07b9
ms.sourcegitcommit: 3cdc82a5561abe564c318bd12986df63fc980a5a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/05/2018
---
# <a name="secure-access-to-an-applications-data-in-the-cloud"></a>Zabezpečený přístup k datům aplikace v cloudu

V tomto kurzu je součástí série, tři. Zjistíte, jak zabezpečit přístup k účtu úložiště. 

V rámci tři řady, zjistíte, jak:

> [!div class="checklist"]
> * Používat tokeny SAS pro přístup k obrázky miniatur
> * Zapnout šifrování na straně serveru
> * Povolit přenos jenom protokol HTTPS

[Úložiště objektů blob Azure](../common/storage-introduction.md#blob-storage) poskytuje robustní služby k uložení souborů pro aplikace. V tomto kurzu rozšiřuje [předchozí téma] [ previous-tutorial] zobrazíte postup zabezpečený přístup k účtu úložiště z webové aplikace. Po dokončení se šifrují bitové kopie a webové aplikace používá zabezpečené tokeny SAS pro přístup k miniatury.

## <a name="prerequisites"></a>Požadavky

K dokončení tohoto kurzu je nutné provést předchozí kurzu úložiště: [automatické změně velikosti nahrán obrázky pomocí událostí mřížky][previous-tutorial]. 

## <a name="set-container-public-access"></a>Sada kontejneru veřejného přístupu

V této části kurzu řady tokeny SAS slouží pro přístup k miniatur. V tomto kroku nastavíte veřejný přístup _palec_ kontejner, aby `off`.

```azurecli-interactive 
blobStorageAccount=<blob_storage_account>

blobStorageAccountKey=$(az storage account keys list -g myResourceGroup \
-n $blobStorageAccount --query [0].value --output tsv) 

az storage container set-permission \ --account-name $blobStorageAccount \ --account-key $blobStorageAccountKey \ --name thumbs  \
--public-access off
``` 

## <a name="configure-sas-tokens-for-thumbnails"></a>Konfigurace tokeny SAS pro miniatury

V rámci jeden z této série kurz webové aplikace se zobrazuje bitové kopie z veřejného kontejneru. V této části řady použijete [zabezpečení přístupového podpisu (SAS)](../common/storage-dotnet-shared-access-signature-part-1.md#what-is-a-shared-access-signature) tokeny pro načtení miniatury. Tokeny SAS umožňují poskytovat omezený přístup na kontejner objektů blob na základě IP, protokol, časový interval nebo práva povolené.

V tomto příkladu používá úložiště zdrojového kódu `sasTokens` větve, který má ukázce aktualizované kódu. Odstranit existující nasazení Githubu s [odstranění zdroj nasazení webapp az](/cli/azure/webapp/deployment/source#az_webapp_deployment_source_delete). V dalším kroku nakonfigurujte Githubu nasazení do webové aplikace pomocí [az webapp nasazení zdroj konfigurace](/cli/azure/webapp/deployment/source#az_webapp_deployment_source_config) příkaz.  

V následujícím příkazu `<web-app>` je název vaší webové aplikace.  

```azurecli-interactive 
az webapp deployment source delete --name <web-app> --resource-group myResourceGroup

az webapp deployment source config --name <web_app> \
--resource-group myResourceGroup --branch sasTokens --manual-integration \
--repo-url https://github.com/Azure-Samples/storage-blob-upload-from-webapp
``` 

`sasTokens` Větve úložiště aktualizací `StorageHelper.cs` souboru. Nahradí `GetThumbNailUrls` úlohy s následujícím příkladu kódu. Aktualizované úloha načte miniaturu adresy URL nastavením [SharedAccessBlobPolicy](/dotnet/api/microsoft.windowsazure.storage.blob.sharedaccessblobpolicy?view=azure-dotnet) zadat čas spuštění, čas vypršení platnosti a oprávnění pro SAS token. Po nasazení webové aplikace teď načte miniatur s adresou URL, pomocí SAS token. Aktualizované úloh je znázorněno v následujícím příkladu:
    
```csharp
public static async Task<List<string>> GetThumbNailUrls(AzureStorageConfig _storageConfig)
{
    List<string> thumbnailUrls = new List<string>();

    // Create storagecredentials object by reading the values from the configuration (appsettings.json)
    StorageCredentials storageCredentials = new StorageCredentials(_storageConfig.AccountName, _storageConfig.AccountKey);

    // Create cloudstorage account by passing the storagecredentials
    CloudStorageAccount storageAccount = new CloudStorageAccount(storageCredentials, true);

    // Create blob client
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    // Get reference to the container
    CloudBlobContainer container = blobClient.GetContainerReference(_storageConfig.ThumbnailContainer);

    // Set the permission of the container to public
    await container.SetPermissionsAsync(new BlobContainerPermissions { PublicAccess = BlobContainerPublicAccessType.Blob });

    BlobContinuationToken continuationToken = null;

    BlobResultSegment resultSegment = null;

    //Call ListBlobsSegmentedAsync and enumerate the result segment returned, while the continuation token is non-null.
    //When the continuation token is null, the last page has been returned and execution can exit the loop.
    do
    {
        //This overload allows control of the page size. You can return all remaining results by passing null for the maxResults parameter,
        //or by calling a different overload.
        resultSegment = await container.ListBlobsSegmentedAsync("", true, BlobListingDetails.All, 10, continuationToken, null, null);

        foreach (var blobItem in resultSegment.Results)
        {
            CloudBlockBlob blob = blobItem as CloudBlockBlob;
            //Set the expiry time and permissions for the blob.
            //In this case, the start time is specified as a few minutes in the past, to mitigate clock skew.
            //The shared access signature will be valid immediately.
            SharedAccessBlobPolicy sasConstraints = new SharedAccessBlobPolicy();

            sasConstraints.SharedAccessStartTime = DateTimeOffset.UtcNow.AddMinutes(-5);

            sasConstraints.SharedAccessExpiryTime = DateTimeOffset.UtcNow.AddHours(24);

            sasConstraints.Permissions = SharedAccessBlobPermissions.Read;

            //Generate the shared access signature on the blob, setting the constraints directly on the signature.
            string sasBlobToken = blob.GetSharedAccessSignature(sasConstraints);

            //Return the URI string for the container, including the SAS token.
            thumbnailUrls.Add(blob.Uri + sasBlobToken);

        }

        //Get the continuation token.
        continuationToken = resultSegment.ContinuationToken;
    }

    while (continuationToken != null);

    return await Task.FromResult(thumbnailUrls);
}
```

Následující třídy, vlastnosti a metody se používají v předchozí úloze:

|Třída  |Vlastnosti| Metody  |
|---------|---------|---------|
|[StorageCredentials](/dotnet/api/microsoft.windowsazure.storage.auth.storagecredentials?view=azure-dotnet)    |         |
|[CloudStorageAccount](/dotnet/api/microsoft.windowsazure.storage.cloudstorageaccount?view=azure-dotnet)     | |[CreateCloudBlobClient](/dotnet/api/microsoft.windowsazure.storage.cloudstorageaccount.createcloudblobclient?view=azure-dotnet#Microsoft_WindowsAzure_Storage_CloudStorageAccount_CreateCloudBlobClient)        |
|[CloudBlobClient](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblobclient?view=azure-dotnet)     | |[GetContainerReference](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblobclient.getcontainerreference?view=azure-dotnet#Microsoft_WindowsAzure_Storage_Blob_CloudBlobClient_GetContainerReference_System_String_)         |
|[CloudBlobContainer](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblobcontainer?view=azure-dotnet)     | |[SetPermissionsAsync](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblobcontainer.setpermissionsasync?view=azure-dotnet#Microsoft_WindowsAzure_Storage_Blob_CloudBlobContainer_SetPermissionsAsync_Microsoft_WindowsAzure_Storage_Blob_BlobContainerPermissions_) <br> [ListBlobsSegmentedAsync](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblobcontainer.listblobssegmentedasync?view=azure-dotnet#Microsoft_WindowsAzure_Storage_Blob_CloudBlobContainer_ListBlobsSegmentedAsync_System_String_System_Boolean_Microsoft_WindowsAzure_Storage_Blob_BlobListingDetails_System_Nullable_System_Int32__Microsoft_WindowsAzure_Storage_Blob_BlobContinuationToken_Microsoft_WindowsAzure_Storage_Blob_BlobRequestOptions_Microsoft_WindowsAzure_Storage_OperationContext_)       |
|[BlobContinuationToken](/dotnet/api/microsoft.windowsazure.storage.blob.blobcontinuationtoken?view=azure-dotnet)     |         |
|[BlobResultSegment](/dotnet/api/microsoft.windowsazure.storage.blob.blobresultsegment?view=azure-dotnet)    | [Výsledky](/dotnet/api/microsoft.windowsazure.storage.blob.blobresultsegment.results?view=azure-dotnet#Microsoft_WindowsAzure_Storage_Blob_BlobResultSegment_Results)         |
|[CloudBlockBlob](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblockblob?view=azure-dotnet)    |         | [GetSharedAccessSignature](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblob.getsharedaccesssignature?view=azure-dotnet#Microsoft_WindowsAzure_Storage_Blob_CloudBlob_GetSharedAccessSignature_Microsoft_WindowsAzure_Storage_Blob_SharedAccessBlobPolicy_)
|[SharedAccessBlobPolicy](/dotnet/api/microsoft.windowsazure.storage.blob.sharedaccessblobpolicy?view=azure-dotnet)     | [SharedAccessStartTime](/dotnet/api/microsoft.windowsazure.storage.blob.sharedaccessblobpolicy.sharedaccessstarttime?view=azure-dotnet#Microsoft_WindowsAzure_Storage_Blob_SharedAccessBlobPolicy_SharedAccessStartTime)<br>[SharedAccessExpiryTime](/dotnet/api/microsoft.windowsazure.storage.blob.sharedaccessblobpolicy.sharedaccessexpirytime?view=azure-dotnet#Microsoft_WindowsAzure_Storage_Blob_SharedAccessBlobPolicy_SharedAccessExpiryTime)<br>[Oprávnění](/dotnet/api/microsoft.windowsazure.storage.blob.sharedaccessblobpolicy.permissions?view=azure-dotnet#Microsoft_WindowsAzure_Storage_Blob_SharedAccessBlobPolicy_Permissions) |        |

## <a name="server-side-encryption"></a>Šifrování na straně serveru

[Šifrování služby Azure Storage (SSE)](../common/storage-service-encryption.md) pomáhá chránit a chrání vaše data. SSE šifruje data v klidovém stavu, zpracování šifrování, dešifrování a správu klíčů. Všechna data se šifrují pomocí 256 bitů [šifrování AES](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard), jednu z nejsilnějších bloku šifer k dispozici.

V následující ukázce je povolit šifrování pro objekty BLOB. Existující objekty BLOB vytvořené před povolením šifrování nejsou šifrovány. `x-ms-server-encrypted` Záhlaví na vyžádání pro objekt blob se zobrazí stav šifrování objektu blob.

```azurecli-interactive
az storage account update --encryption-services blob --name <storage-account-name> --resource-group myResourceGroup
```

Teď, když je povolené šifrování, nahrajte novou bitovou kopii do webové aplikace.

Pomocí `curl` s přepínačem `-I` načíst pouze záhlaví, dosaďte svoje vlastní hodnoty `<storage-account-name>`, `<container>`, a `<blob-name>`.  

```azurecli-interactive
sasToken=$(az storage blob generate-sas \
    --account-name <storage-account-name> \
    --account-key <storage-account-key> \
    --container-name <container> \
    --name <blob-name> \
    --permissions r \
    --expiry `date --date="next day" +%Y-%m-%d` \
    --output tsv)

curl https://<storage-account-name>.blob.core.windows.net/<container>/<blob-name>?$sasToken -I
```

V odpovědi, Upozorňujeme `x-ms-server-encrypted` záhlaví ukazuje `true`. Tuto hlavičku identifikuje, že data je nyní šifrován SSE.

```
HTTP/1.1 200 OK
Content-Length: 209489
Content-Type: image/png
Last-Modified: Mon, 11 Sep 2017 19:27:42 GMT
Accept-Ranges: bytes
ETag: "0x8D4F94B2BE76D45"
Server: Windows-Azure-Blob/1.0 Microsoft-HTTPAPI/2.0
x-ms-request-id: 57047db3-001e-0050-3e34-2ba769000000
x-ms-version: 2017-04-17
x-ms-lease-status: unlocked
x-ms-lease-state: available
x-ms-blob-type: BlockBlob
x-ms-server-encrypted: true
Date: Mon, 11 Sep 2017 19:27:46 GMT
```

## <a name="enable-https-only"></a>Povolit pouze protokol HTTPS

Aby se zajistilo, že jsou požadavky na data z účtu úložiště a zabezpečení, můžete omezit pouze požadavky na protokol HTTPS. Aktualizujte protokol vyžaduje účet úložiště pomocí [aktualizace účtu úložiště az](/cli/azure/storage/account#az_storage_account_update) příkaz.

```azurecli-interactive
az storage account update --resource-group myresourcegroup --name <storage-account-name> --https-only true
```

Testovací připojení pomocí `curl` pomocí `HTTP` protokolu.

```azurecli-interactive
curl http://<storage-account-name>.blob.core.windows.net/<container>/<blob-name> -I
```

Teď, když bezpečnému přenosu je potřeba, zobrazí se následující zpráva:

```
HTTP/1.1 400 The account being accessed does not support http.
```

## <a name="next-steps"></a>Další postup

V rámci tři řady jste zjistili, jak zabezpečit přístup k účtu úložiště, jako například:

> [!div class="checklist"]
> * Používat tokeny SAS pro přístup k obrázky miniatur
> * Zapnout šifrování na straně serveru
> * Povolit přenos jenom protokol HTTPS

Přejděte k části čtyři série se dozvíte, jak ke sledování a řešení potíží s aplikací na cloudové úložiště.

> [!div class="nextstepaction"]
> [Monitorování a řešení úložiště aplikací cloudové aplikace](storage-monitor-troubleshoot-storage-application.md)

[previous-tutorial]: ../../event-grid/resize-images-on-storage-blob-upload-event.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json