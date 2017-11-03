---
title: "Spravovat konec platnosti objektů BLOB Azure Storage v Azure CDN | Microsoft Docs"
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
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: d4741921806e443d92c385a04b781cec296c2ae8
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="manage-expiration-of-azure-storage-blobs-in-azure-cdn"></a>Spravovat konec platnosti objektů BLOB Azure Storage v Azure CDN
> [!div class="op_single_selector"]
> * [Službě Azure Web Apps nebo cloudové služby, ASP.NET nebo služby IIS](cdn-manage-expiration-of-cloud-service-content.md)
> * [Služba Azure blob Storage](cdn-manage-expiration-of-blob-content.md)
> 
> 

[Služba objektů blob](../storage/common/storage-introduction.md#blob-storage) v [Azure Storage](../storage/common/storage-introduction.md) je jedním z několika Azure na základě původu integrované s Azure CDN.  Veškerý obsah, veřejně přístupná objektů blob můžete v Azure CDN do mezipaměti, dokud uplynutí jeho time to live (TTL).  Hodnota TTL je dáno [ *Cache-Control* záhlaví](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.9) v odpovědi HTTP z úložiště Azure.

> [!TIP]
> Můžete nastavit žádné TTL na objekt blob.  V takovém případě Azure CDN automaticky použije výchozí hodnotu TTL sedm dní.
> 
> Další informace o tom, jak funguje Azure CDN pro urychlení přístupu k objektům BLOB a další soubory, najdete v článku [přehled CDN Azure](cdn-overview.md).
> 
> Další informace o objektu blob služby Azure Storage najdete v tématu [koncepty služby objektů Blob](https://msdn.microsoft.com/library/dd179376.aspx). 
> 
> 

Tento kurz představuje několik způsobů, které můžete nastavit hodnotu TTL u objektu blob ve službě Azure Storage.  

## <a name="azure-powershell"></a>Azure PowerShell
[Prostředí Azure PowerShell](/powershell/azure/overview) je jedním z nejrychlejší, nejúčinnějších způsobů, jak spravovat služeb Azure.  Použití `Get-AzureStorageBlob` rutiny odkazovat na objekt blob, nastavte `.ICloudBlob.Properties.CacheControl` vlastnost. 

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

## <a name="azure-storage-client-library-for-net"></a>Klientská knihovna pro úložiště Azure pro .NET
Pokud chcete nastavit hodnotu TTL objekt blob pomocí rozhraní .NET, použijte [Klientská knihovna pro úložiště Azure pro .NET](../storage/blobs/storage-dotnet-how-to-use-blobs.md) nastavit [CloudBlob.Properties.CacheControl](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.blob.blobproperties.cachecontrol.aspx) vlastnost.

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
> Nejsou k dispozici v mnoha další ukázek kódu .NET [ukázky úložiště objektů Blob Azure pro .NET](https://azure.microsoft.com/documentation/samples/storage-blob-dotnet-getting-started/).
> 
> 

## <a name="other-methods"></a>Ostatní metody
* [Rozhraní příkazového řádku Azure](../cli-install-nodejs.md)
  
    Pokud chcete nahrát objekt blob, nastavit *cacheControl* pomocí vlastnosti `-p` přepínače.  Tento příklad nastaví hodnotu TTL na jednu hodinu (3600 sekund).
  
    ```text
    azure storage blob upload -c <connectionstring> -p cacheControl="public, max-age=3600" .\test.txt myContainer test.txt
    ```
* [REST API služby Azure Storage](https://msdn.microsoft.com/library/azure/dd179355.aspx)
  
    Explicitně nastavit *x-ms-blob-cache-control* vlastnost na [Put Blob](https://msdn.microsoft.com/en-us/library/azure/dd179451.aspx), [uvést seznam blokovaných](https://msdn.microsoft.com/en-us/library/azure/dd179467.aspx), nebo [nastavit vlastnosti objektu Blob](https://msdn.microsoft.com/library/azure/ee691966.aspx) požadavku.
* Nástroje pro správu úložiště třetí strany.
  
    Některé nástroje pro správu Azure úložiště jiných výrobců povolit nastavení *CacheControl* vlastnost na objekty BLOB. 

## <a name="testing-the-cache-control-header"></a>Testování *Cache-Control* záhlaví
Snadno můžete ověřit hodnoty TTL objektů BLOB.  Pomocí prohlížeče [nástroje pro vývojáře](https://developer.microsoft.com/microsoft-edge/platform/documentation/f12-devtools-guide/), test, který je včetně objektu blob služby *Cache-Control* hlavičky odpovědi.  Můžete použít také nástroje, jako je **wget**, [Postman](https://www.getpostman.com/), nebo [Fiddler](http://www.telerik.com/fiddler) a prověří hlavičky odpovědi.

## <a name="next-steps"></a>Další kroky
* [Přečtěte si informace o *Cache-Control* záhlaví](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.9)
* [Zjistěte, jak spravovat platnost obsahu cloudové služby v Azure CDN](cdn-manage-expiration-of-cloud-service-content.md)

