---
title: "Vytvoření funkce aktivovány Azure Cosmos DB | Microsoft Docs"
description: "Pomocí Azure Functions vytvořit funkci bez serveru, která je volána, když data budou přidána do databáze v Azure Cosmos DB."
services: azure-functions
documentationcenter: na
author: ggailey777
manager: cfowler
editor: 
tags: 
ms.assetid: bc497d71-75e7-47b1-babd-a060a664adca
ms.service: functions; cosmos-db
ms.devlang: multiple
ms.topic: quickstart
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 10/02/2017
ms.author: glenga
ms.custom: 
ms.openlocfilehash: 1ff4c2e024faba777fc479b3cd5864e097bbfce1
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="create-a-function-triggered-by-azure-cosmos-db"></a>Vytvoření funkce aktivovány Azure Cosmos DB

Zjistěte, jak vytvořit funkci aktivuje, když data budou přidána do nebo změnili v Azure Cosmos DB. Další informace o databázi Cosmos Azure najdete v tématu [Cosmos databázi Azure: bez serveru databáze computing pomocí Azure Functions](..\cosmos-db\serverless-computing-database.md).

![Zobrazte si zprávy v protokolech.](./media/functions-create-cosmos-db-triggered-function/quickstart-completed.png)

## <a name="prerequisites"></a>Požadavky

K provedení kroků v tomto kurzu je potřeba:

+ Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) před tím, než začnete.

[!INCLUDE [functions-portal-favorite-function-apps](../../includes/functions-portal-favorite-function-apps.md)]

## <a name="create-an-azure-function-app"></a>Vytvoření aplikace Azure Function App

[!INCLUDE [Create function app Azure portal](../../includes/functions-create-function-app-portal.md)]

Dál vytvoříte v nové aplikaci Function App funkci.

<a name="create-function"></a>

## <a name="create-azure-cosmos-db-trigger"></a>Vytvořit aktivační událost Azure Cosmos DB

1. Rozbalte aplikaci Function App a klikněte na tlačítko **+** vedle položky **Funkce**. Pokud jde o první funkci ve vaší aplikaci Function App, vyberte možnost **Vlastní funkce**. Zobrazí se kompletní sada šablon funkcí.

    ![Stručný úvod do služby Functions na webu Azure Portal](./media/functions-create-cosmos-db-triggered-function/add-first-function.png)

2. Vyhledejte a vyberte **Azure CosmosDBTrigger** šablonu pro požadovaný jazyk.

    ![Vytvoření funkce Azure DB Cosmos aktivaci](./media/functions-create-cosmos-db-triggered-function/select-cosmos-db-trigger-portal.png)

3. Nakonfigurujte novou aktivační událost pomocí nastavení uvedeného v následující tabulce bitovou kopii.

    ![Vytvoření funkce Azure DB Cosmos aktivaci](./media/functions-create-cosmos-db-triggered-function/functions-cosmosdb-trigger-settings.png)
    
    | Nastavení      | Navrhovaná hodnota  | Popis                                |
    | ------------ | ---------------- | ------------------------------------------ |
    | **Pojmenujte svoji funkci** | Výchozí | Použijte výchozí název funkce navrhované šablonou. |
    | **Název databáze** | Úlohy | Název databáze s kolekcí, které mají být monitorovány. |
    | **Název kolekce** | Items | Název kolekce, které mají být monitorovány. |
    | **Vytvoření kolekce zapůjčení, pokud neexistuje** | Zaškrtnuté | Kolekce ještě neexistuje, takže ji vytvořte. |

4. Vyberte **nový** vedle **připojení účtu Azure Cosmos DB** label a zvolte existující účet Cosmos DB nebo **+ vytvořit nový**. 
 
    ![Konfigurace připojení databáze Cosmos Azure](./media/functions-create-cosmos-db-triggered-function/functions-create-CosmosDB.png)

6. Při vytvoření nového účtu Cosmos DB, použijte **nový účet** nastavení uvedeného v tabulce.

    | Nastavení      | Navrhovaná hodnota  | Popis                                |
    | ------------ | ---------------- | ------------------------------------------ |
    | **ID** | Název databáze | Jedinečné ID pro databázi Azure Cosmos DB  |
    | **Rozhraní API** | SQL (DocumentDB) | Toto téma používá databázi dokumentů rozhraní API.  |
    | **Předplatné** | předplatné Azure | předplatné Azure  |
    | **Skupina prostředků** | myResourceGroup |  Použijte existující skupinu prostředků, která obsahuje vaši aplikací funkcí. |
    | **Umístění**  | WestEurope | Vyberte umístění blízko vaší aplikaci funkcí nebo jiným aplikacím, které používají uložené dokumenty.  |

6. Kliknutím na **OK** vytvořte databázi. Vytvoření databáze může trvat několik minut. Po vytvoření databáze se připojovací řetězec databáze uloží jako nastavení aplikace funkcí. Název nastavení této aplikace je vložen do **připojení účtu Azure Cosmos DB**. 

7. Klikněte na tlačítko **vytvořit** k vytvoření vašeho Azure DB Cosmos aktivuje funkce. Po vytvoření funkce se zobrazí kód na základě šablon funkce.  

    ![Šablony funkcí cosmos DB v jazyce C#](./media/functions-create-cosmos-db-triggered-function/function-cosmosdb-template.png)

    Tato šablona funkce zapíše počet dokumentů a první ID dokumentu do protokolů. 

V dalším kroku připojit k účtu Azure Cosmos DB a vytvořit **úlohy** databázi v kolekci. 

## <a name="create-the-items-collection"></a>Vytvoření kolekce položek

1. Otevřít druhou instanci [portál Azure](https://portal.azure.com) na nové záložce v prohlížeči. 

2. Na levé straně na portálu, rozbalte položku panelu ikonu typ `cosmos` v pole hledání a vyberte **Azure Cosmos DB**.

    ![Vyhledávání pro službu Azure Cosmos DB](./media/functions-create-cosmos-db-triggered-function/functions-search-cosmos-db.png)

2. Zvolte účet Azure Cosmos DB a potom vyberte **Průzkumníku dat**. 
 
3. V **kolekce**, zvolte **taskDatabase** a vyberte **nové kolekce**.

    ![Vytvoření kolekce](./media/functions-create-cosmos-db-triggered-function/cosmosdb-create-collection.png)

4. V **přidat kolekce**, použijte nastavení zobrazené v následující tabulce bitovou kopii. 
 
    ![Definování taskCollection](./media/functions-create-cosmos-db-triggered-function/cosmosdb-create-collection2.png)
 
    | Nastavení|Navrhovaná hodnota|Popis |
    | ---|---|--- |
    | **ID databáze** | Úlohy |Název nové databáze. Toto musí odpovídat názvu definované v vaší vazba funkce. |
    | **ID kolekce** | Items | Název pro novou kolekci. Toto musí odpovídat názvu definované v vaší vazba funkce.  |
    | **Kapacita úložiště** | Pevná (10 GB)|Použijte výchozí hodnotu. Tato hodnota je kapacita úložiště databáze. |
    | **Propustnost** |400 RU| Použijte výchozí hodnotu. Pokud budete chtít snížit latenci, můžete propustnost později navýšit. |
    | **[Klíč oddílu](../cosmos-db/partition-data.md#design-for-partitioning)** | /kategorie|Klíč oddílu, který rovnoměrně distribuuje data do jednotlivých oddílů. Výběr správného klíče oddílu je důležitý pro vytvoření výkonné kolekce. | 

1. Klikněte na tlačítko **OK** vytvořit **úlohy** kolekce. To může trvat po krátkou dobu kolekce vytvářeny.

Po kolekce Zadaná vazba funkce existuje, můžete otestovat funkci přidáním dokumenty do této nové kolekce.

## <a name="test-the-function"></a>Testování funkce

1. Rozbalte nové **taskCollection** Zvolte kolekci v Průzkumníku dat **dokumenty**, pak vyberte **nový dokument**.

    ![Vytvořte dokument v taskCollection](./media/functions-create-cosmos-db-triggered-function/create-document-in-collection.png)

2. Nahraďte obsah nový dokument s následujícím obsahem, a potom vyberte **Uložit**.

        {
            "id": "task1",
            "category": "general",
            "description": "some task"
        }

1. Přepnout na první kartu prohlížeče, který obsahuje funkce na portálu. Rozbalte funkce protokoly a ověřte, že nový dokument má aktivuje funkce. Zjistíte, že je `task1` hodnota ID dokumentu se zapisují do protokolů. 

    ![Zobrazte si zprávy v protokolech.](./media/functions-create-cosmos-db-triggered-function/functions-cosmosdb-trigger-view-logs.png)

4. (Volitelné) Přejděte zpět do dokumentu udělat změnu a klikněte na tlačítko **aktualizace**. Poté přejděte zpět do protokolů funkce a ověřte aktualizace má také aktivuje funkce.

## <a name="clean-up-resources"></a>Vyčištění prostředků

[!INCLUDE [Next steps note](../../includes/functions-quickstart-cleanup.md)]

## <a name="next-steps"></a>Další kroky

Vytvořili jste funkci, která se spustí v případě, že dokument je přidat ani upravit v vaší Azure DB Cosmos.

[!INCLUDE [Next steps note](../../includes/functions-quickstart-next-steps.md)]

Další informace o aktivačních událostech fronty úložiště najdete v tématu [Vazby front úložiště služby Azure Functions](functions-bindings-storage-queue.md).
