---
title: "Zabezpečený přístup k datům aplikace v cloudu pomocí služby Azure Storage | Microsoft Docs"
description: "Použití tokenů SAS, šifrování a HTTPS k zabezpečení dat aplikace v cloudu"
services: storage
author: tamram
manager: jeconnoc
ms.service: storage
ms.workload: web
ms.devlang: csharp
ms.topic: tutorial
ms.date: 02/20/2018
ms.author: tamram
ms.custom: mvc
ms.openlocfilehash: 7b7a45073d8d518700f866d9701c3ba64e665dc2
ms.sourcegitcommit: d1f35f71e6b1cbeee79b06bfc3a7d0914ac57275
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/22/2018
---
# <a name="secure-access-to-an-applications-data-in-the-cloud"></a>Zabezpečený přístup k datům aplikace v cloudu

Tento kurz je třetí částí série. Zjistíte, jak bezpečně přistupovat k účtu úložiště. 

Ve třetí části této série se naučíte:

> [!div class="checklist"]
> * Použití tokenů SAS pro přístup k obrázkům miniatur
> * Zapnutí šifrování na straně serveru
> * Povolení přenosu pouze přes protokol HTTP

[Úložiště objektů blob v Azure](../common/storage-introduction.md#blob-storage) představuje robustní službu pro ukládání souborů pro aplikace. Tento kurz rozšiřuje [předchozí téma][previous-tutorial] a ukazuje, jak bezpečně přistupovat k účtu úložiště z webové aplikace. Až budete hotovi, obrázky budou šifrované a webová aplikace bude pro přístup k obrázkům miniatur používat zabezpečené tokeny SAS.

## <a name="prerequisites"></a>Požadavky

K dokončení tohoto kurzu je nutné dokončit předchozí kurz o službě Storage: [Automatizace změny velikosti nahraných obrázků s využitím služby Event Grid][previous-tutorial]. 

## <a name="set-container-public-access"></a>Nastavení veřejného přístupu ke kontejneru

V této části série kurzů se pro přístup k miniaturám používají tokeny SAS. V tomto kroku nastavíte veřejný přístup ke kontejneru _thumbs_ na hodnotu `off` (vypnuto).

```azurecli-interactive 
blobStorageAccount=<blob_storage_account>

blobStorageAccountKey=$(az storage account keys list -g myResourceGroup \
-n $blobStorageAccount --query [0].value --output tsv) 

az storage container set-permission \ --account-name $blobStorageAccount \ --account-key $blobStorageAccountKey \ --name thumbs  \
--public-access off
``` 

## <a name="configure-sas-tokens-for-thumbnails"></a>Konfigurace tokenů SAS pro miniatury

V první části této série kurzů zobrazovala webová aplikace obrázky z veřejného kontejneru. V této části série použijete k načtení obrázků miniatur tokeny [SAS (sdílený přístupový podpis)](../common/storage-dotnet-shared-access-signature-part-1.md#what-is-a-shared-access-signature). Tokeny SAS umožňují zajistit omezený přístup ke kontejneru nebo objektu blob na základě IP adresy, protokolu, časového intervalu nebo povolených oprávnění.

V tomto příkladu používá úložiště zdrojového kódu větev `sasTokens`, která obsahuje aktualizovaný vzorový kód. Odstraňte stávající nasazení z GitHubu pomocí příkazu [az webapp deployment source delete](/cli/azure/webapp/deployment/source#az_webapp_deployment_source_delete). Dále nakonfigurujte nasazení z GitHubu do webové aplikace pomocí příkazu [az webapp deployment source config](/cli/azure/webapp/deployment/source#az_webapp_deployment_source_config).  

V následujícím příkazu je `<web-app>` název vaší webové aplikace.  

```azurecli-interactive 
az webapp deployment source delete --name <web-app> --resource-group myResourceGroup

az webapp deployment source config --name <web_app> \
--resource-group myResourceGroup --branch sasTokens --manual-integration \
--repo-url https://github.com/Azure-Samples/storage-blob-upload-from-webapp
``` 

Ve větvi `sasTokens` úložiště se aktualizuje soubor `StorageHelper.cs`. Úlohu `GetThumbNailUrls` nahradí níže uvedeným příkladem kódu. Aktualizovaná úloha načítá adresy URL miniatur tím, že v [SharedAccessBlobPolicy](/dotnet/api/microsoft.windowsazure.storage.blob.sharedaccessblobpolicy?view=azure-dotnet) nastaví čas spuštění, čas ukončení platnosti a oprávnění tokenu SAS. Webová aplikace teď po nasazení načítá miniatury s použitím adresy URL s tokenem SAS. Aktualizovaná úloha je znázorněná v následujícím příkladu:
    
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

Předchozí úloha využívá následující třídy, vlastnosti a metody:

|Třída  |Vlastnosti| Metody  |
|---------|---------|---------|
|[StorageCredentials](/dotnet/api/microsoft.windowsazure.storage.auth.storagecredentials?view=azure-dotnet)    |         |
|[CloudStorageAccount](/dotnet/api/microsoft.windowsazure.storage.cloudstorageaccount?view=azure-dotnet)     | |[CreateCloudBlobClient](/dotnet/api/microsoft.windowsazure.storage.cloudstorageaccount.createcloudblobclient?view=azure-dotnet#Microsoft_WindowsAzure_Storage_CloudStorageAccount_CreateCloudBlobClient)        |
|[CloudBlobClient](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblobclient?view=azure-dotnet)     | |[GetContainerReference](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblobclient.getcontainerreference?view=azure-dotnet#Microsoft_WindowsAzure_Storage_Blob_CloudBlobClient_GetContainerReference_System_String_)         |
|[CloudBlobContainer](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblobcontainer?view=azure-dotnet)     | |[SetPermissionsAsync](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblobcontainer.setpermissionsasync?view=azure-dotnet#Microsoft_WindowsAzure_Storage_Blob_CloudBlobContainer_SetPermissionsAsync_Microsoft_WindowsAzure_Storage_Blob_BlobContainerPermissions_) <br> [ListBlobsSegmentedAsync](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblobcontainer.listblobssegmentedasync?view=azure-dotnet#Microsoft_WindowsAzure_Storage_Blob_CloudBlobContainer_ListBlobsSegmentedAsync_System_String_System_Boolean_Microsoft_WindowsAzure_Storage_Blob_BlobListingDetails_System_Nullable_System_Int32__Microsoft_WindowsAzure_Storage_Blob_BlobContinuationToken_Microsoft_WindowsAzure_Storage_Blob_BlobRequestOptions_Microsoft_WindowsAzure_Storage_OperationContext_)       |
|[BlobContinuationToken](/dotnet/api/microsoft.windowsazure.storage.blob.blobcontinuationtoken?view=azure-dotnet)     |         |
|[BlobResultSegment](/dotnet/api/microsoft.windowsazure.storage.blob.blobresultsegment?view=azure-dotnet)    | [Results](/dotnet/api/microsoft.windowsazure.storage.blob.blobresultsegment.results?view=azure-dotnet#Microsoft_WindowsAzure_Storage_Blob_BlobResultSegment_Results)         |
|[CloudBlockBlob](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblockblob?view=azure-dotnet)    |         | [GetSharedAccessSignature](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblob.getsharedaccesssignature?view=azure-dotnet#Microsoft_WindowsAzure_Storage_Blob_CloudBlob_GetSharedAccessSignature_Microsoft_WindowsAzure_Storage_Blob_SharedAccessBlobPolicy_)
|[SharedAccessBlobPolicy](/dotnet/api/microsoft.windowsazure.storage.blob.sharedaccessblobpolicy?view=azure-dotnet)     | [SharedAccessStartTime](/dotnet/api/microsoft.windowsazure.storage.blob.sharedaccessblobpolicy.sharedaccessstarttime?view=azure-dotnet#Microsoft_WindowsAzure_Storage_Blob_SharedAccessBlobPolicy_SharedAccessStartTime)<br>[SharedAccessExpiryTime](/dotnet/api/microsoft.windowsazure.storage.blob.sharedaccessblobpolicy.sharedaccessexpirytime?view=azure-dotnet#Microsoft_WindowsAzure_Storage_Blob_SharedAccessBlobPolicy_SharedAccessExpiryTime)<br>[Oprávnění](/dotnet/api/microsoft.windowsazure.storage.blob.sharedaccessblobpolicy.permissions?view=azure-dotnet#Microsoft_WindowsAzure_Storage_Blob_SharedAccessBlobPolicy_Permissions) |        |

## <a name="server-side-encryption"></a>Šifrování na straně serveru

[Šifrování služby Azure Storage (SSE)](../common/storage-service-encryption.md) pomáhá chránit a zabezpečit vaše data. SSE šifruje neaktivní uložená data a přitom zpracovává šifrování, dešifrování a správu klíčů. Veškerá data se šifrují pomocí 256bitového [šifrování AES](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard). To je jedna z nejsilnějších dostupných variant blokového šifrování.

V následující ukázce povolíte šifrování objektů blob. Existující objekty blob vytvořené před povolením šifrování se nešifrují. Hlavička `x-ms-server-encrypted` v požadavku na objekt blob ukazuje stav šifrování objektu blob.

```azurecli-interactive
az storage account update --encryption-services blob --name <storage-account-name> --resource-group myResourceGroup
```

Když je teď šifrování povolené, nahrajte do webové aplikace nový obrázek.

Pomocí příkazu `curl` s přepínačem `-I` načtěte pouze hlavičky a místo `<storage-account-name>` (název účtu úložiště), `<container>` (kontejner) a `<blob-name>` (název objektu blob) použijte vlastní hodnoty.  

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

V odpovědi si všimněte, že hlavička `x-ms-server-encrypted` má hodnotu `true`. Tato hlavička značí, že jsou teď data šifrovaná pomocí SSE.

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

## <a name="enable-https-only"></a>Povolení pouze HTTPS

Abyste zajistili zabezpečení požadavků na data přicházejících do a z účtu úložiště, můžete požadavky omezit pouze na HTTPS. Aktualizujte požadovaný protokol pro účet úložiště pomocí příkazu [az storage account update](/cli/azure/storage/account#az_storage_account_update).

```azurecli-interactive
az storage account update --resource-group myresourcegroup --name <storage-account-name> --https-only true
```

Otestujte připojení pomocí příkazu `curl` s použitím protokolu `HTTP`.

```azurecli-interactive
curl http://<storage-account-name>.blob.core.windows.net/<container>/<blob-name> -I
```

Vzhledem k tomu, že se teď vyžaduje zabezpečený přenos, zobrazí se následující zpráva:

```
HTTP/1.1 400 The account being accessed does not support http.
```

## <a name="next-steps"></a>Další kroky

Ve třetí části série jste se dozvěděli, jak zabezpečit přístup k účtu úložiště a naučili jste se například:

> [!div class="checklist"]
> * Použití tokenů SAS pro přístup k obrázkům miniatur
> * Zapnutí šifrování na straně serveru
> * Povolení přenosu pouze přes protokol HTTP

Přejděte ke čtvrté části série, kde se dozvíte, jak monitorovat a řešit potíže s aplikací cloudového úložiště.

> [!div class="nextstepaction"]
> [Monitorování a řešení potíží s aplikací cloudového úložiště](storage-monitor-troubleshoot-storage-application.md)

[previous-tutorial]: ../../event-grid/resize-images-on-storage-blob-upload-event.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json