---
title: "Ukládání nestrukturovaných dat pomocí Azure Functions a databáze Cosmos DB"
description: "Ukládání nestrukturovaných dat pomocí Azure Functions a databáze Cosmos DB"
services: functions
documentationcenter: functions
author: rachelappel
manager: erikre
editor: 
tags: 
keywords: "azure functions, functions, zpracování událostí, Cosmos DB, dynamické výpočty, architektura bez serverů"
ms.assetid: 
ms.service: functions
ms.devlang: multiple
ms.topic: get-started-article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 05/08/2017
ms.author: rachelap
ms.custom: mvc
ms.translationtype: Human Translation
ms.sourcegitcommit: 6adaf7026d455210db4d7ce6e7111d13c2b75374
ms.openlocfilehash: 785bd144805a472ae457f9a3323d512b5cbf055d
ms.contentlocale: cs-cz
ms.lasthandoff: 06/22/2017

---
# Ukládání nestrukturovaných dat pomocí Azure Functions a databáze Cosmos DB
<a id="store-unstructured-data-using-azure-functions-and-cosmos-db" class="xliff"></a>

Databáze Azure Cosmos DB nabízí skvělou možnost pro ukládání nestrukturovaných dat a dat JSON. Spolu s Azure Functions urychluje a zjednodušuje ukládání dat – ve srovnání s ukládáním dat v relační databáze budete potřebovat méně kódování.

V tomto kurzu se dozvíte, jak pomocí portálu Azure Portal vytvořit funkci Azure, která bude ukládat nestrukturovaná data do dokumentu databáze Cosmos DB. 

## Požadavky
<a id="prerequisites" class="xliff"></a>

[!INCLUDE [Previous quickstart note](../../includes/functions-quickstart-previous-topics.md)]

[!INCLUDE [functions-portal-favorite-function-apps](../../includes/functions-portal-favorite-function-apps.md)]

## Vytvoření funkce
<a id="create-a-function" class="xliff"></a>

Vytvořte nový obecný webhook jazyka C#, který pojmenujete `MyTaskList`.

1. Rozbalte seznam existujících funkcí a kliknutím na symbol + vytvořte novou funkci.
1. Vyberte možnost GenericWebHook-CSharp a webhook pojmenujte `MyTaskList`.

![Přidejte novou aplikaci Function App pomocí obecného webhooku v jazyce C#.](./media/functions-integrate-store-unstructured-data-cosmosdb/functions-create-new-functionapp.png)

## Přidání výstupní vazby
<a id="add-an-output-binding" class="xliff"></a>

Funkce Azure může mít jednu aktivační událost a libovolný počet vstupních nebo výstupních vazeb. V tomto příkladu použijeme jako aktivační událost Požadavek HTTP a jako výstupní vazbu dokument databáze Cosmos DB.

1. Kliknutím na kartu *Integrace* této funkce můžete zobrazit nebo upravit její aktivační událost a vazby.
1. V pravém horním rohu stránky vyberte odkaz *Nový výstup*.

Poznámka: Aktivační událost Požadavek HTTP je už nakonfigurovaná, ale musíte přidat vazbu s dokumentem databáze Cosmos DB.

![Přidání nové výstupní vazby databáze Cosmos DB](./media/functions-integrate-store-unstructured-data-cosmosdb/functions-integrate-tab-add-new-output-binding.png)

1. Zadejte požadované informace pro vytvoření vazby. Hodnoty určete pomocí níže uvedené tabulky.

![Konfigurace výstupní vazby databáze Cosmos DB](./media/functions-integrate-store-unstructured-data-cosmosdb/functions-integrate-tab-configure-cosmosdb-binding.png)

|  Pole | Hodnota  |
|---|---|
| Název parametru dokumentu | Název, který odkazuje na objekt Cosmos DB v kódu |
| Název databáze | Název databáze pro uložení dokumentů |
| Název kolekce | Název seskupení databází Cosmos DB |
| Chcete vytvořit databázi Cosmos DB a kolekci? | Ano nebo Ne |
| Připojení účtu databáze Cosmos DB | Připojovací řetězec odkazující na databázi Cosmos DB |

Musíte také nakonfigurovat připojení k databázi Cosmos DB.

1. Vedle popisku „Připojení dokumentu databáze Cosmos DB“ klikněte na odkaz „Nové“.
1. Vyplňte pole a vyberte příslušné možnosti pro vytvoření dokumentu databáze Cosmos DB.

![Konfigurace připojení Cosmos DB](./media/functions-integrate-store-unstructured-data-cosmosdb/functions-create-CosmosDB.png)

|  Pole | Hodnota  |
|---|---|
| ID | Jedinečné ID databáze Cosmos DB  |
| NoSQL API | Cosmos DB nebo MongoDB  |
| Předplatné | Předplatné MSDN  |
| Skupina prostředků  | Vytvořte novou skupinu, nebo vyberte existující.  |
| Umístění  | WestEurope  |

1. Klikněte na tlačítko *OK*. Možná budete muset počkat několik minut, než Azure vytvoří potřebné prostředky.
1. Klikněte na tlačítko *Uložit*.

## Aktualizace kódu funkce
<a id="update-the-function-code" class="xliff"></a>

Kód šablony funkce nahraďte následujícím:

Upozorňujeme, že kód v této ukázce je pouze v jazyce C#.

```csharp
using System.Net;

public static HttpResponseMessage Run(HttpRequestMessage req, out object taskDocument, TraceWriter log)
{
    string name = req.GetQueryNameValuePairs()
        .FirstOrDefault(q => string.Compare(q.Key, "name", true) == 0)
        .Value;

    string task = req.GetQueryNameValuePairs()
        .FirstOrDefault(q => string.Compare(q.Key, "task", true) == 0)
        .Value;

    string duedate = req.GetQueryNameValuePairs()
        .FirstOrDefault(q => string.Compare(q.Key, "duedate", true) == 0)
        .Value;

    taskDocument = new {
        name = name,
        duedate = duedate.ToString(),
        task = task
    };

    if (name != "" && task != "") {
        return req.CreateResponse(HttpStatusCode.OK);
    }
    else {
        return req.CreateResponse(HttpStatusCode.BadRequest);
    }
}

```

Tento vzorový kód přečte řetězce dotazu na požadavek HTTP a přiřadí je jako členy objektu `taskDocument`. Objekt `taskDocument` automaticky uloží data do databáze Cosmos DB a databázi dokonce vytvoří při prvním použití.

## Testování funkce a databáze
<a id="test-the-function-and-database" class="xliff"></a>

1. Na kartě funkce klikněte vpravo na portále na dokaz *Test* a zadejte následující řetězce dotazu na HTTP:

| Řetězec dotazu | Hodnota |
|---|---|
| jméno | Jan Slanina |
| úkol | Vyrobit BLT sendvič |
| termín | 05/12/2017 |

1. Klikněte na odkaz *Spustit*.
1. Zkontrolujte, jestli funkce vrátila kód odpovědi *HTTP 200 OK*.

![Konfigurace výstupní vazby databáze Cosmos DB](./media/functions-integrate-store-unstructured-data-cosmosdb/functions-test-function.png)

Zkontrolujte, jestli se do databáze Cosmos DB uložil záznam.

1. Na portálu Azure Portal vyhledejte svou databázi a vyberte ji.
1. Vyberte možnost *Průzkumník dat*.
1. Rozbalte potřebné uzly, dokud se nedostanete k záznamům dokumentu.
1. Zkontrolujte položky databáze. V databázi budou kromě vašich dat také další metadata.

![Kontrola záznamu v databázi Cosmos DB](./media/functions-integrate-store-unstructured-data-cosmosdb/functions-verify-cosmosdb-output.png)

Pokud se data nachází v dokumentu, podařilo se vám vytvořit funkci Azure, která ukládá nestrukturovaná data do databáze Cosmos DB.

## Vyčištění prostředků
<a id="clean-up-resources" class="xliff"></a>

[!INCLUDE [Next steps note](../../includes/functions-quickstart-cleanup.md)]

## Další kroky
<a id="next-steps" class="xliff"></a>

Další informace o službě Azure Functions najdete v těchto tématech:

[!INCLUDE [Getting help note](../../includes/functions-get-help.md)]

[!INCLUDE [functions-quickstart-next-steps](../../includes/functions-quickstart-next-steps.md)]

