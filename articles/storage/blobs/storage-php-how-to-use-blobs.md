---
title: "Jak používat úložiště objektů blob (úložiště objektů) z PHP | Microsoft Docs"
description: "Ukládejte nestrukturovaná data v cloudu pomocí Azure Blob Storage (úložiště objektů)."
documentationcenter: php
services: storage
author: tamram
manager: timlt
editor: tysonn
ms.assetid: 1af56b59-b3f0-4b46-8441-aab463ae088e
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: PHP
ms.topic: article
ms.date: 12/08/2016
ms.author: tamram
ms.openlocfilehash: 9de2f7e81d75669267fe6448030c118d06b3f88a
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-use-blob-storage-from-php"></a>Používání úložiště blob z PHP
[!INCLUDE [storage-selector-blob-include](../../../includes/storage-selector-blob-include.md)]

[!INCLUDE [storage-try-azure-tools-queues](../../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a>Přehled
Úložiště objektů blob v Azure je služba, která ukládá nestrukturovaná data v cloudu jako objekty nebo objekty blob. Do Blob storage se dá ukládat jakýkoli druh textu nebo binárních dat, jako je dokument, soubor médií nebo instalátor aplikace. Blob storage se také nazývá úložiště objektů.

Tento průvodce vám ukáže, jak provádět běžné scénáře s využitím služby Azure blob. Ukázky jsou napsané v PHP a použití [Klientská knihovna pro úložiště Azure pro jazyk PHP][download]. Pokryté scénáře zahrnují **odesílání**, **výpis**, **stahování**, a **odstraňování** objekty BLOB. Další informace o objekty BLOB, najdete v článku [další kroky](#next-steps) části.

[!INCLUDE [storage-blob-concepts-include](../../../includes/storage-blob-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../../includes/storage-create-account-include.md)]

## <a name="create-a-php-application"></a>Vytvoření aplikace PHP
Jediný požadavek pro vytvoření aplikace PHP, který přistupuje k službě Azure blob je odkazující na třídy v [Klientská knihovna pro úložiště Azure pro jazyk PHP] [ download] z vašeho kódu. Všechny nástroje pro vývoj slouží k vytvoření aplikace, včetně Poznámkový blok.

V této příručce používat funkce služby úložiště objektů Blob, které může být volána v rámci aplikace PHP místně nebo v kódu běžící v rámci webové role Azure, role pracovního procesu nebo webu.

## <a name="get-the-azure-client-libraries"></a>Získat knihoven klienta Azure
[!INCLUDE [get-client-libraries](../../../includes/get-client-libraries.md)]

## <a name="configure-your-application-to-access-the-blob-service"></a>Konfigurace aplikace pro přístup ke službě blob
Pomocí rozhraní API služby objektů blob v Azure, budete muset:

1. Reference souboru pomocí automatického zavaděče [require_once] příkaz, a
2. Referenční všechny třídy, které můžete použít.

Následující příklad ukazuje, jak se zahrnuje automatického zavaděče souboru a odkaz **ServicesBuilder** třídy.

```php
require_once 'vendor/autoload.php';
use MicrosoftAzure\Storage\Common\ServicesBuilder;
```

V následujících příkladech `require_once` příkazu se zobrazí vždy, ale jenom ty třídy potřebné pro tento příklad provést odkazují.

## <a name="set-up-an-azure-storage-connection"></a>Nastavit připojení k úložišti Azure
K vytvoření instance klienta služby Azure blob, Nejdřív musíte mít platný připojovací řetězec. Formát pro připojovací řetězec služby objektů blob je:

Pro přístup k službě za provozu:

```php
DefaultEndpointsProtocol=[http|https];AccountName=[yourAccount];AccountKey=[yourKey]
```

Pro přístup k emulátoru úložiště:

```php
UseDevelopmentStorage=true
```

Pokud chcete vytvořit libovolného klienta služby Azure, budete muset použít **ServicesBuilder** třídy. Můžete:

* Připojovací řetězec přímo jí předat nebo
* Připojovací řetězec uložit pomocí proměnné prostředí ve vaší webové aplikaci. V tématu [nastavení konfigurace Azure webové aplikace](../../app-service/web-sites-configure.md) dokumentu pro konfiguraci připojovacího řetězce.

Příklady podle zde uvedeného je předaná přímo připojovací řetězec.

```php
require_once 'vendor/autoload.php';

use MicrosoftAzure\Storage\Common\ServicesBuilder;

$connectionString = "DefaultEndpointsProtocol=http;AccountName=<accountNameHere>;AccountKey=<accountKeyHere>";

$blobRestProxy = ServicesBuilder::getInstance()->createBlobService($connectionString);
```

## <a name="create-a-container"></a>Vytvoření kontejneru
[!INCLUDE [storage-container-naming-rules-include](../../../includes/storage-container-naming-rules-include.md)]

A **BlobRestProxy** objektu umožňuje vytvářet kontejner objektů blob s **createContainer** metoda. Při vytváření kontejneru, můžete nastavit možnosti v kontejneru, ale to tak není povinný. (Následující příklad ukazuje, jak nastavit kontejner seznam řízení přístupu (ACL) a metadata kontejneru.)

```php
require_once 'vendor\autoload.php';

use MicrosoftAzure\Storage\Common\ServicesBuilder;
use MicrosoftAzure\Storage\Blob\Models\CreateContainerOptions;
use MicrosoftAzure\Storage\Blob\Models\PublicAccessType;
use MicrosoftAzure\Storage\Common\ServiceException;

$connectionString = "DefaultEndpointsProtocol=http;AccountName=<accountNameHere>;AccountKey=<accountKeyHere>";

// Create blob REST proxy.
$blobRestProxy = ServicesBuilder::getInstance()->createBlobService($connectionString);


// OPTIONAL: Set public access policy and metadata.
// Create container options object.
$createContainerOptions = new CreateContainerOptions();

// Set public access policy. Possible values are
// PublicAccessType::CONTAINER_AND_BLOBS and PublicAccessType::BLOBS_ONLY.
// CONTAINER_AND_BLOBS:
// Specifies full public read access for container and blob data.
// proxys can enumerate blobs within the container via anonymous
// request, but cannot enumerate containers within the storage account.
//
// BLOBS_ONLY:
// Specifies public read access for blobs. Blob data within this
// container can be read via anonymous request, but container data is not
// available. proxys cannot enumerate blobs within the container via
// anonymous request.
// If this value is not specified in the request, container data is
// private to the account owner.
$createContainerOptions->setPublicAccess(PublicAccessType::CONTAINER_AND_BLOBS);

// Set container metadata.
$createContainerOptions->addMetaData("key1", "value1");
$createContainerOptions->addMetaData("key2", "value2");

try    {
    // Create container.
    $blobRestProxy->createContainer("mycontainer", $createContainerOptions);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // http://msdn.microsoft.com/library/azure/dd179439.aspx
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

Volání metody **setPublicAccess (PublicAccessType::CONTAINER\_a\_objektů BLOB)** zpřístupní data kontejnerů a objektů blob prostřednictvím anonymních požadavků. Volání metody **setPublicAccess(PublicAccessType::BLOBS_ONLY)** díky pouze blob dat, které jsou přístupné prostřednictvím anonymních požadavků. Další informace o kontejneru seznamy řízení přístupu najdete v tématu [sadu kontejneru seznamu řízení přístupu (REST API)][container-acl].

Další informace o chybových kódech služby objektů Blob najdete v tématu [kódy chyb služby objektů Blob][error-codes].

## <a name="upload-a-blob-into-a-container"></a>Nahrání objektu blob do kontejneru
Pokud chcete nahrát soubor jako objekt blob, použijte **BlobRestProxy -> createBlockBlob** metoda. Tato operace vytvoří objekt blob, pokud neexistuje, nebo ho přepíše, pokud k tomu. Následující příklad kódu předpokládá, že kontejner byl vytvořen a používá [fopen –] [ fopen] k otevření souboru jako datový proud.

```php
require_once 'vendor/autoload.php';

use MicrosoftAzure\Storage\Common\ServicesBuilder;
use MicrosoftAzure\Storage\Common\ServiceException;

$connectionString = "DefaultEndpointsProtocol=http;AccountName=<accountNameHere>;AccountKey=<accountKeyHere>";

// Create blob REST proxy.
$blobRestProxy = ServicesBuilder::getInstance()->createBlobService($connectionString);

$content = fopen("c:\myfile.txt", "r");
$blob_name = "myblob";

try    {
    //Upload blob
    $blobRestProxy->createBlockBlob("mycontainer", $blob_name, $content);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // http://msdn.microsoft.com/library/azure/dd179439.aspx
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

Všimněte si, že v předchozím příkladu odešle objekt blob jako datový proud. Ale objekt blob je možné také uložit jako řetězec, pomocí, například [soubor\_získat\_obsah] [ file_get_contents] funkce. Chcete-li to provést, pomocí v předchozím příkladu, změňte `$content = fopen("c:\myfile.txt", "r");` k `$content = file_get_contents("c:\myfile.txt");`.

## <a name="list-the-blobs-in-a-container"></a>Zobrazí seznam objektů blob v kontejneru
K zobrazení seznamu objektů BLOB v kontejneru, použijte **BlobRestProxy -> listBlobs** metoda s **foreach** smyčky do cyklus prostřednictvím výsledek. Následující kód zobrazí název jednotlivých objektů blob jako výstup v kontejneru a jeho identifikátoru URI do prohlížeče.

```php
require_once 'vendor/autoload.php';

use MicrosoftAzure\Storage\Common\ServicesBuilder;
use MicrosoftAzure\Storage\Common\ServiceException;

// Create blob REST proxy.
$blobRestProxy = ServicesBuilder::getInstance()->createBlobService($connectionString);

try    {
    // List blobs.
    $blob_list = $blobRestProxy->listBlobs("mycontainer");
    $blobs = $blob_list->getBlobs();

    foreach($blobs as $blob)
    {
        echo $blob->getName().": ".$blob->getUrl()."<br />";
    }
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // http://msdn.microsoft.com/library/azure/dd179439.aspx
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

## <a name="download-a-blob"></a>Stažení objektu blob
Chcete-li stáhnout objekt blob, volejte **BlobRestProxy -> getblob –** metoda, pak volání **getContentStream** metodu výsledná **GetBlobResult** objektu.

```php
require_once 'vendor/autoload.php';

use MicrosoftAzure\Storage\Common\ServicesBuilder;
use MicrosoftAzure\Storage\Common\ServiceException;

// Create blob REST proxy.
$blobRestProxy = ServicesBuilder::getInstance()->createBlobService($connectionString);


try    {
    // Get blob.
    $blob = $blobRestProxy->getBlob("mycontainer", "myblob");
    fpassthru($blob->getContentStream());
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // http://msdn.microsoft.com/library/azure/dd179439.aspx
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

Všimněte si, že v předchozím příkladu získá objekt blob jako prostředek datového proudu (výchozí nastavení). Můžete však použít [datového proudu\_získat\_obsah] [ stream-get-contents] funkce vráceném datovém proudu převést na řetězec.

## <a name="delete-a-blob"></a>Odstranění objektu blob
Chcete-li odstranit objekt blob, předejte název kontejneru a název objektu blob na **BlobRestProxy -> deleteBlob**.

```php
require_once 'vendor/autoload.php';

use MicrosoftAzure\Storage\Common\ServicesBuilder;
use MicrosoftAzure\Storage\Common\ServiceException;

// Create blob REST proxy.
$blobRestProxy = ServicesBuilder::getInstance()->createBlobService($connectionString);

try    {
    // Delete blob.
    $blobRestProxy->deleteBlob("mycontainer", "myblob");
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // http://msdn.microsoft.com/library/azure/dd179439.aspx
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

## <a name="delete-a-blob-container"></a>Odstranit kontejner objektů blob
Nakonec pokud chcete odstranit kontejner objektů blob, předat název kontejneru, který má **BlobRestProxy -> deleteContainer**.

```php
require_once 'vendor/autoload.php';

use MicrosoftAzure\Storage\Common\ServicesBuilder;
use MicrosoftAzure\Storage\Common\ServiceException;

// Create blob REST proxy.
$blobRestProxy = ServicesBuilder::getInstance()->createBlobService($connectionString);

try    {
    // Delete container.
    $blobRestProxy->deleteContainer("mycontainer");
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // http://msdn.microsoft.com/library/azure/dd179439.aspx
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

## <a name="next-steps"></a>Další kroky
Teď, když jste se naučili základy služby Azure blob, postupujte podle následujících odkazech na další informace o složitějších úlohách úložiště.

* Přejděte [referenční dokumentace rozhraní API pro klientská knihovna pro úložiště Azure PHP](http://azure.github.io/azure-storage-php/)
* Najdete v článku [Advanced Blob příklad](https://github.com/Azure/azure-storage-php/blob/master/samples/BlobSamples.php).

Další informace naleznete také [středisku pro vývojáře PHP](/develop/php/).

[download]: https://github.com/Azure/azure-storage-php
[container-acl]: http://msdn.microsoft.com/library/azure/dd179391.aspx
[error-codes]: http://msdn.microsoft.com/library/azure/dd179439.aspx
[file_get_contents]: http://php.net/file_get_contents
[require_once]: http://php.net/require_once
[fopen]: http://www.php.net/fopen
[stream-get-contents]: http://www.php.net/stream_get_contents
