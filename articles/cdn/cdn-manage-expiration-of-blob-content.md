---
title: "Spravovat konec platnosti úložiště objektů Azure Blob v Azure Content Delivery Network | Microsoft Docs"
description: "Informace o možnostech řízení time to live pro objekty BLOB v Azure CDN ukládání do mezipaměti."
services: cdn
documentationcenter: 
author: zhangmanling
manager: erikre
editor: 
ms.assetid: ad4801e9-d09a-49bf-b35c-efdc4e6034e8
ms.service: cdn
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 11/10/2017
ms.author: mazha
ms.openlocfilehash: 276b0ecd6d84725b94018e73d53555ffd9f3e9a0
ms.sourcegitcommit: 310748b6d66dc0445e682c8c904ae4c71352fef2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/28/2017
---
# <a name="manage-expiration-of-azure-blob-storage-in-azure-content-delivery-network"></a>Spravovat konec platnosti úložiště objektů Azure Blob v Azure Content Delivery Network
> [!div class="op_single_selector"]
> * [Azure webového obsahu](cdn-manage-expiration-of-cloud-service-content.md)
> * [Azure Blob Storage](cdn-manage-expiration-of-blob-content.md)
> 
> 

[Služba úložiště objektů Blob](../storage/common/storage-introduction.md#blob-storage) ve službě Azure Storage je jedním z několika Azure na základě původu integrované s Azure Content Delivery Network (CDN). Veškerý obsah, veřejně přístupná objektů blob můžete v Azure CDN do mezipaměti, dokud uplynutí jeho time to live (TTL). Hodnota TTL je dáno `Cache-Control` hlavičku HTTP odpovědi ze zdrojového serveru. Tento článek popisuje několik způsobů, které můžete nastavit `Cache-Control` záhlaví u objektu blob ve službě Azure Storage.

> [!TIP]
> Můžete nastavit žádné TTL pro objekt blob. V takovém případě Azure CDN automaticky použije výchozí hodnotu TTL sedm dní. Toto výchozí nastavení TTL se vztahuje pouze na obecné webové doručení optimalizace. Pro optimalizace na velkých souborů je výchozí hodnota TTL je jeden den a pro streamování optimalizace médií, je výchozí hodnota TTL je jeden rok.
> 
> Další informace o tom, jak funguje Azure CDN pro urychlení přístupu k objektům BLOB a další soubory, najdete v části [přehled Azure Content Delivery Network](cdn-overview.md).
> 
> Další informace o Azure Blob storage najdete v tématu [Úvod do úložiště objektů Blob](https://docs.microsoft.com/en-us/azure/storage/blobs/storage-blobs-introduction).
 

## <a name="setting-cache-control-headers-by-using-azure-powershell"></a>Nastavení hlavičky Cache-Control pomocí prostředí Azure PowerShell
[Prostředí Azure PowerShell](/powershell/azure/overview) je jedním z nejrychlejší a nejúčinnějších způsobů, jak spravovat služeb Azure. Použití `Get-AzureStorageBlob` rutiny odkazovat na objekt blob, nastavte `.ICloudBlob.Properties.CacheControl` vlastnost. 

Například:

```powershell
# Create a storage context
$context = New-AzureStorageContext -StorageAccountName "<storage account name>" -StorageAccountKey "<storage account key>"

# Get a reference to the blob
$blob = Get-AzureStorageBlob -Context $context -Container "<container name>" -Blob "<blob name>"

# Set the CacheControl property to expire in 1 hour (3600 seconds)
$blob.ICloudBlob.Properties.CacheControl = "public, max-age=3600"

# Send the update to the cloud
$blob.ICloudBlob.SetProperties()
```

> [!TIP]
> Můžete taky použít PowerShell k [spravovat koncové body a profilů CDN](cdn-manage-powershell.md).
> 
>

## <a name="setting-cache-control-headers-by-using-net"></a>Nastavení hlavičky Cache-Control pomocí rozhraní .NET
Nastavit objekt blob `Cache-Control` záhlaví pomocí rozhraní .NET kódu, použijte [Klientská knihovna pro úložiště Azure pro .NET](../storage/blobs/storage-dotnet-how-to-use-blobs.md) nastavit [CloudBlob.Properties.CacheControl](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.blob.blobproperties.cachecontrol.aspx) vlastnost.

Například:

```csharp
class Program
{
    const string connectionString = "<storage connection string>";
    static void Main()
    {
        // Retrieve storage account information from connection string
        CloudStorageAccount storageAccount = CloudStorageAccount.Parse(connectionString);

        // Create a blob client for interacting with the blob service.
        CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

        // Create a reference to the container
        CloudBlobContainer container = blobClient.GetContainerReference("<container name>");

        // Create a reference to the blob
        CloudBlob blob = container.GetBlobReference("<blob name>");

        // Set the CacheControl property to expire in 1 hour (3600 seconds)
        blob.Properties.CacheControl = "public, max-age=3600";

        // Update the blob's properties in the cloud
        blob.SetProperties();
    }
}
```

> [!TIP]
> Další ukázky kódu .NET jsou k dispozici v [ukázky úložiště objektů Blob Azure pro .NET](https://azure.microsoft.com/documentation/samples/storage-blob-dotnet-getting-started/).
> 

## <a name="setting-cache-control-headers-by-using-other-methods"></a>Nastavení hlavičky Cache-Control pomocí jiných metod

### <a name="azure-storage-explorer"></a>Azure Storage Explorer
S [Azure Storage Explorer](https://azure.microsoft.com/en-us/features/storage-explorer/), můžete zobrazit a upravit vaše prostředky úložiště objektů blob, včetně vlastnosti, jako *CacheControl* vlastnost. 

Chcete-li aktualizovat *CacheControl* vlastnost objektu blob s Azure Storage Explorer:
   1. Vyberte objekt blob a potom vyberte **vlastnosti** v místní nabídce. 
   2. Přejděte dolů k položce *CacheControl* vlastnost.
   3. Zadejte hodnotu a pak klikněte na **Uložit**.


![Azure Storage Explorer vlastnosti](./media/cdn-manage-expiration-of-blob-content/cdn-storage-explorer-properties.png)

### <a name="azure-command-line-interface"></a>Rozhraní příkazového řádku Azure
Pokud jste nahrát objekt blob, můžete nastavit *cacheControl* vlastnost s `-p` přepínač ve [rozhraní příkazového řádku Azure](../cli-install-nodejs.md). Následující příklad ukazuje, jak nastavit hodnotu TTL na jednu hodinu (3600 sekund):
  
    ```text
    azure storage blob upload -c <connectionstring> -p cacheControl="public, max-age=3600" .\test.txt myContainer test.txt
    ```

### <a name="azure-storage-services-rest-api"></a>Azure storage services REST API
Můžete použít [Azure storage services REST API](https://msdn.microsoft.com/library/azure/dd179355.aspx) explicitně nastavit *x-ms-blob-cache-control* vlastnost pomocí následující operace na vyžádání:
  
   - [Uveďte objektů Blob](https://msdn.microsoft.com/en-us/library/azure/dd179451.aspx)
   - [Uveďte seznam blokovaných položek](https://msdn.microsoft.com/en-us/library/azure/dd179467.aspx)
   - [Nastavit vlastnosti objektů Blob](https://msdn.microsoft.com/library/azure/ee691966.aspx)

## <a name="testing-the-cache-control-header"></a>Testování hlavička Cache-Control
Snadno můžete ověřit nastavení TTL objektů BLOB. V prohlížeči [nástroje pro vývojáře](https://developer.microsoft.com/microsoft-edge/platform/documentation/f12-devtools-guide/), test, který zahrnuje objektu blob služby `Cache-Control` hlavičky odpovědi. Můžete také použít nástroj jako [Wget](https://www.gnu.org/software/wget/), [Postman](https://www.getpostman.com/), nebo [Fiddler](http://www.telerik.com/fiddler) a prověří hlavičky odpovědi.

## <a name="next-steps"></a>Další kroky
* [Zjistěte, jak spravovat platnost obsahu cloudové služby v Azure CDN](cdn-manage-expiration-of-cloud-service-content.md)

