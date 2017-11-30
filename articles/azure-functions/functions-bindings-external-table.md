---
title: "Externí tabulky funkcí vazby Azure (Preview) | Microsoft Docs"
description: "Používání vazeb externí tabulky v Azure Functions"
services: functions
documentationcenter: 
author: alexkarcher-msft
manager: cfowler
editor: 
ms.assetid: 
ms.service: functions
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 04/12/2017
ms.author: alkarche
ms.openlocfilehash: 7b226aa4ec71535aa0222389aacd74764a80021a
ms.sourcegitcommit: cfd1ea99922329b3d5fab26b71ca2882df33f6c2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/30/2017
---
# <a name="azure-functions-external-table-binding-preview"></a>Externí tabulky funkcí vazby Azure (Preview)
Tento článek ukazuje, jak k manipulaci s tabulková data v poskytovatelů SaaS (například služby Sharepoint, Dynamics) v rámci funkce s integrovanou vazby. Azure Functions podporuje vstupní a výstupní vazby pro externí tabulky.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

## <a name="api-connections"></a>Připojení rozhraní API

Tabulka vazby využívat externí rozhraní API připojení k ověření pomocí poskytovatelů SaaS 3. stran. 

Při přiřazování vazbu můžete buď vytvořit nové připojení rozhraní API, nebo použít stávající připojení k rozhraní API v rámci stejné skupiny prostředků

### <a name="supported-api-connections-tables"></a>Připojení podporované rozhraní API (tabulky) s

|konektor|Trigger|Vstup|Výstup|
|:-----|:---:|:---:|:---:|
|[DB2](https://docs.microsoft.com/azure/connectors/connectors-create-api-db2)||x|x
|[Dynamics 365 pro operace](https://ax.help.dynamics.com/wiki/install-and-configure-dynamics-365-for-operations-warehousing/)||x|x
|[Dynamics 365](https://docs.microsoft.com/azure/connectors/connectors-create-api-crmonline)||x|x
|[Dynamics NAV](https://msdn.microsoft.com/library/gg481835.aspx)||x|x
|[Tabulky Google](https://docs.microsoft.com/azure/connectors/connectors-create-api-googledrive)||x|x
|[Informix](https://docs.microsoft.com/azure/connectors/connectors-create-api-informix)||x|x
|[Dynamics 365 pro Finance](https://docs.microsoft.com/azure/connectors/connectors-create-api-crmonline)||x|x
|[MySQL](https://docs.microsoft.com/azure/store-php-create-mysql-database)||x|x
|[Oracle Database](https://docs.microsoft.com/azure/connectors/connectors-create-api-oracledatabase)||x|x
|[Běžné datové služby](https://docs.microsoft.com/common-data-service/entity-reference/introduction)||x|x
|[Salesforce](https://docs.microsoft.com/azure/connectors/connectors-create-api-salesforce)||x|x
|[SharePoint](https://docs.microsoft.com/azure/connectors/connectors-create-api-sharepointonline)||x|x
|[SQL Server](https://docs.microsoft.com/azure/connectors/connectors-create-api-sqlazure)||x|x
|[Teradata](http://www.teradata.com/products-and-services/azure/products/)||x|x
|UserVoice||x|x
|Zendesk||x|x


> [!NOTE]
> Externí připojení tabulky lze použít také v [Azure Logic Apps](https://docs.microsoft.com/azure/connectors/apis-list)

### <a name="creating-an-api-connection-step-by-step"></a>Vytvoření připojení k rozhraní API: krok za krokem

1. Vytvoření funkce > vlastní funkce ![vytvoření vlastní funkce](./media/functions-bindings-storage-table/create-custom-function.jpg)
1. Scénář `Experimental`  >  `ExternalTable-CSharp` šablony > vytvořit novou `External Table connection` 
 ![vstupní šablonu vybrat tabulky](./media/functions-bindings-storage-table/create-template-table.jpg)
1. Vyberte poskytovatele SaaS > vyberte nebo vytvořte připojení ![připojení SaaS konfigurace](./media/functions-bindings-storage-table/authorize-API-connection.jpg)
1. Vyberte připojení k rozhraní API > vytvořit funkci ![vytvořit tabulku – funkce](./media/functions-bindings-storage-table/table-template-options.jpg)
1. Vyberte`Integrate` > `External Table`
    1. Nakonfigurujte připojení používat, cílová tabulka. Tato nastavení budou velmi mezi poskytovatelů SaaS. Jsou outline níže v [nastavení zdroje dat](#datasourcesettings)
![konfigurace tabulky](./media/functions-bindings-storage-table/configure-API-connection.jpg)

## <a name="usage"></a>Využití

Tento příklad se připojí k tabulku s názvem "Kontakt" s Id, FirstName a LastName sloupce. Kód zobrazí entity kontaktů v tabulce a protokoly první a poslední názvy.

### <a name="bindings"></a>Vazby
```json
{
  "bindings": [
    {
      "type": "manualTrigger",
      "direction": "in",
      "name": "input"
    },
    {
      "type": "apiHubTable",
      "direction": "in",
      "name": "table",
      "connection": "ConnectionAppSettingsKey",
      "dataSetName": "default",
      "tableName": "Contact",
      "entityId": "",
    }
  ],
  "disabled": false
}
```
`entityId`musí být u vazeb tabulky prázdný.

`ConnectionAppSettingsKey`Určuje nastavení aplikace, která ukládá připojovací řetězec rozhraní API. Nastavení aplikace je vytvořena automaticky při přidání připojení k rozhraní API v integrací uživatelského rozhraní.

Tabulkové konektor poskytuje datových sad a každé datové sady obsahuje tabulky. Název výchozí sadu dat je "default". Názvy pro datovou sadu a tabulky v různých zprostředkovatelů SaaS jsou uvedeny níže:

|konektor|Datová sada|Table|
|:-----|:---|:---| 
|**SharePoint**|Lokality|Seznam serveru SharePoint
|**SQL**|Databáze|Table 
|**List Google**|Tabulky|Listu 
|**Excel**|Soubor aplikace Excel|List 

<!--
See the language-specific sample that copies the input file to the output file.

* [C#](#incsharp)
* [Node.js](#innodejs)

-->
<a name="incsharp"></a>

### <a name="usage-in-c"></a>Použití v jazyce C# #

```cs
#r "Microsoft.Azure.ApiHub.Sdk"
#r "Newtonsoft.Json"

using System;
using Microsoft.Azure.ApiHub;

//Variable name must match column type
//Variable type is dynamically bound to the incoming data
public class Contact
{
    public string Id { get; set; }
    public string LastName { get; set; }
    public string FirstName { get; set; }
}

public static async Task Run(string input, ITable<Contact> table, TraceWriter log)
{
    //Iterate over every value in the source table
    ContinuationToken continuationToken = null;
    do
    {   
        //retreive table values
        var contactsSegment = await table.ListEntitiesAsync(
            continuationToken: continuationToken);

        foreach (var contact in contactsSegment.Items)
        {   
            log.Info(string.Format("{0} {1}", contact.FirstName, contact.LastName));
        }

        continuationToken = contactsSegment.ContinuationToken;
    }
    while (continuationToken != null);
}
```

<!--
<a name="innodejs"></a>

### Usage in Node.js

```javascript
module.exports = function(context) {
    context.log('Node.js Queue trigger function processed', context.bindings.myQueueItem);
    context.bindings.myOutputFile = context.bindings.myInputFile;
    context.done();
};
```
-->
<a name="datasourcesettings"></a>
##Nastavení zdroje dat

### <a name="sql-server"></a>SQL Server

Skript, který chcete vytvořit a naplnit tabulky Kontakt je níže. dataSetName je "default".

```sql
CREATE TABLE Contact
(
    Id int NOT NULL,
    LastName varchar(20) NOT NULL,
    FirstName varchar(20) NOT NULL,
    CONSTRAINT PK_Contact_Id PRIMARY KEY (Id)
)
GO
INSERT INTO Contact(Id, LastName, FirstName)
     VALUES (1, 'Bitt', 'Prad') 
GO
INSERT INTO Contact(Id, LastName, FirstName)
     VALUES (2, 'Glooney', 'Ceorge') 
GO
```

### <a name="google-sheets"></a>Tabulky Google
V Google dokumentace, vytvořit tabulku s názvem list `Contact`. Zobrazovaný název tabulky nelze použít konektor. Interní název (tučným písmem) potřebám má být použit jako dataSetName, například: `docs.google.com/spreadsheets/d/`  **`1UIz545JF_cx6Chm_5HpSPVOenU4DZh4bDxbFgJOSMz0`**  přidat názvy sloupců `Id`, `LastName`, `FirstName` na první řádek, pak naplnění dat na Další řádky.

### <a name="salesforce"></a>Salesforce
dataSetName je "default".

## <a name="next-steps"></a>Další kroky

> [!div class="nextstepaction"]
> [Další informace o Azure functions triggerů a vazeb](functions-triggers-bindings.md)
