---
title: "Používání úložiště Azure Table z Node.js | Microsoft Docs"
description: "Ukládejte si strukturovaná data v cloudu pomocí Azure Table Storage, úložiště dat typu NoSQL."
services: cosmos-db
documentationcenter: nodejs
author: mimig1
manager: jhubbard
editor: tysonn
ms.assetid: fc2e33d2-c5da-4861-8503-53fdc25750de
ms.service: cosmos-db
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: article
ms.date: 11/03/2017
ms.author: mimig
ms.openlocfilehash: 7a92abbb08270c33f1848dc6789a89c2a81701ff
ms.sourcegitcommit: 295ec94e3332d3e0a8704c1b848913672f7467c8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/06/2017
---
# <a name="how-to-use-azure-table-storage-from-nodejs"></a>Používání úložiště Azure Table z Node.js
[!INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]
[!INCLUDE [storage-table-cosmos-db-langsoon-tip-include](../../includes/storage-table-cosmos-db-langsoon-tip-include.md)]

## <a name="overview"></a>Přehled
Toto téma ukazuje, jak provádět běžné scénáře s využitím služby Azure Table v aplikaci Node.js.

Příklady kódu v tomto tématu se předpokládá, že již máte aplikace Node.js. Informace o tom, jak vytvořit aplikaci Node.js v Azure najdete v tématu některé z těchto témat:

* [Vytvoření webové aplikace Node.js ve službě Azure App Service](../app-service/app-service-web-get-started-nodejs.md)
* [Sestavení a nasazení aplikace Node.js ve službě Azure Cloud Service](../cloud-services/cloud-services-nodejs-develop-deploy-app.md) (pomocí prostředí Windows PowerShell)

[!INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="configure-your-application-to-access-azure-storage"></a>Konfigurace aplikace pro přístup k úložišti Azure
Používání Azure Storage, musíte sadu Azure SDK úložiště pro Node.js, která obsahuje sadu knihoven pohodlí, které komunikují s služby REST úložiště.

### <a name="use-node-package-manager-npm-to-install-the-package"></a>Uzel balíčku správce (NPM) použijte k instalaci balíčku
1. Pomocí rozhraní příkazového řádku, jako například **prostředí PowerShell** (Windows), **Terminálové** (Mac), nebo **Bash** (Unix) a přejděte do složky, které jste vytvořili vaší aplikace.
2. Typ **npm nainstalujte azure-storage** v příkazovém okně. Výstup z tohoto příkazu se podobně jako v následujícím příkladu.

       azure-storage@0.5.0 node_modules\azure-storage
       +-- extend@1.2.1
       +-- xmlbuilder@0.4.3
       +-- mime@1.2.11
       +-- node-uuid@1.4.3
       +-- validator@3.22.2
       +-- underscore@1.4.4
       +-- readable-stream@1.0.33 (string_decoder@0.10.31, isarray@0.0.1, inherits@2.0.1, core-util-is@1.0.1)
       +-- xml2js@0.2.7 (sax@0.5.2)
       +-- request@2.57.0 (caseless@0.10.0, aws-sign2@0.5.0, forever-agent@0.6.1, stringstream@0.0.4, oauth-sign@0.8.0, tunnel-agent@0.4.1, isstream@0.1.2, json-stringify-safe@5.0.1, bl@0.9.4, combined-stream@1.0.5, qs@3.1.0, mime-types@2.0.14, form-data@0.2.0, http-signature@0.11.0, tough-cookie@2.0.0, hawk@2.3.1, har-validator@1.8.0)
3. Můžete ručně spustit **ls** příkazu ověřte, že **uzlu\_moduly** složka byla vytvořena. Uvnitř této složky najdete **azure-storage** balíček, který obsahuje knihovny, je třeba získat přístup k úložišti.

### <a name="import-the-package"></a>Import balíčku
Přidejte následující kód do horní části **server.js** souborů ve vaší aplikaci:

```nodejs
var azure = require('azure-storage');
```

## <a name="set-up-an-azure-storage-connection"></a>Nastavit připojení k Azure Storage
Modul azure, bude číst proměnné prostředí AZURE\_úložiště\_účet a AZURE\_úložiště\_přístup\_klíč nebo AZURE\_úložiště\_připojení\_řetězec informace požadované pro připojení k účtu úložiště Azure. Pokud nejsou nastavené těchto proměnných prostředí, musíte zadat informace o účtu při volání metody **TableService**.

## <a name="create-a-table"></a>Vytvoření tabulky
Následující kód vytvoří **TableService** objektu a pomocí něj vytvořit novou tabulku. Přidejte následující v horní části **server.js**.

```nodejs
var tableSvc = azure.createTableService();
```

Volání **createTableIfNotExists** vytvoří novou tabulku se zadaným názvem, pokud ještě neexistuje. Následující příklad vytvoří novou tabulku s názvem "mytable", pokud ještě neexistuje:

```nodejs
tableSvc.createTableIfNotExists('mytable', function(error, result, response){
  if(!error){
    // Table exists or created
  }
});
```

`result.created` Bude `true` Pokud se vytvoří nové tabulky, a `false` Pokud tabulka již existuje. `response` Bude obsahovat informace o požadavku.

### <a name="filters"></a>Filtry
Volitelné filtrování operace lze použít pro operace provedené pomocí **TableService**. Filtrování operací může zahrnovat protokolování, automatickým opakovaným pokusem o atd. Filtry jsou objekty, které implementovat metodu s podpisem:

```nodejs
function handle (requestOptions, next)
```

Až to předzpracování na žádost o možnostech, metoda potřebuje volat tlačítko Další"předání zpětné volání podpisem následující:

```nodejs
function (returnObject, finalCallback, next)
```

V této zpětného volání a po zpracování returnObject (odpovědi požadavek na server) zpětné volání je potřeba buď vyvolat další, pokud existuje pokračovat ve zpracovávání ostatní filtry nebo jednoduše vyvolat finalCallback jinak k ukončení volání služby.

Dva filtry, které implementují logiku opakovaných pokusů, které jsou součástí sady Azure SDK pro Node.js, **ExponentialRetryPolicyFilter** a **LinearRetryPolicyFilter**. Vytvoří následující **TableService** objekt, který používá **ExponentialRetryPolicyFilter**:

```nodejs
var retryOperations = new azure.ExponentialRetryPolicyFilter();
var tableSvc = azure.createTableService().withFilter(retryOperations);
```

## <a name="add-an-entity-to-a-table"></a>Přidání entity do tabulky
Chcete-li přidat entitu, nejprve vytvořte objekt, který definuje vlastnosti vaší entity. Musí obsahovat všechny entity **PartitionKey** a **RowKey**, které jsou jedinečné identifikátory pro entitu.

* **PartitionKey** -určuje uložených entity v oddílu
* **RowKey** – jednoznačně identifikuje entity v oddílu

Obě **PartitionKey** a **RowKey** musí být řetězcové hodnoty. Další informace najdete v tématu [Principy datového modelu služby Table](http://msdn.microsoft.com/library/azure/dd179338.aspx).

Následuje příklad definování entity. Všimněte si, že **dueDate** je definována jako typ **Edm.DateTime**. Určení typu je volitelný a typy bude možné odvodit, pokud není zadán.

```nodejs
var task = {
  PartitionKey: {'_':'hometasks'},
  RowKey: {'_': '1'},
  description: {'_':'take out the trash'},
  dueDate: {'_':new Date(2015, 6, 20), '$':'Edm.DateTime'}
};
```

> [!NOTE]
> K dispozici je také **časové razítko** pole pro každý záznam, který je nastavena jako Azure po vložení nebo aktualizaci entity.
>
>

Můžete také **entityGenerator** vytváření entit. Následující příklad vytvoří stejné entity úloh pomocí **entityGenerator**.

```nodejs
var entGen = azure.TableUtilities.entityGenerator;
var task = {
  PartitionKey: entGen.String('hometasks'),
  RowKey: entGen.String('1'),
  description: entGen.String('take out the trash'),
  dueDate: entGen.DateTime(new Date(Date.UTC(2015, 6, 20))),
};
```

Chcete-li do tabulky přidat entitu, předejte objekt entity, který má **insertEntity** metoda.

```nodejs
tableSvc.insertEntity('mytable',task, function (error, result, response) {
  if(!error){
    // Entity inserted
  }
});
```

Pokud byla operace úspěšná, `result` bude obsahovat [značka ETag](http://en.wikipedia.org/wiki/HTTP_ETag) vložené záznamu a `response` bude obsahovat informace o operaci.

Příklad odpovědi:

```nodejs
{ '.metadata': { etag: 'W/"datetime\'2015-02-25T01%3A22%3A22.5Z\'"' } }
```

> [!NOTE]
> Ve výchozím nastavení **insertEntity** nevrací vložené entity jako součást `response` informace. Pokud plánujete provádí jiné operace na tuto entitu, nebo chcete pro ukládání do mezipaměti informace, může být užitečné jej vraceny jako součást `result`. Můžete k tomu povolením **echoContent** následujícím způsobem:
>
> `tableSvc.insertEntity('mytable', task, {echoContent: true}, function (error, result, response) {...}`
>
>

## <a name="update-an-entity"></a>Aktualizace entity
Existuje několik metod, které jsou k dispozici pro aktualizace stávající entity:

* **replaceEntity** -aktualizace stávající entity podle jeho nahrazení
* **mergeEntity** -aktualizace stávající entity sloučením nové hodnoty vlastností do stávající entity
* **insertOrReplaceEntity** -aktualizace stávající entity podle jeho nahrazení. Pokud žádná entita existuje, bude možné vložit nový
* **insertOrMergeEntity** -aktualizace stávající entity sloučením nové hodnoty vlastností do stávající. Pokud žádná entita existuje, bude možné vložit nový

Následující příklad ukazuje, aktualizuje entitu s využitím **replaceEntity**:

```nodejs
tableSvc.replaceEntity('mytable', updatedTask, function(error, result, response){
  if(!error) {
    // Entity updated
  }
});
```

> [!NOTE]
> Ve výchozím nastavení aktualizaci entity nekontroluje zobrazíte, když data aktualizované dříve byla změněna jiným procesem. Podpora souběžných aktualizace:
>
> 1. Získá značku ETag objekt je aktualizován. K této chybě dochází v rámci `response` pro všechny operace související entity a mohou být načteny prostřednictvím `response['.metadata'].etag`.
> 2. Při provádění operace aktualizace na entitu, přidání značka ETag informace dříve načtené do nové entity. Například:
>
>       entity2 [.metadata] .etag = currentEtag;
> 3. Proveďte operaci aktualizace. Pokud byla entita od načíst hodnotu značka ETag, jako je například jiná instance aplikace, změněna `error` bude vrácen s informacemi o tom, zda je aktualizace podmínka uvedená v žádosti nebyla splněná.
>
>

S **replaceEntity** a **mergeEntity**, pokud typ entity, která se právě aktualizuje neexistuje, pak operace aktualizace se nezdaří. Proto pokud chcete uložit entity bez ohledu na tom, zda již existuje, použijte **insertOrReplaceEntity** nebo **insertOrMergeEntity**.

`result` Operace úspěšná aktualizace bude obsahovat **Značka Etag** aktualizované entity.

## <a name="work-with-groups-of-entities"></a>Práce se skupinami entit
Někdy má smysl odeslat více operací společně v dávce zajistit atomic zpracování serverem. Chcete-li provést tuto akci, použijte **TableBatch** třídy pro vytvoření dávky a pak použijte **executeBatch** metodu **TableService** provádět dávkové operace.

 Následující příklad ukazuje, odesílání dvě entity v dávce:

```nodejs
var task1 = {
  PartitionKey: {'_':'hometasks'},
  RowKey: {'_': '1'},
  description: {'_':'Take out the trash'},
  dueDate: {'_':new Date(2015, 6, 20)}
};
var task2 = {
  PartitionKey: {'_':'hometasks'},
  RowKey: {'_': '2'},
  description: {'_':'Wash the dishes'},
  dueDate: {'_':new Date(2015, 6, 20)}
};

var batch = new azure.TableBatch();

batch.insertEntity(task1, {echoContent: true});
batch.insertEntity(task2, {echoContent: true});

tableSvc.executeBatch('mytable', batch, function (error, result, response) {
  if(!error) {
    // Batch completed
  }
});
```

Pro úspěšné dávkových operací `result` bude obsahovat informace o každé operace v dávce.

### <a name="work-with-batched-operations"></a>Práce s dávkové operace
Přidat do dávky operace může být prověřovány zobrazením `operations` vlastnost. Můžete taky následující metody pro práci s operací:

* **Vymazat** -vymaže všechny operace z dávky
* **getOperations** -získá operace z dávky
* **hasOperations** -vrátí hodnotu true, pokud dávka obsahuje operace
* **removeOperations** – odebere operace
* **velikost** -vrátí počet operací v dávce

## <a name="retrieve-an-entity-by-key"></a>Načtení entity pomocí klíče
Vrátit konkrétní entitu na základě **PartitionKey** a **RowKey**, použijte **retrieveEntity** metoda.

```nodejs
tableSvc.retrieveEntity('mytable', 'hometasks', '1', function(error, result, response){
  if(!error){
    // result contains the entity
  }
});
```

Po dokončení této operace `result` bude obsahovat entity.

## <a name="query-a-set-of-entities"></a>Dotaz na sadu entit
Dotaz na tabulku, použijte **TableQuery** objekt vybudovat výrazu dotazu pomocí klauzule následující:

* **Vyberte** -polí, která mají být vráceny z dotazu
* **kde** -where – klauzule

  * **a** – `and` kde podmínky
  * **nebo** – `or` kde podmínky
* **horní** -počet položek k načtení

Následující příklad vytvoří dotaz, který vrátí nejvyšší pět položek s PartitionKey 'hometasks'.

```nodejs
var query = new azure.TableQuery()
  .top(5)
  .where('PartitionKey eq ?', 'hometasks');
```

Vzhledem k tomu **vyberte** se nepoužívá, bude vrácen všechna pole. Pokud chcete provést dotaz na tabulku, použijte **queryEntities**. Následující příklad používá tento dotaz se vrátí entity ze "mytable".

```nodejs
tableSvc.queryEntities('mytable',query, null, function(error, result, response) {
  if(!error) {
    // query was successful
  }
});
```

V případě úspěšného `result.entries` bude obsahovat pole entit, které odpovídají dotazu. Pokud dotaz se nepodařilo vrátit všechny entity `result.continuationToken` bude jinou hodnotu než*null* a mohou být použity jako třetí parametr funkce **queryEntities** k načtení více výsledků. Pro počáteční dotaz, použít *null* pro třetí parametr.

### <a name="query-a-subset-of-entity-properties"></a>Dotaz na podmnožinu vlastností entity
Dotaz na tabulku může načíst několika pole z entity.
To zmenšuje šířku pásma a může zlepšit výkon dotazů, hlavně pro velké entity. Použití **vyberte** klauzule a předejte názvy polí, která má být vrácen. Například následující dotaz vrátí jenom **popis** a **dueDate** pole.

```nodejs
var query = new azure.TableQuery()
  .select(['description', 'dueDate'])
  .top(5)
  .where('PartitionKey eq ?', 'hometasks');
```

## <a name="delete-an-entity"></a>Odstranění entity
Můžete odstranit pomocí jeho klíče oddílu a řádku entity. V tomto příkladu **task1** objekt obsahuje **RowKey** a **PartitionKey** hodnoty entity, která má být odstraněna. Pak je objekt předaný **deleteEntity** metoda.

```nodejs
var task = {
  PartitionKey: {'_':'hometasks'},
  RowKey: {'_': '1'}
};

tableSvc.deleteEntity('mytable', task, function(error, response){
  if(!error) {
    // Entity deleted
  }
});
```

> [!NOTE]
> Zvažte použití značky etag binárním rozsáhlým při odstraňování položek, zajistit, že položka nebyl změněn jiným procesem. V tématu [aktualizovat entitu,](#update-an-entity) informace o použití značky etag binárním rozsáhlým.
>
>

## <a name="delete-a-table"></a>Odstranění tabulky
Následující kód odstraní tabulku z účtu úložiště.

```nodejs
tableSvc.deleteTable('mytable', function(error, response){
    if(!error){
        // Table deleted
    }
});
```

Pokud si nejste jisti, zda tabulka existuje, použijte **deleteTableIfExists**.

## <a name="use-continuation-tokens"></a>Použít pokračování tokeny
Když dotazujete tabulky pro velké objemy výsledky, vyhledejte pokračování tokeny. K dispozici pro svůj dotaz, který je nemusíte být vědomi toho, pokud není sestavení poznáte při token pokračování je k dispozici může být velké objemy dat.

Výsledky objektu vrátil během dotazování sady entit `continuationToken` vlastnost, pokud takový token je k dispozici. Pak můžete toto při provádění dotazu nadále přesouvat mezi oddílů a tabulka entity.

Při dotazování, může být zadán parametr continuationToken mezi instance objektu dotazu a funkce zpětného volání:

```nodejs
var nextContinuationToken = null;
dc.table.queryEntities(tableName,
    query,
    nextContinuationToken,
    function (error, results) {
        if (error) throw error;

        // iterate through results.entries with results

        if (results.continuationToken) {
            nextContinuationToken = results.continuationToken;
        }

    });
```

Je-li si prohlédnout `continuationToken` objekt, zjistíte, vlastnosti, jako `nextPartitionKey`, `nextRowKey` a `targetLocation`, které lze použít k iteraci v rámci všechny výsledky.

Je také ukázku pokračování v úložišti Azure Storage Node.js na Githubu. Vyhledejte `examples/samples/continuationsample.js`.

## <a name="work-with-shared-access-signatures"></a>Práce s podpisy sdíleného přístupu
Sdílené přístupové podpisy (SAS) jsou zabezpečené způsob, jak poskytnout podrobné přístup k tabulkám bez zadání názvu účtu úložiště nebo klíče. SAS se často používá k poskytování omezený přístup k datům, například povolení mobilní aplikace vyhledejte záznamy.

Důvěryhodné aplikace, jako je Cloudová služba vygeneruje SAS pomocí **generateSharedAccessSignature** z **TableService**a poskytuje ji jako nedůvěryhodný nebo částečně důvěryhodné aplikace mobilní aplikace. SAS je generována pomocí zásad, která popisuje počátečním a koncovým datem, během které SAS je platný, a také úroveň přístupu k majiteli SAS.

Následující příklad vytvoří nové zásady sdíleného přístupu, který vám umožní držitele SAS pro dotaz ('r') v tabulce a vyprší platnost 100 minut od okamžiku, kdy je vytvořena.

```nodejs
var startDate = new Date();
var expiryDate = new Date(startDate);
expiryDate.setMinutes(startDate.getMinutes() + 100);
startDate.setMinutes(startDate.getMinutes() - 100);

var sharedAccessPolicy = {
  AccessPolicy: {
    Permissions: azure.TableUtilities.SharedAccessPermissions.QUERY,
    Start: startDate,
    Expiry: expiryDate
  },
};

var tableSAS = tableSvc.generateSharedAccessSignature('mytable', sharedAccessPolicy);
var host = tableSvc.host;
```

Všimněte si, že informace o hostiteli je třeba zadat také, jako je povinný, když se držitele SAS pokusí o přístup k tabulce.

Klientská aplikace pak používá SAS s **TableServiceWithSAS** k provádění operací s tabulkou. Následující příklad se připojí k tabulce a provádí dotazu.

```nodejs
var sharedTableService = azure.createTableServiceWithSas(host, tableSAS);
var query = azure.TableQuery()
  .where('PartitionKey eq ?', 'hometasks');

sharedTableService.queryEntities(query, null, function(error, result, response) {
  if(!error) {
    // result contains the entities
  }
});
```

Vzhledem k tomu, že SAS se vygeneroval s dotazu přístup jenom, pokud byly proveden pokus o vložení, aktualizaci nebo odstranění entity, bude vrácena chyba.

### <a name="access-control-lists"></a>Seznamy řízení přístupu
Seznam řízení přístupu (ACL) můžete také nastavit zásady přístupu pro SAS. To je užitečné, pokud chcete povolit více klientům přístup k tabulce, ale poskytnutí zásad, jiný přístup pro každého klienta.

Seznam ACL je implementovaná pomocí pole zásady přístupu s ID spojené s každou zásadu. V následujícím příkladu definuje dvě zásady, jeden pro "uživatel1" a jeden pro, uživatel2":

```nodejs
var sharedAccessPolicy = {
  user1: {
    Permissions: azure.TableUtilities.SharedAccessPermissions.QUERY,
    Start: startDate,
    Expiry: expiryDate
  },
  user2: {
    Permissions: azure.TableUtilities.SharedAccessPermissions.ADD,
    Start: startDate,
    Expiry: expiryDate
  }
};
```

Následující příklad načte aktuální seznam ACL pro **hometasks** tabulky a potom se přidají nové zásady pomocí **setTableAcl**. Tento přístup umožňuje:

```nodejs
var extend = require('extend');
tableSvc.getTableAcl('hometasks', function(error, result, response) {
if(!error){
    var newSignedIdentifiers = extend(true, result.signedIdentifiers, sharedAccessPolicy);
    tableSvc.setTableAcl('hometasks', newSignedIdentifiers, function(error, result, response){
      if(!error){
        // ACL set
      }
    });
  }
});
```

Jakmile je nastavená seznamu ACL, potom můžete vytvořit SAS na základě ID pro zásadu. Následující příklad vytvoří nový SAS pro, uživatel2":

```nodejs
tableSAS = tableSvc.generateSharedAccessSignature('hometasks', { Id: 'user2' });
```

## <a name="next-steps"></a>Další kroky
Další informace najdete v následujících zdrojích informací.

* [Microsoft Azure Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md) je bezplatná samostatná aplikace od Microsoftu, která umožňuje vizuálně pracovat s daty Azure Storage ve Windows, macOS a Linuxu.
* [Azure SDK úložiště pro uzel](https://github.com/Azure/azure-storage-node) úložišti na Githubu.
* [Středisko pro vývojáře Node.js](/develop/nodejs/)
* [Vytvoření a nasazení aplikace Node.js ve službě Azure Web](../app-service/app-service-web-get-started-nodejs.md)