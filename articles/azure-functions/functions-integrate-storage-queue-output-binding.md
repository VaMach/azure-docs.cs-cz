---
title: "Přidat zprávy do fronty Azure Storage pomocí funkcí | Microsoft Docs"
description: "Pomocí služby Azure Functions vytvoříte funkci bez serveru, kterou vyvolávají zprávy odeslané do fronty služby Azure Storage."
services: azure-functions
documentationcenter: na
author: ggailey777
manager: cfowler
editor: 
tags: 
ms.assetid: 0b609bc0-c264-4092-8e3e-0784dcc23b5d
ms.service: functions
ms.devlang: multiple
ms.topic: quickstart
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 09/19/2017
ms.author: glenga
ms.custom: mvc
ms.openlocfilehash: 822879861ee8189cdd413f0061f26fb91819d88d
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="add-messages-to-an-azure-storage-queue-using-functions"></a>Přidání zpráv do fronty Azure Storage pomocí funkcí

Ve službě Azure Functions poskytují vstupní a výstupní vazby deklarativní způsob připojení k datům externí služby z funkce. V tomto tématu zjistíte, jak aktualizovat existující funkci přidáním výstupní vazby, která odesílá zprávy do Azure Queue Storage.  

![Zobrazte si zprávy v protokolech.](./media/functions-integrate-storage-queue-output-binding/functions-integrate-storage-binding-in-portal.png)

## <a name="prerequisites"></a>Požadavky 

[!INCLUDE [Previous topics](../../includes/functions-quickstart-previous-topics.md)]

* Nainstalujte [Microsoft Azure Storage Explorer](http://storageexplorer.com/).

## <a name="add-binding"></a>Přidání výstupní vazby
 
1. Rozbalte aplikaci Function App i funkci.

2. Vyberte **integrací** a **+ nový výstupní**, zvolte **Azure Queue storage** a zvolte **vyberte**.
    
    ![Přidejte výstupní vazbu Queue Storage do funkce na webu Azure Portal.](./media/functions-integrate-storage-queue-output-binding/function-add-queue-storage-output-binding.png)

3. Použijte nastavení uvedená v tabulce: 

    ![Přidejte výstupní vazbu Queue Storage do funkce na webu Azure Portal.](./media/functions-integrate-storage-queue-output-binding/function-add-queue-storage-output-binding-2.png)

    | Nastavení      |  Navrhovaná hodnota   | Popis                              |
    | ------------ |  ------- | -------------------------------------------------- |
    | **Název fronty**   | myqueue-items    | Název fronty, ke které se připojíte ve svém účtu úložiště. |
    | **Připojení k účtu úložiště** | AzureWebJobStorage | Můžete použít připojení k účtu úložiště, které už používá vaše aplikace Function App, nebo můžete vytvořit nové.  |
    | **Název parametru zprávy** | outputQueueItem | Název výstupního parametru vazby. | 

4. Kliknutím na **Uložit** přidejte vazbu.
 
Teď máte definovanou výstupní vazbu a je potřeba aktualizovat kód tak, aby tuto vazbu využíval k přidávání zpráv do fronty.  

## <a name="update-the-function-code"></a>Aktualizace kódu funkce

1. Vybráním určité funkce zobrazíte kód této funkce v editoru. 

2. Pro C# funkci, aktualizovat svou definici funkce následujícím způsobem, aby přidat **outputQueueItem** úložiště parametr vazby. V případě funkce v jazyce JavaScript tento krok přeskočte.

    ```cs   
    public static async Task<HttpResponseMessage> Run(HttpRequestMessage req, 
        ICollector<string> outputQueueItem, TraceWriter log)
    {
        ....
    }
    ```

3. Těsně před vrácením hodnoty touto metodou přidejte do funkce následující kód. Použijte fragment kódu odpovídající jazyku vaší funkce.

    ```javascript
    context.bindings.outputQueueItem = "Name passed to the function: " + 
                (req.query.name || req.body.name);
    ```

    ```cs
    outputQueueItem.Add("Name passed to the function: " + name);     
    ```

4. Změny uložíte tak, že vyberete **Uložit**.

Hodnota předaná aktivační události HTTP je součástí zprávy přidané do fronty.
 
## <a name="test-the-function"></a>Testování funkce 

1. Po uložení změn kódu vyberte **Spustit**. 

    ![Přidejte výstupní vazbu Queue Storage do funkce na webu Azure Portal.](./media/functions-integrate-storage-queue-output-binding/functions-test-run-function.png)

2. Zkontrolujte protokoly a zkontrolujte, jestli se spuštění funkce zdařilo. Při prvním použití výstupní vazby vytvoří modul runtime Functions v účtu úložiště novou frontu s názvem **outqueue**.

Teď se můžete připojit ke svému účtu úložiště a zkontrolovat novou frontu i zprávy, které jste do ní přidali. 

## <a name="connect-to-the-queue"></a>Připojení k frontě

Pokud jste už nainstalovali Storage Explorer a připojili ho ke svému účtu úložiště, přeskočte první tři kroky.    

1. Ve funkci, zvolte **integrací** a nové **Azure Queue storage** výstup vazby, pak rozbalte **dokumentaci**. Zkopírujte nastavení **Název účtu** i **Klíč účtu**. Tyto přihlašovací údaje použijte k připojení k účtu úložiště.
 
    ![Získejte přihlašovací údaje účtu úložiště.](./media/functions-integrate-storage-queue-output-binding/function-get-storage-account-credentials.png)

2. Spusťte [Microsoft Azure Storage Explorer](http://storageexplorer.com/), vlevo vyberte ikonu připojení, zvolte **Použít název a klíč účtu úložiště** a vyberte **Další**.

    ![Spusťte nástroj Průzkumník účtu úložiště.](./media/functions-integrate-storage-queue-output-binding/functions-storage-manager-connect-1.png)
    
3. Do příslušných polí vložte **Název účtu** a **Klíč účtu** z kroku 1 a pak vyberte **Další** a **Připojit**. 
  
    ![Vložte přihlašovací údaje úložiště a připojte se.](./media/functions-integrate-storage-queue-output-binding/functions-storage-manager-connect-2.png)

4. Rozbalte účet připojené úložiště, rozbalte položku **fronty** a ověřte, že frontu s názvem **Moje_fronta položky** existuje. Fronta už by taky měla obsahovat zprávu.  
 
    ![Vytvořte frontu úložiště.](./media/functions-integrate-storage-queue-output-binding/function-queue-storage-output-view-queue.png)
 

## <a name="clean-up-resources"></a>Vyčištění prostředků

[!INCLUDE [Next steps note](../../includes/functions-quickstart-cleanup.md)]

## <a name="next-steps"></a>Další kroky

Přidali jste k existující funkci výstupní vazbu. 

[!INCLUDE [Next steps note](../../includes/functions-quickstart-next-steps.md)]

Další informace o vazbách do úložiště front najdete v tématu [Vazby front úložiště služby Azure Functions](functions-bindings-storage-queue.md). 



