---
title: "Používání úložiště table z PHP | Microsoft Docs"
description: "Další informace o použití služby Table z PHP vytvářet a odstraňovat tabulky, vložení, odstranit a dotaz tabulku."
services: cosmos-db
documentationcenter: php
author: mimig1
manager: jhubbard
editor: tysonn
ms.assetid: 1e57f371-6208-4753-b2a0-05db4aede8e3
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: php
ms.topic: article
ms.date: 11/03/2017
ms.author: mimig
ms.openlocfilehash: 7fa82875ba823f1a4a9a886d4f699ca6757c3a3b
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/01/2018
---
# <a name="how-to-use-azure-table-storage-from-php"></a>Používání úložiště Azure Table z PHP
[!INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]
[!INCLUDE [storage-table-cosmos-db-langsoon-tip-include](../../includes/storage-table-cosmos-db-langsoon-tip-include.md)]

## <a name="overview"></a>Přehled
Tento průvodce vám ukáže, jak provádět běžné scénáře s využitím služby Azure Table. Ukázky jsou napsané v PHP a použití [Azure SDK pro jazyk PHP][download]. Pokryté scénáře zahrnují **vytváření a odstraňování tabulek a vkládání, odstraňování a dotazování entity v tabulce**. Další informace o službě Azure Table, najdete v článku [další kroky](#next-steps) části.

[!INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-php-application"></a>Vytvoření aplikace PHP
Jediný požadavek pro vytvoření aplikace PHP, který přistupuje k službě Azure Table je odkazování třídy v sadě Azure SDK pro jazyk PHP z vašeho kódu. Všechny nástroje pro vývoj slouží k vytvoření aplikace, včetně Poznámkový blok.

V této příručce používat funkce služby tabulky, které lze volat z v rámci aplikace PHP místně nebo v kódu běžící v rámci webové role Azure, role pracovního procesu nebo webu.

## <a name="get-the-azure-client-libraries"></a>Získat knihoven klienta Azure
[!INCLUDE [get-client-libraries](../../includes/get-client-libraries.md)]

## <a name="configure-your-application-to-access-the-table-service"></a>Konfigurace aplikace pro přístup ke službě tabulky
Pokud chcete používat službu Azure Table rozhraní API, budete muset:

1. Reference souboru pomocí automatického zavaděče [require_once] [ require_once] příkaz, a
2. Referenční všechny třídy, které můžete použít.

Následující příklad ukazuje, jak se zahrnuje automatického zavaděče souboru a odkaz **ServicesBuilder** třídy.

> [!NOTE]
> V příkladech v tomto článku předpokládá, že jste nainstalovali PHP klientské knihovny pro Azure prostřednictvím autora. Pokud jste nainstalovali v knihovnách ručně, je třeba odkazovat <code>WindowsAzure.php</code> automatického zavaděče souboru.
>
>

```php
require_once 'vendor/autoload.php';
use WindowsAzure\Common\ServicesBuilder;
```

V následujících příkladech `require_once` údajů se vždy zobrazí, ale jenom ty třídy potřebné pro tento příklad provést odkazují.

## <a name="set-up-an-azure-storage-connection"></a>Nastavit připojení k úložišti Azure
K vytvoření instance klienta služby Azure Table, musíte nejprve mít platný připojovací řetězec. Formát pro připojovací řetězec služby tabulky je:

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
* Použití **CloudConfigurationManager (CCM)** zkontrolujte několik externích zdrojů pro připojovací řetězec:
  * ve výchozím nastavení je teď obsahuje podporu pro jeden externí zdroj – proměnné prostředí
  * můžete přidat nové zdroje tím, že rozšíří **ConnectionStringSource** – třída

Příklady podle zde uvedeného se předají připojovací řetězec, který přímo.

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;

$tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);
```

## <a name="create-a-table"></a>Vytvoření tabulky
A **TableRestProxy** objektu umožňuje vytvořit tabulku s **createTable** metoda. Při vytváření tabulky, můžete nastavit časový limit služby Table. (Další informace o tabulce časový limit služby najdete v tématu [nastavení časových limitů pro operace služby s tabulkou][table-service-timeouts].)

```php
require_once 'vendor\autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use MicrosoftAzure\Storage\Common\ServiceException;

// Create table REST proxy.
$tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

try    {
    // Create table.
    $tableRestProxy->createTable("mytable");
}
catch(ServiceException $e){
    $code = $e->getCode();
    $error_message = $e->getMessage();
    // Handle exception based on error codes and messages.
    // Error codes and messages can be found here:
    // http://msdn.microsoft.com/library/azure/dd179438.aspx
}
```

Informace o omezeních na názvy tabulek najdete v tématu [Principy datového modelu služby Table][table-data-model].

## <a name="add-an-entity-to-a-table"></a>Přidání entity do tabulky
Pokud chcete přidat entitu do tabulky, vytvořte novou **Entity** objektu a předejte ji do **TableRestProxy -> insertEntity**. Všimněte si, že při vytváření entity, musíte zadat `PartitionKey` a `RowKey`. Tyto jsou jedinečné identifikátory pro entitu a jsou hodnoty, které může být dotazován mnohem rychleji než ostatní vlastnosti entity. Používá systém `PartitionKey` automaticky distribuovat entity v tabulce přes mnoho uzly úložiště. Entity se stejným `PartitionKey` jsou uložené na stejném uzlu. (Provedení operace u více entit, které jsou uložené ve stejném uzlu lepší, než na entity ukládat v rámci různých uzlech.) `RowKey` Je jedinečný Identifikátor entity v rámci oddílu.

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use MicrosoftAzure\Storage\Common\ServiceException;
use MicrosoftAzure\Storage\Table\Models\Entity;
use MicrosoftAzure\Storage\Table\Models\EdmType;

// Create table REST proxy.
$tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

$entity = new Entity();
$entity->setPartitionKey("tasksSeattle");
$entity->setRowKey("1");
$entity->addProperty("Description", null, "Take out the trash.");
$entity->addProperty("DueDate",
                        EdmType::DATETIME,
                        new DateTime("2012-11-05T08:15:00-08:00"));
$entity->addProperty("Location", EdmType::STRING, "Home");

try{
    $tableRestProxy->insertEntity("mytable", $entity);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // http://msdn.microsoft.com/library/azure/dd179438.aspx
    $code = $e->getCode();
    $error_message = $e->getMessage();
}
```

Informace o vlastnosti tabulky a typy najdete v tématu [Principy datového modelu služby Table][table-data-model].

**TableRestProxy** třída nabízí dvě alternativní metody pro vložení entity: **insertOrMergeEntity** a **insertOrReplaceEntity**. Pokud chcete použít tyto metody, vytvořte novou **Entity** a předejte ji jako parametr metody. Pokud neexistuje, každá metoda vloží entitu. Pokud entita již existuje, **insertOrMergeEntity** aktualizace hodnot vlastností, pokud již existují vlastnosti a přidá nové vlastnosti Pokud neexistují, při **insertOrReplaceEntity** úplně nahradí stávající entity. Následující příklad ukazuje, jak používat **insertOrMergeEntity**. Pokud entita s `PartitionKey` "tasksSeattle" a `RowKey` "1" již neexistuje, bude možné vložit. Ale pokud dříve vložení (jak je uvedeno v předchozím příkladu), `DueDate` vlastnost bude aktualizovat a `Status` přidá vlastnost. `Description` a `Location` jsou aktualizovány vlastnosti, ale s hodnotami, které efektivně je ponechte beze změny. Pokud tyto dvě vlastnosti pozdější nebyly přidány, jak je znázorněno v příkladu, ale byly na cílovou entitu, jejich existující hodnoty by zůstanou beze změny.

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use MicrosoftAzure\Storage\Common\ServiceException;
use MicrosoftAzure\Storage\Table\Models\Entity;
use MicrosoftAzure\Storage\Table\Models\EdmType;

// Create table REST proxy.
$tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

//Create new entity.
$entity = new Entity();

// PartitionKey and RowKey are required.
$entity->setPartitionKey("tasksSeattle");
$entity->setRowKey("1");

// If entity exists, existing properties are updated with new values and
// new properties are added. Missing properties are unchanged.
$entity->addProperty("Description", null, "Take out the trash.");
$entity->addProperty("DueDate", EdmType::DATETIME, new DateTime()); // Modified the DueDate field.
$entity->addProperty("Location", EdmType::STRING, "Home");
$entity->addProperty("Status", EdmType::STRING, "Complete"); // Added Status field.

try    {
    // Calling insertOrReplaceEntity, instead of insertOrMergeEntity as shown,
    // would simply replace the entity with PartitionKey "tasksSeattle" and RowKey "1".
    $tableRestProxy->insertOrMergeEntity("mytable", $entity);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // http://msdn.microsoft.com/library/azure/dd179438.aspx
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

## <a name="retrieve-a-single-entity"></a>Načtení jedné entity
**TableRestProxy -> getEntity** metoda umožňuje načíst jednu entitu pomocí dotazů na jeho `PartitionKey` a `RowKey`. V příkladu níže klíč oddílu `tasksSeattle` a klíč řádku `1` jsou předávány **getEntity** metoda.

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use MicrosoftAzure\Storage\Common\ServiceException;

// Create table REST proxy.
$tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

try    {
    $result = $tableRestProxy->getEntity("mytable", "tasksSeattle", 1);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // http://msdn.microsoft.com/library/azure/dd179438.aspx
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}

$entity = $result->getEntity();

echo $entity->getPartitionKey().":".$entity->getRowKey();
```

## <a name="retrieve-all-entities-in-a-partition"></a>Načtení všech entit v oddílu
Dotazy na entity se vytvářejí pomocí filtrů (Další informace najdete v tématu [dotazování tabulky a entity][filters]). Pro načtení všech entit v oddílu, použijte filtr "PartitionKey eq *název_oddílu*". Následující příklad ukazuje, jak načíst všechny entity v `tasksSeattle` oddílu pomocí filtru k předání **queryEntities** metoda.

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use MicrosoftAzure\Storage\Common\ServiceException;

// Create table REST proxy.
$tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

$filter = "PartitionKey eq 'tasksSeattle'";

try    {
    $result = $tableRestProxy->queryEntities("mytable", $filter);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // http://msdn.microsoft.com/library/azure/dd179438.aspx
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}

$entities = $result->getEntities();

foreach($entities as $entity){
    echo $entity->getPartitionKey().":".$entity->getRowKey()."<br />";
}
```

## <a name="retrieve-a-subset-of-entities-in-a-partition"></a>Načtení podmnožinu entit v oddílu
Stejným způsobem používaným v předchozím příkladu lze použít k načtení všech podmnožinu entit v oddílu. Určuje podmnožinu entity načtete filtru můžete použít (Další informace najdete v tématu [dotazování tabulky a entity][filters]). Následující příklad ukazuje, jak použít filtr k načtení všech entit s konkrétním `Location` a `DueDate` nižší než zadané datum.

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use MicrosoftAzure\Storage\Common\ServiceException;

// Create table REST proxy.
$tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

$filter = "Location eq 'Office' and DueDate lt '2012-11-5'";

try    {
    $result = $tableRestProxy->queryEntities("mytable", $filter);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // http://msdn.microsoft.com/library/azure/dd179438.aspx
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}

$entities = $result->getEntities();

foreach($entities as $entity){
    echo $entity->getPartitionKey().":".$entity->getRowKey()."<br />";
}
```

## <a name="retrieve-a-subset-of-entity-properties"></a>Načtení podmnožinu vlastností entity
Dotaz můžete načíst podmnožinu vlastností entity. Tento postup volá *projekce*, omezuje šířku pásma a může zlepšit výkon dotazů, hlavně pro velké entity. Pokud chcete nastavit vlastnost, která má být načtena, předat název vlastnosti, která má **dotazu -> addSelectField** metoda. Tuto metodu můžete volat více než jednou. Chcete-li přidat další vlastnosti. Po provedení **TableRestProxy -> queryEntities**, vrácené entity budou mít pouze vybraných vlastností. (V Pokud budete chtít vrátit podmnožinu entity tabulky, použijte filtr jak je znázorněno v dotazech výše.)

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use MicrosoftAzure\Storage\Common\ServiceException;
use MicrosoftAzure\Storage\Table\Models\QueryEntitiesOptions;

// Create table REST proxy.
$tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

$options = new QueryEntitiesOptions();
$options->addSelectField("Description");

try    {
    $result = $tableRestProxy->queryEntities("mytable", $options);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // http://msdn.microsoft.com/library/azure/dd179438.aspx
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}

// All entities in the table are returned, regardless of whether
// they have the Description field.
// To limit the results returned, use a filter.
$entities = $result->getEntities();

foreach($entities as $entity){
    $description = $entity->getProperty("Description")->getValue();
    echo $description."<br />";
}
```

## <a name="update-an-entity"></a>Aktualizace entity
Stávající entitu můžete aktualizovat pomocí **Entity -> SetProperty –** a **Entity -> AddProperty –** metody pro entitu a pak volání **TableRestProxy -> updateEntity**. Následující příklad načte entitu, upraví jednu vlastnost, odebere jinou vlastnost a přidá novou vlastnost. Všimněte si, že vlastnost můžete odebrat tak nastavení její hodnoty na **null**.

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use MicrosoftAzure\Storage\Common\ServiceException;
use MicrosoftAzure\Storage\Table\Models\Entity;
use MicrosoftAzure\Storage\Table\Models\EdmType;

// Create table REST proxy.
$tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

$result = $tableRestProxy->getEntity("mytable", "tasksSeattle", 1);

$entity = $result->getEntity();

$entity->setPropertyValue("DueDate", new DateTime()); //Modified DueDate.

$entity->setPropertyValue("Location", null); //Removed Location.

$entity->addProperty("Status", EdmType::STRING, "In progress"); //Added Status.

try    {
    $tableRestProxy->updateEntity("mytable", $entity);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // http://msdn.microsoft.com/library/azure/dd179438.aspx
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

## <a name="delete-an-entity"></a>Odstranění entity
Pokud chcete odstranit entitu, předat název tabulky a entity `PartitionKey` a `RowKey` k **TableRestProxy -> deleteEntity** metoda.

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use MicrosoftAzure\Storage\Common\ServiceException;

// Create table REST proxy.
$tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

try    {
    // Delete entity.
    $tableRestProxy->deleteEntity("mytable", "tasksSeattle", "2");
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // http://msdn.microsoft.com/library/azure/dd179438.aspx
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

Všimněte si, že pro kontrolách souběžnosti, můžete nastavit značku Etag pro entitu k odstranění pomocí **DeleteEntityOptions -> setEtag** metoda a předávání **DeleteEntityOptions** do objektu  **deleteEntity** jako čtvrtý parametr.

## <a name="batch-table-operations"></a>Operace s tabulkou batch
**TableRestProxy -> batch** metoda umožňuje spuštění více operací v jedné žádosti. Vzor zde vyžaduje přidání operací do **BatchRequest** objekt a následné předání **BatchRequest** do objektu **TableRestProxy -> batch** metoda. Chcete-li přidat operace **BatchRequest** objektu, žádný z následujících metod můžete volat vícekrát:

* **addInsertEntity** (přidá insertEntity operace)
* **addUpdateEntity** (přidá updateEntity operace)
* **addMergeEntity** (přidá mergeEntity operace)
* **addInsertOrReplaceEntity** (přidá insertOrReplaceEntity operace)
* **addInsertOrMergeEntity** (přidá insertOrMergeEntity operace)
* **addDeleteEntity** (přidá deleteEntity operace)

Následující příklad ukazuje, jak provádět **insertEntity** a **deleteEntity** operace v jedné žádosti:

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use MicrosoftAzure\Storage\Common\ServiceException;
use MicrosoftAzure\Storage\Table\Models\Entity;
use MicrosoftAzure\Storage\Table\Models\EdmType;
use MicrosoftAzure\Storage\Table\Models\BatchOperations;

    // Create table REST proxy.
$tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

// Create list of batch operation.
$operations = new BatchOperations();

$entity1 = new Entity();
$entity1->setPartitionKey("tasksSeattle");
$entity1->setRowKey("2");
$entity1->addProperty("Description", null, "Clean roof gutters.");
$entity1->addProperty("DueDate",
                        EdmType::DATETIME,
                        new DateTime("2012-11-05T08:15:00-08:00"));
$entity1->addProperty("Location", EdmType::STRING, "Home");

// Add operation to list of batch operations.
$operations->addInsertEntity("mytable", $entity1);

// Add operation to list of batch operations.
$operations->addDeleteEntity("mytable", "tasksSeattle", "1");

try    {
    $tableRestProxy->batch($operations);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // http://msdn.microsoft.com/library/azure/dd179438.aspx
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

Další informace o dávkování operace s tabulkou najdete v tématu [provádění transakcí skupiny Entity][entity-group-transactions].

## <a name="delete-a-table"></a>Odstranění tabulky
Nakonec pokud chcete odstranit tabulku, předat název tabulky k **TableRestProxy -> deleteTable** metoda.

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use MicrosoftAzure\Storage\Common\ServiceException;

// Create table REST proxy.
$tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

try    {
    // Delete table.
    $tableRestProxy->deleteTable("mytable");
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // http://msdn.microsoft.com/library/azure/dd179438.aspx
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

## <a name="next-steps"></a>Další postup
Teď, když jste se naučili základy služby Azure Table, postupujte podle následujících odkazech na další informace o složitějších úlohách úložiště.

* [Microsoft Azure Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md) je bezplatná samostatná aplikace od Microsoftu, která umožňuje vizuálně pracovat s daty Azure Storage ve Windows, macOS a Linuxu.

* [Středisko pro vývojáře PHP](/develop/php/).

[download]: http://go.microsoft.com/fwlink/?LinkID=252473
[require_once]: http://php.net/require_once
[table-service-timeouts]: http://msdn.microsoft.com/library/azure/dd894042.aspx

[table-data-model]: http://msdn.microsoft.com/library/azure/dd179338.aspx
[filters]: http://msdn.microsoft.com/library/azure/dd894031.aspx
[entity-group-transactions]: http://msdn.microsoft.com/library/azure/dd894038.aspx
