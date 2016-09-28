<properties
   pageTitle="Správa Azure Data Lake Analytics pomocí sady Azure SDK pro Node.js | Azure"
   description="Naučte se spravovat zdroje dat, úlohy, uživatele a účty Data Lake Analytics pomocí sady Azure SDK pro Node.js"
   services="data-lake-analytics"
   documentationCenter=""
   authors="edmacauley"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-analytics"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="05/16/2016"
   ms.author="edmaca"/>


# Správa Azure Data Lake Analytics pomocí sady Azure SDK pro Node.js


[AZURE.INCLUDE [manage-selector](../../includes/data-lake-analytics-selector-manage.md)]

Sada Azure SDK pro Node.js se dá použít k správě úloh, katalogů a účtů Azure Data Lake Analytics. Pokud chcete zobrazit téma správy s použitím jiných nástrojů, klikněte na výběr karty výše.

Aktuálně podporuje:

  *  **Verze Node.js: 0.10.0 nebo vyšší**
  *  **Verze rozhraní REST API pro Účet: 2015-10-01-preview**
  *  **Verze rozhraní REST API pro Katalog: 2015-10-01-preview**
  *  **Verze rozhraní REST API pro Úlohu: 2016-03-20 preview**

## Funkce

- Správa účtů: vytvoření, získání, seznam, aktualizace a odstranění.
- Správa úloh: odeslání, získání, seznam, zrušení.
- Správa katalogů: získání, seznam, vytvoření (tajné údaje), aktualizace (tajné údaje), odstranění (tajné údaje).

## Postup instalace

```bash
npm install azure-arm-datalake-analytics
```

## Ověření pomocí služby Azure Active Directory

 ```javascript
 var msrestAzure = require('ms-rest-azure');
 //user authentication
 var credentials = new msRestAzure.UserTokenCredentials('your-client-id', 'your-domain', 'your-username', 'your-password', 'your-redirect-uri');
 //service principal authentication
 var credentials = new msRestAzure.ApplicationTokenCredentials('your-client-id', 'your-domain', 'your-secret');
 ```

## Vytvoření klienta Data Lake Analytics

```javascript
var adlaManagement = require("azure-arm-datalake-analytics");
var acccountClient = new adlaManagement.DataLakeAnalyticsAccountClient(credentials, 'your-subscription-id');
var jobClient = new adlaManagement.DataLakeAnalyticsJobClient(credentials, 'azuredatalakeanalytics.net');
var catalogClient = new adlaManagement.DataLakeAnalyticsCatalogClient(credentials, 'azuredatalakeanalytics.net');
```

## Vytvoření účtu Data Lake Analytics

```javascript
var util = require('util');
var resourceGroupName = 'testrg';
var accountName = 'testadlaacct';
var location = 'eastus2';

// A Data Lake Store account must already have been created to create
// a Data Lake Analytics account. See the Data Lake Store readme for
// information on doing so. For now, we assume one exists already.
var datalakeStoreAccountName = 'existingadlsaccount';

// account object to create
var accountToCreate = {
  tags: {
    testtag1: 'testvalue1',
    testtag2: 'testvalue2'
  },
  name: accountName,
  location: location,
  properties: {
    defaultDataLakeStoreAccount: datalakeStoreAccountName,
    dataLakeStoreAccounts: [
      {
        name: datalakeStoreAccountName
      }
    ]
  }
};

client.account.create(resourceGroupName, accountName, accountToCreate, function (err, result, request, response) {
  if (err) {
    console.log(err);
    /*err has reference to the actual request and response, so you can see what was sent and received on the wire.
      The structure of err looks like this:
      err: {
        code: 'Error Code',
        message: 'Error Message',
        body: 'The response body if any',
        request: reference to a stripped version of http request
        response: reference to a stripped version of the response
      }
    */
  } else {
    console.log('result is: ' + util.inspect(result, {depth: null}));
  }
});
```

## Získání seznamu úloh

```javascript
var util = require('util');
var accountName = 'testadlaacct';
jobClient.job.list(accountName, function (err, result, request, response) {
  if (err) {
    console.log(err);
  } else {
    console.log('result is: ' + util.inspect(result, {depth: null}));
  }
});
```

## Získání seznamu databází v katalogu Data Lake Analytics
```javascript
var util = require('util');
var accountName = 'testadlaacct';
catalogClient.catalog.listDatabases(accountName, function (err, result, request, response) {
  if (err) {
    console.log(err);
  } else {
    console.log('result is: ' + util.inspect(result, {depth: null}));
  }
});
```

## Viz také

- [Microsoft Azure SDK pro Node.js](https://github.com/azure/azure-sdk-for-node)
- [Microsoft Azure SDK pro Node.js – správa Data Lake Store](https://github.com/Azure/azure-sdk-for-node/tree/autorest/lib/services/dataLake.Store)



<!--HONumber=Sep16_HO3-->


