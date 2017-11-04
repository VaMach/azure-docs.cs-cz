---
title: "Začínáme s Azure Storage zásobníku nástroje pro vývoj"
description: "Pokyny, jak začít s pomocí nástrojů pro vývoj Azure zásobník úložiště"
services: azure-stack
author: xiaofmao
ms.author: xiaofmao
ms.date: 9/25/2017
ms.topic: get-started-article
ms.service: azure-stack
ms.openlocfilehash: 5b2898c64c0f1b5d804e63fa4e4e1218fa7a672c
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="get-started-with-azure-stack-storage-development-tools"></a>Začínáme s Azure Storage zásobníku nástroje pro vývoj

*Platí pro: Azure zásobníku integrované systémy a Azure zásobníku Development Kit*


Zásobník Microsoft Azure poskytuje sadu služeb úložiště, včetně úložiště objektů Azure Blob, Table a Queue.

Tento článek obsahuje rychlý pokyny o tom, jak začít používat nástroje pro vývoj zásobník úložiště Azure. Podrobnější informace a ukázkový kód můžete najít v odpovídající kurzech Azure Storage.

Existují známé rozdíly mezi Azure Storage a zásobník úložiště Azure, včetně některé specifické požadavky na každou platformu. Například existují určité klientské knihovny a konkrétní koncový bod příponu požadavky pro Azure zásobníku. Další informace najdete v tématu [zásobník úložiště Azure: rozdíly a aspekty](azure-stack-acs-differences.md).

## <a name="azure-client-libraries"></a>Knihovny klienta Azure
Podporované verze rozhraní REST API pro Azure zásobníku úložiště je 2015-04-05. Nemá úplné parita s nejnovější verzi rozhraní API REST úložiště Azure. Proto pro knihovny klienta úložiště, musíte znát verze, která je kompatibilní s REST API 2015-04-05.


|Klientská knihovna|Azure zásobníku podporovaná verze|Odkaz|Koncový bod specifikace|
|---------|---------|---------|---------|
|.NET     |6.2.0|Balíček Nuget:<br>[https://www.nuget.org/Packages/WindowsAzure.Storage/6.2.0](https://www.nuget.org/packages/WindowsAzure.Storage/6.2.0)<br><br>Verze Githubu:<br>[https://github.com/Azure/Azure-Storage-NET/releases/tag/v6.2.1](https://github.com/Azure/azure-storage-net/releases/tag/v6.2.1)|soubor App.config|
|Java|4.1.0|Balíček maven:<br>[http://mvnrepository.com/Artifact/com.microsoft.Azure/Azure-Storage/4.1.0](http://mvnrepository.com/artifact/com.microsoft.azure/azure-storage/4.1.0)<br><br>Verze Githubu:<br> [https://github.com/Azure/Azure-Storage-Java/releases/tag/v4.1.0](https://github.com/Azure/azure-storage-java/releases/tag/v4.1.0)|Nastavení připojení řetězce|
|Node.js     |1.1.0|NPM odkaz:<br>[https://www.npmjs.com/Package/Azure-Storage](https://www.npmjs.com/package/azure-storage)<br>(spustit:`npm install azure-storage@1.1.0)`<br><br>Verze Githubu:<br>[https://github.com/Azure/Azure-Storage-node/releases/tag/1.1.0](https://github.com/Azure/azure-storage-node/releases/tag/1.1.0)|Deklarace instance služby||C++|2.4.0|Balíček Nuget:<br>[https://www.nuget.org/Packages/wastorage.v140/2.4.0](https://www.nuget.org/packages/wastorage.v140/2.4.0)<br><br>Verze Githubu:<br>[https://github.com/Azure/Azure-Storage-cpp/releases/tag/v2.4.0](https://github.com/Azure/azure-storage-cpp/releases/tag/v2.4.0)|Nastavení připojení řetězce|
|C++|2.4.0|Balíček Nuget:<br>[https://www.nuget.org/Packages/wastorage.v140/2.4.0](https://www.nuget.org/packages/wastorage.v140/2.4.0)<br><br>Verze Githubu:<br>[https://github.com/Azure/Azure-Storage-cpp/releases/tag/v2.4.0](https://github.com/Azure/azure-storage-cpp/releases/tag/v2.4.0)|Nastavení připojení řetězce|
|PHP|0.15.0|Verze Githubu:<br>[https://github.com/Azure/Azure-Storage-PHP/releases/tag/V0.15.0](https://github.com/Azure/azure-storage-php/releases/tag/v0.15.0)<br><br>Nainstalovat prostřednictvím autora (viz níže podrobnosti)|Nastavení připojení řetězce|
|Python     |0.30.0|Balíček PIP:<br> [https://pypi.Python.org/pypi/Azure-Storage/0.30.0](https://pypi.python.org/pypi/azure-storage/0.30.0)<br>(Spustit:`pip install -v azure-storage==0.30.0)`<br><br>Verze Githubu:<br> [https://github.com/Azure/Azure-Storage-Python/releases/tag/V0.30.0](https://github.com/Azure/azure-storage-python/releases/tag/v0.30.0)|Deklarace instance služby|
|Ruby|0.12.1<br>Preview|Balíček RubyGems:<br> [https://rubygems.org/GEMS/Azure-Storage/versions/0.12.1.Preview](https://rubygems.org/gems/azure-storage/versions/0.12.1.preview)<br><br>Verze Githubu:<br> [https://github.com/Azure/Azure-Storage-Ruby/releases/tag/V0.12.1](https://github.com/Azure/azure-storage-ruby/releases/tag/v0.12.1)|Nastavení připojení řetězce|

> [!NOTE]
> Podrobnosti o PHP<br><br>
>Chcete-li nainstalovat prostřednictvím autora:
>1. Vytvořte soubor s názvem `composer.json` v kořenu projektu s následujícím kódem:<br>
>
>   ```
>   {
>       "require":{
>           "Microsoft/azure-storage":"0.15.0"
>        }
>    }
>   ```
>
>2. Stáhněte si [composer.phar](http://getcomposer.org/composer.phar) do kořenu projektu.
>3. Spustit: `php composer.phar install`.
>


## <a name="endpoint-declaration"></a>Koncový bod deklarace
Koncový bod Azure Stack zahrnuje dvě části: název, oblast a doména Azure zásobníku.
V sadě Azure zásobníku Development Kit je výchozí koncový bod **local.azurestack.external**.
Pokud si nejste jistí o váš koncový bod, obraťte se na správce cloudu.

## <a name="examples"></a>Příklady


### <a name="net"></a>.NET

Pro Azure zásobníku zadána přípona koncový bod v souboru app.config:

```
<add key="StorageConnectionString" 
value="DefaultEndpointsProtocol=https;AccountName=myaccount;AccountKey=mykey;
EndpointSuffix=local.azurestack.external;" />
```
### <a name="java"></a>Java

Pro Azure zásobníku zadána přípona koncového bodu v nastavení připojovacího řetězce:

```
public static final String storageConnectionString =
    "DefaultEndpointsProtocol=http;" +
    "AccountName=your_storage_account;" +
    "AccountKey=your_storage_account_key;" +
    "EndpointSuffix=local.azurestack.external";
```

### <a name="nodejs"></a>Node.js

Pro Azure zásobníku zadána přípona koncového bodu v deklaraci instance:

```
var blobSvc = azure.createBlobService('myaccount', 'mykey',
'myaccount.blob.local.azurestack.external');
```
### <a name="c"></a>C++

Pro Azure zásobníku zadána přípona koncového bodu v nastavení připojovacího řetězce:

```
const utility::string_t storage_connection_string(U("DefaultEndpointsProtocol=https;
AccountName=your_storage_account;
AccountKey=your_storage_account_key;
EndpointSuffix=local.azurestack.external"));
```

### <a name="php"></a>PHP

Pro Azure zásobníku zadána přípona koncového bodu v nastavení připojovacího řetězce:

```
$connectionString = 'BlobEndpoint=http://<storage account name>.blob.local.azurestack.external/;
QueueEndpoint=http:// <storage account name>.queue.local.azurestack.external/;
TableEndpoint=http:// <storage account name>.table.local.azurestack.external/;
AccountName=<storage account name>;AccountKey=<storage account key>'
```

### <a name="python"></a>Python

Pro Azure zásobníku zadána přípona koncového bodu v deklaraci instance:

```
block_blob_service = BlockBlobService(account_name='myaccount',
account_key='mykey',
endpoint_suffix='local.azurestack.external')
```
### <a name="ruby"></a>Ruby

Pro Azure zásobníku zadána přípona koncového bodu v nastavení připojovacího řetězce:

```
set
AZURE_STORAGE_CONNECTION_STRING=DefaultEndpointsProtocol=https;
AccountName=myaccount;
AccountKey=mykey;
EndpointSuffix=local.azurestack.external
```

## <a name="blob-storage"></a>Blob Storage

Následující kurzy úložiště objektů Blob v Azure se vztahují na Azure zásobníku. Poznámka: požadavek příponu konkrétní koncový bod Azure zásobníku popsané v předchozí [příklady](#examples) části.

* [Začínáme s úložištěm Azure Blob pomocí rozhraní .NET](../../storage/blobs/storage-dotnet-how-to-use-blobs.md)
* [Používání úložiště Blob z Javy](../../storage/blobs/storage-java-how-to-use-blob-storage.md)
* [Jak používat úložiště objektů Blob z Node.js]... /.. / storage/blobs/storage-nodejs-how-to-use-blob-storage.md)
* [Používání úložiště Blob z jazyka C++](../../storage/blobs/storage-c-plus-plus-how-to-use-blobs.md)
* [Používání úložiště Blob z PHP](../../storage/blobs/storage-php-how-to-use-blobs.md)
* [Jak používat Azure Blob storage z Pythonu](../../storage/blobs/storage-python-how-to-use-blob-storage.md)
* [Používání úložiště Blob z Ruby](../../storage/blobs/storage-ruby-how-to-use-blob-storage.md)

## <a name="queue-storage"></a>Queue Storage

Následující kurzy Azure Queue storage se vztahují na Azure zásobníku. Poznámka: požadavek příponu konkrétní koncový bod Azure zásobníku popsané v předchozí [příklady](#examples) části.

* [Začínáme s úložištěm Azure Queue pomocí rozhraní .NET](../../storage/queues/storage-dotnet-how-to-use-queues.md)
* [Používání úložiště Queue z Javy](../../storage/queues/storage-java-how-to-use-queue-storage.md)
* [Používání úložiště Queue z Node.js](../../storage/queues/storage-nodejs-how-to-use-queues.md)
* [Postup používání úložiště Queue z jazyka C++](../../storage/queues/storage-c-plus-plus-how-to-use-queues.md)
* [Používání úložiště Queue z PHP](../../storage/queues/storage-php-how-to-use-queues.md)
* [Používání úložiště Queue z Pythonu](../../storage/queues/storage-python-how-to-use-queue-storage.md)
* [Používání úložiště Queue z Ruby](../../storage/queues/storage-ruby-how-to-use-queue-storage.md)


## <a name="table-storage"></a>Úložiště Table

Následující kurzy úložiště Azure Table se vztahují na Azure zásobníku. Poznámka: požadavek příponu konkrétní koncový bod Azure zásobníku popsané v předchozí [příklady](#examples) části.

* [Začínáme s úložištěm Azure Table pomocí rozhraní .NET](../../cosmos-db/table-storage-how-to-use-dotnet.md)
* [Používání úložiště Table z Javy](../../cosmos-db/table-storage-how-to-use-java.md)
* [Používání úložiště Azure Table z Node.js](../../cosmos-db/table-storage-how-to-use-nodejs.md)
* [Postup používání úložiště Table z jazyka C++](../../cosmos-db/table-storage-how-to-use-c-plus.md)
* [Používání úložiště Table z PHP](../../cosmos-db/table-storage-how-to-use-php.md)
* [Postup používání úložiště Table v Pythonu](../../cosmos-db/table-storage-how-to-use-python.md)
* [Používání úložiště Table z Ruby](../../cosmos-db/table-storage-how-to-use-ruby.md)

## <a name="next-steps"></a>Další kroky

* [Úvod do Microsoft Azure Storage](../../storage/common/storage-introduction.md)