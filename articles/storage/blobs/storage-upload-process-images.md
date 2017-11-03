---
title: "Odesílání bitové kopie dat v cloudu s Azure Storage | Microsoft Docs"
description: "Ukládání dat aplikací pomocí služby Azure blob storage s webovou aplikaci"
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
ms.openlocfilehash: a204498016ff837c5247009eaaffbd4f79285d0b
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="upload-image-data-in-the-cloud-with-azure-storage"></a>Odesílání bitové kopie dat v cloudu s Azure Storage

V tomto kurzu je součástí, jednu z řady. V tomto kurzu se dozvíte, jak nasadit webovou aplikaci, která používá Klientská knihovna pro úložiště Azure odesílání obrázků na účet úložiště. Jakmile budete hotovi, máte webovou aplikaci, ukládání a zobrazení obrázků ze služby Azure storage.

![Kontejner zobrazení obrázků](media/storage-upload-process-images/figure2.png)

V rámci jedna řada, zjistíte, jak:

> [!div class="checklist"]
> * vytvořit účet úložiště
> * Vytvořte kontejner a nastavení oprávnění
> * Načtení přístupového klíče
> * Konfigurace nastavení aplikace
> * Nasazení webové aplikace do Azure
> * Interakci s webovou aplikací

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Pokud si zvolíte instalaci a použití rozhraní příkazového řádku místně, tento kurz vyžaduje, že používáte Azure CLI verze verze 2.0.4 nebo novější. Verzi zjistíte spuštěním příkazu `az --version`. Pokud potřebujete instalaci nebo upgrade, přečtěte si téma [Instalace Azure CLI 2.0]( /cli/azure/install-azure-cli). 

## <a name="create-a-resource-group"></a>Vytvoření skupiny prostředků 

Vytvořte skupinu prostředků pomocí příkazu [az group create](/cli/azure/group#create). Skupina prostředků Azure je logický kontejner, ve kterém se nasazují a spravují prostředky Azure.
 
Následující příklad vytvoří skupinu prostředků s názvem `myResourceGroup`.
 
```azurecli-interactive 
az group create --name myResourceGroup --location westcentralus 
``` 

## <a name="create-a-storage-account"></a>vytvořit účet úložiště
 
Ukázka ukládání bitových kopií na kontejner objektů blob v účtu Azure Storage. Účet úložiště poskytuje jedinečný obor názvů pro ukládání a přístup k vaší datové objekty Azure storage. Ve skupině prostředků, kterou jste vytvořili vytvořte účet úložiště pomocí příkazu [az storage account create](/cli/azure/storage/account#create). 

> [!IMPORTANT] 
> V části 2 kurzu použijete odběry událostí pro úložiště objektů blob. Odběry událostí jsou aktuálně podporovány pouze pro účty úložiště Blob v – Západ střední USA a západní USA 2. Z důvodu toto omezení musíte vytvořit účet úložiště Blob, který je používán ukázková aplikace pro uložení bitové kopie a miniatur.   

V následujícím příkazu nahraďte vlastní globálně jedinečný název pro účet úložiště Blob, kde uvidíte `<blob_storage_account>` zástupný symbol.  

```azurecli-interactive 
az storage account create --name <blob_storage_account> \
--location westcentralus --resource-group myResourceGroup \
--sku Standard_LRS --kind blobstorage --access-tier hot 
``` 
 
## <a name="create-blob-storage-containers"></a>Vytvoření kontejnerů úložiště objektů blob
 
Aplikace používá dva kontejnery v účtu úložiště Blob. Kontejnery jsou podobné do složek a se používají k ukládání objektů BLOB. _Bitové kopie_ kontejner je, kde aplikace nahraje plném rozlišení. V pozdější části řady, aplikaci Azure funkce ukládání miniatur změněnou bitovou kopii do _palec_ kontejneru. 

Získat klíč účtu úložiště pomocí [seznam klíčů účtu úložiště az](/cli/azure/storage/account/keys#list) příkaz. Pak použijete tento klíč k vytvoření dvou kontejnerů pomocí [vytvořit kontejner úložiště az](/cli/azure/storage/container#create) příkaz.  
 
V takovém případě `<blob_storage_account>` je název účtu úložiště Blob, který jste vytvořili. _Bitové kopie_ kontejnery veřejný přístup je nastavena na `off`, _palec_ kontejnery veřejný přístup je nastavena na `container`. `container` Nastavení veřejný přístup umožňuje miniatur lze zobrazit na osoby, které naleznete na webové stránce.
 
```azurecli-interactive 
blobStorageAccount=<blob_storage_account>

blobStorageAccountKey=$(az storage account keys list -g myResourceGroup \
-n $blobStorageAccount --query [0].value --output tsv) 

az storage container create -n images --account-name $blobStorageAccount \
--account-key $blobStorageAccountKey --public-access off 

az storage container create -n thumbs --account-name $blobStorageAccount \
--account-key $blobStorageAccountKey --public-access container

echo "Make a note of your blob storage account key..." 
echo $blobStorageAccountKey 
``` 

Poznamenejte si název účtu úložiště objektů blob a klíče. Ukázková aplikace používá tato nastavení pro připojení k účtu úložiště pro odesílání obrázků. 

## <a name="create-an-app-service-plan"></a>Vytvoření plánu služby App Service 

[Plán služby App Service](../../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) určuje umístění, velikost a funkce farmy webových serverů, která je hostitelem vaší aplikace. 

Pomocí příkazu [az appservice plan create](/cli/azure/appservice/plan#create) vytvořte plán služby App Service. 

Následující příklad vytvoří plán služby App Service s názvem `myAppServicePlan` v cenové úrovni **Free**: 

```azurecli-interactive 
az appservice plan create --name myAppServicePlan --resource-group myResourceGroup --sku FREE 
``` 

## <a name="create-a-web-app"></a>Vytvoření webové aplikace 

Webové aplikace místo hostování pro ukázkový kód aplikace, která je nasazena z ukázkové úložiště GitHub. Pomocí příkazu [az webapp create](/cli/azure/webapp#create) vytvořte [webovou aplikaci](../../app-service/app-service-web-overview.md) v plánu služby App Service `myAppServicePlan`.  
 
V následujícím příkazu nahraďte `<web_app>` s jedinečným názvem (platnými znaky jsou `a-z`, `0-9`, a `-`). Pokud `<web_app>` není jedinečný, zobrazí se chybová zpráva: _web se zadaným názvem `<web_app>` již existuje._ Výchozí adresa URL webové aplikace je `https://<web_app>.azurewebsites.net`.  

```azurecli-interactive 
az webapp create --name <web_app> --resource-group myResourceGroup --plan myAppServicePlan 
``` 

## <a name="deploy-the-sample-app-from-the-github-repository"></a>Nasadit ukázkovou aplikaci z úložiště Githubu 

Služby App Service podporuje několik způsobů pro nasazení obsahu pro webovou aplikaci. V tomto kurzu nasazujete webové aplikace z veřejného Ukázka úložiště GitHub: [https://github.com/Azure-Samples/storage-blob-upload-from-webapp](https://github.com/Azure-Samples/storage-blob-upload-from-webapp). Konfigurace Githubu nasazení do webové aplikace pomocí [az webapp nasazení zdroj konfigurace](/cli/azure/webapp/deployment/source#config) příkaz. Nahraďte `<web_app>` s názvem webové aplikace, které jste vytvořili v předchozím kroku.

Obsahuje ukázkový projekt [ASP.NET MVC](https://www.asp.net/mvc) aplikaci, která přijímá bitovou kopii, uloží ho do účtu úložiště a zobrazí obrázky miniatur kontejneru. Webová aplikace používá [Microsoft.WindowsAzure.Storage](/dotnet/api/microsoft.windowsazure.storage?view=azure-dotnet), [Microsoft.WindowsAzure.Storage.Blob](/dotnet/api/microsoft.windowsazure.storage.blob?view=azure-dotnet)a [Microsoft.WindowsAzure.Storage.Auth](/dotnet/api/microsoft.windowsazure.storage.auth?view=azure-dotnet) obory názvů v úložišti Azure klientské knihovny pro interakci s Azure storage. 

```azurecli-interactive 
az webapp deployment source config --name <web_app> \
--resource-group myResourceGroup --branch master --manual-integration \
--repo-url https://github.com/Azure-Samples/storage-blob-upload-from-webapp
``` 

## <a name="configure-web-app-settings"></a>Konfigurovat nastavení webové aplikace 

Použití ukázkové webové aplikace [Klientská knihovna pro úložiště Azure](/dotnet/api/overview/azure/storage?view=azure-dotnet) požadovat přístup tokeny, které se používají k odesílání obrázků. Údaje k účtu úložiště používat sady SDK úložiště se nastavují v nastavení aplikace pro webovou aplikaci. Přidat nastavení aplikace do aplikace nasazené pomocí [az webapp konfigurace appsettings sadu](/cli/azure/webapp/config/appsettings#set) příkaz. 

V následujícím příkazu `<blob_storage_account>` je název účtu úložiště objektů Blob a `<blob_storage_key>` je přidružený klíč. Nahraďte `<web_app>` s názvem webové aplikace, které jste vytvořili v předchozím kroku.     

```azurecli-interactive 
az webapp config appsettings set --name <web_app> --resource-group myResourceGroup \
--settings AzureStorageConfig__AccountName=<blob_storage_account> \
AzureStorageConfig__ImageContainer=images  \
AzureStorageConfig__ThumbnailContainer=thumbs \
AzureStorageConfig__AccountKey=<blob_storage_key>  
``` 

Po webové aplikace je nasazená a nakonfigurovaná, můžete otestovat funkci odesílání bitové kopie v aplikaci.   

## <a name="upload-an-image"></a>Nahrání image 

K otestování webové aplikace, přejděte na adresu URL publikované aplikace. Výchozí adresa URL webové aplikace je `https://<web_app>.azurewebsites.net`. Vyberte **nahrát fotografie** oblasti, kterou chcete vybrat a nahrát soubor nebo přetažení souboru v oblasti. Obrázek zmizí, pokud se úspěšně nahrál.

![ImageResizer aplikace](media/storage-upload-process-images/figure1.png)

V ukázkovém kódu `UploadFiletoStorage` úloh v `Storagehelper.cs` soubor se používá k odeslání obrázky, které chcete `images` kontejneru v účtu úložiště pomocí [UploadFromStreamAsync](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblockblob.uploadfromstreamasync?view=azure-dotnet) metoda. Následující ukázka kódu obsahuje `UploadFiletoStorage` úloh. 

```csharp
public static async Task<bool> UploadFileToStorage(Stream fileStream, string fileName, AzureStorageConfig _storageConfig)
{
    // Create storagecredentials object by reading the values from the configuration (appsettings.json)
    StorageCredentials storageCredentials = new StorageCredentials(_storageConfig.AccountName, _storageConfig.AccountKey);

    // Create cloudstorage account by passing the storagecredentials
    CloudStorageAccount storageAccount = new CloudStorageAccount(storageCredentials, true);

    // Create the blob client.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    // Get reference to the blob container by passing the name by reading the value from the configuration (appsettings.json)
    CloudBlobContainer container = blobClient.GetContainerReference(_storageConfig.ImageContainer);

    // Get the reference to the block blob from the container
    CloudBlockBlob blockBlob = container.GetBlockBlobReference(fileName);

    // Upload the file
    await blockBlob.UploadFromStreamAsync(fileStream);

    return await Task.FromResult(true);
}
```

Následující třídy a metody se používají v předchozí úloze:

|– Třída  |Metoda  |
|---------|---------|
|[StorageCredentials](/dotnet/api/microsoft.windowsazure.storage.auth.storagecredentials?view=azure-dotnet)     |         |
|[CloudStorageAccount](/dotnet/api/microsoft.windowsazure.storage.cloudstorageaccount?view=azure-dotnet)    |  [CreateCloudBlobClient](/dotnet/api/microsoft.windowsazure.storage.cloudstorageaccount.createcloudblobclient?view=azure-dotnet#Microsoft_WindowsAzure_Storage_CloudStorageAccount_CreateCloudBlobClient)       |
|[CloudBlobClient](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblobclient?view=azure-dotnet)     |[GetContainerReference](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblobclient.getcontainerreference?view=azure-dotnet#Microsoft_WindowsAzure_Storage_Blob_CloudBlobClient_GetContainerReference_System_String_)         |
|[CloudBlobContainer](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblobcontainer?view=azure-dotnet)    | [GetBlockBlobReference](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblobcontainer.getblockblobreference?view=azure-dotnet#Microsoft_WindowsAzure_Storage_Blob_CloudBlobContainer_GetBlockBlobReference_System_String_)        |
|[CloudBlockBlob](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblockblob?view=azure-dotnet)     | [UploadFromStreamAsync](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblockblob.uploadfromstreamasync?view=azure-dotnet)        |

## <a name="verify-the-image-is-shown-in-the-storage-account"></a>Ověřte, že obrázek se zobrazí v účtu úložiště

Přihlaste se k https://portal.azure.com. V nabídce vlevo vyberte **účty úložiště**, pak vyberte název svého účtu úložiště. V části **přehled**, vyberte **bitové kopie** kontejneru.

Ověřte, že obrázek se zobrazí v kontejneru.

![Kontejner zobrazení obrázků](media/storage-upload-process-images/figure13.png)

## <a name="test-thumbnail-viewing"></a>Zobrazení miniatur testu

K testování zobrazení miniatur, budete nahrajte image ke kontejneru miniatur aby se zajistilo, že aplikace může číst miniatur kontejneru.

Přihlaste se k https://portal.azure.com. V nabídce vlevo vyberte **účty úložiště**, pak vyberte název svého účtu úložiště. Vyberte **kontejnery** pod **služby objektů Blob** a vyberte **palec** kontejneru. Vyberte **nahrát** otevřete **nahrávání blob** podokně.

Vyberte soubor pomocí nástroje pro výběr souborů a vyberte **nahrát**.

Přejděte zpět do své aplikace a ověřte, že obrázek nahrán do **palec** kontejner je viditelné.

![Kontejner zobrazení obrázků](media/storage-upload-process-images/figure2.png)

V **palec** kontejneru na portálu Azure vyberte bitovou kopii odeslán a vyberte **odstranit** odstranit bitovou kopii. V rámci dvě řady jsou automatické vytváření miniatur, proto tuto bitovou kopii test není potřeba.

CDN se dá nastavit obsah do mezipaměti z vašeho účtu úložiště Azure. Když nejsou popsané v tomto kurzu, se dozvíte, jak povolit CDN s vaším účtem úložiště Azure, můžete navštívit: [účet úložiště Azure integrovat Azure CDN](../../cdn/cdn-create-a-storage-account-with-cdn.md).

## <a name="next-steps"></a>Další kroky

V rámci jedna řada jste se dozvěděli o konfiguraci webové aplikace interakci s úložištěm, například jak:

> [!div class="checklist"]
> * vytvořit účet úložiště
> * Vytvořte kontejner a nastavení oprávnění
> * Načtení přístupového klíče
> * Konfigurace nastavení aplikace
> * Nasazení webové aplikace do Azure
> * Interakci s webovou aplikací

Přechodu na dva součástí řady Další informace o použití událostí mřížky k aktivaci funkce Azure ke změně velikosti obrázku.

> [!div class="nextstepaction"]
> [Použít k aktivaci funkce Azure ke změně velikosti image nahrané událostí mřížky](../../event-grid/resize-images-on-storage-blob-upload-event.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)
