---
title: "Postup dotazování dat v tabulce v Azure Cosmos DB? | Dokumentace Microsoftu"
description: "Naučte se tabulka dotaz na data v Azure Cosmos DB"
services: cosmos-db
documentationcenter: 
author: kanshiG
manager: jhubbard
editor: 
tags: 
ms.assetid: 14bcb94e-583c-46f7-9ea8-db010eb2ab43
ms.service: cosmos-db
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: 
ms.date: 11/15/2017
ms.author: govindk
ms.openlocfilehash: cbb752692fbd618d9e7e14c8a80b582aad657b38
ms.sourcegitcommit: 9a61faf3463003375a53279e3adce241b5700879
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/15/2017
---
# <a name="azure-cosmos-db-how-to-query-table-data-by-using-the-table-api"></a>Azure Cosmos DB: Jak dotazovat data tabulky pomocí rozhraní API tabulky

Azure Cosmos DB [tabulky API](table-introduction.md) podporuje OData a [LINQ](https://docs.microsoft.com/rest/api/storageservices/fileservices/writing-linq-queries-against-the-table-service) dotazy na data klíč hodnota (tabulky).  

Tento článek obsahuje následující úlohy: 

> [!div class="checklist"]
> * Dotazování na data s rozhraním API pro tabulky

Dotazy v tomto článku použijte následující příklad `People` tabulky:

| Klíč oddílu | RowKey | E-mail | Telefonní číslo |
| --- | --- | --- | --- |
| Tuleňů grónských | Walter | Walter@contoso.com| 425-555-0101 |
| Smith | Ben | Ben@contoso.com| 425-555-0102 |
| Smith | Jeff | Jeff@contoso.com| 425-555-0104 | 

V tématu [dotazování tabulky a entity] (https://docs.microsoft.com/rest/api/storageservices/fileservices/querying-tables-and-entities) podrobnosti o tom, jak dotazu pomocí rozhraní API tabulky. 

Další informace o možnosti premium, které nabízí Azure Cosmos DB najdete v tématu [rozhraní API služby Azure Cosmos DB tabulky](table-introduction.md) a [vývoj s rozhraním API pro tabulky v rozhraní .NET](tutorial-develop-table-dotnet.md). 

## <a name="prerequisites"></a>Požadavky

Pro tyto dotazy pro práci musí mít účet Azure Cosmos DB a mít data entity v kontejneru. Nemáte žádné těchto? Dokončení [rychlý start pětiminutovou](create-table-dotnet.md) nebo [vývojáře kurzu](tutorial-develop-table-dotnet.md) k vytvoření účtu a naplnit databázi.

## <a name="query-on-partitionkey-and-rowkey"></a>Dotaz na klíč oddílu a RowKey
Protože vlastnosti PartitionKey a RowKey formuláři primární klíč entity, můžete k identifikaci entity speciální syntaxe: 

**Dotaz**

```
https://<mytableendpoint>/People(PartitionKey='Harp',RowKey='Walter')  
```
**Výsledky**

| Klíč oddílu | RowKey | E-mail | Telefonní číslo |
| --- | --- | --- | --- |
| Tuleňů grónských | Walter | Walter@contoso.com| 425-555-0104 |

Alternativně můžete tyto vlastnosti v rámci `$filter` možnost, jak je znázorněno v následující části. Všimněte si, že názvů vlastností klíče a hodnoty konstant jsou malá a velká písmena. Vlastnosti PartitionKey i RowKey jsou typu řetězec. 

## <a name="query-by-using-an-odata-filter"></a>Dotazovat pomocí filtru OData
Když jste vytváření řetězec filtru, berte v úvahu tato pravidla: 

* Logické operátory definované specifikací protokolu OData slouží k porovnání vlastnosti a hodnotu. Všimněte si, že nelze porovnat vlastnost, která má dynamické hodnoty. Jedna strana výrazu musí být konstanta. 
* Název vlastnosti, operátor a hodnotu konstanty musí být odděleny prostory kódovaná adresou URL. Mezeru je kódovaná jako adresa URL jako `%20`. 
* Všechny části řetězec filtru rozlišují velká a malá písmena. 
* Hodnota konstanty musí být stejného typu dat jako vlastnost v pořadí pro filtr vracet výsledky platný. Další informace o typech podporovaných vlastnost najdete v tématu [Principy datového modelu služby Table](https://docs.microsoft.com/rest/api/storageservices/understanding-the-table-service-data-model). 

Tady je příklad dotazu, který ukazuje, jak filtrovat podle vlastnosti PartitionKey a e-mailu pomocí OData `$filter`.

**Dotaz**

```
https://<mytableapi-endpoint>/People()?$filter=PartitionKey%20eq%20'Smith'%20and%20Email%20eq%20'Ben@contoso.com'
```

Další informace o tom, jak vytvořit filtr výrazů pro různé typy dat najdete v tématu [dotazování tabulky a entity](https://docs.microsoft.com/rest/api/storageservices/querying-tables-and-entities).

**Výsledky**

| Klíč oddílu | RowKey | E-mail | Telefonní číslo |
| --- | --- | --- | --- |
| Ben |Smith | Ben@contoso.com| 425-555-0102 |

## <a name="query-by-using-linq"></a>Dotazu pomocí LINQ 
Také můžete dotazovat pomocí LINQ, což znamená, že je odpovídající výrazy dotazu OData. Tady je příklad toho, jak vytvořit dotazy pomocí .NET SDK:

```csharp
CloudTableClient tableClient = account.CreateCloudTableClient();
CloudTable table = tableClient.GetTableReference("people");

TableQuery<CustomerEntity> query = new TableQuery<CustomerEntity>()
    .Where(
        TableQuery.CombineFilters(
            TableQuery.GenerateFilterCondition(PartitionKey, QueryComparisons.Equal, "Smith"),
            TableOperators.And,
            TableQuery.GenerateFilterCondition(Email, QueryComparisons.Equal,"Ben@contoso.com")
    ));

await table.ExecuteQuerySegmentedAsync<CustomerEntity>(query, null);
```

## <a name="next-steps"></a>Další kroky

V tomto kurzu jste provést následující:

> [!div class="checklist"]
> * Dozvěděli, jak dotazovat pomocí rozhraní API tabulky

Nyní můžete přejít k dalším kurzu se dozvíte, jak se bude distribuovat globální data.

> [!div class="nextstepaction"]
> [Globálně distribuci dat](tutorial-global-distribution-table.md)
