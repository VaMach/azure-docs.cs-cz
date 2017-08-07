---
title: "Vytvoření funkce aktivované zprávami ve frontě v Azure | Dokumentace Microsoftu"
description: "Pomocí služby Azure Functions vytvoříte funkci bez serveru, kterou vyvolávají zprávy odeslané do fronty služby Azure Storage."
services: azure-functions
documentationcenter: na
author: ggailey777
manager: erikre
editor: 
tags: 
ms.assetid: 0b609bc0-c264-4092-8e3e-0784dcc23b5d
ms.service: functions
ms.devlang: multiple
ms.topic: get-started-article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 05/02/2017
ms.author: glenga
ms.custom: mvc
ms.translationtype: HT
ms.sourcegitcommit: c30998a77071242d985737e55a7dc2c0bf70b947
ms.openlocfilehash: 3eae02f7cf756e8e24d4f1952d12c37f2ad4b400
ms.contentlocale: cs-cz
ms.lasthandoff: 08/02/2017

---
# <a name="add-messages-to-an-azure-storage-queue-using-functions"></a>Přidání zpráv do fronty Azure Storage pomocí funkcí

Ve službě Azure Functions poskytují vstupní a výstupní vazby deklarativní způsob připojení k datům externí služby z funkce. V tomto tématu zjistíte, jak aktualizovat existující funkci přidáním výstupní vazby, která odesílá zprávy do Azure Queue Storage.  

![Zobrazte si zprávy v protokolech.](./media/functions-integrate-storage-queue-output-binding/functions-integrate-storage-binding-in-portal.png)

## <a name="prerequisites"></a>Požadavky 

[!INCLUDE [Previous topics](../../includes/functions-quickstart-previous-topics.md)]

* Nainstalujte [Microsoft Azure Storage Explorer](http://storageexplorer.com/).

## <a name="add-binding"></a>Přidání výstupní vazby
 
1. Rozbalte aplikaci Function App i funkci.

2. Vyberte možnost **Integrace** a **+ Nový výstup**, pak vyberte **Azure Queue Storage** a **Vybrat**.
    
    ![Přidejte výstupní vazbu Queue Storage do funkce na webu Azure Portal.](./media/functions-integrate-storage-queue-output-binding/function-add-queue-storage-output-binding.png)

3. Použijte nastavení uvedená v tabulce: 

    ![Přidejte výstupní vazbu Queue Storage do funkce na webu Azure Portal.](./media/functions-integrate-storage-queue-output-binding/function-add-queue-storage-output-binding-2.png)

    | Nastavení      |  Navrhovaná hodnota   | Popis                              |
    | ------------ |  ------- | -------------------------------------------------- |
    | **Název fronty**   | myqueue-items    | Název fronty, ke které se připojíte ve svém účtu úložiště. |
    | **Připojení k účtu úložiště** | AzureWebJobStorage | Můžete použít připojení k účtu úložiště, které už používá vaše aplikace Function App, nebo můžete vytvořit nové.  |
    | **Název parametru zprávy** | outQueueItem | Název výstupního parametru vazby. | 

4. Kliknutím na **Uložit** přidejte vazbu.
 
Teď máte definovanou výstupní vazbu a je potřeba aktualizovat kód tak, aby tuto vazbu využíval k přidávání zpráv do fronty.  

## <a name="update-the-function-code"></a>Aktualizace kódu funkce

1. Vybráním určité funkce zobrazíte kód této funkce v editoru. 

2. V případě funkce v jazyce C# následujícím způsobem aktualizujte definici funkce, aby obsahovala parametr vazby úložiště **outQueueItem**. V případě funkce v jazyce JavaScript tento krok přeskočte.

    ```cs   
    public static async Task<HttpResponseMessage> Run(HttpRequestMessage req, 
        ICollector<string> outQueueItem, TraceWriter log)
    {
        ....
    }
    ```

3. Těsně před vrácením hodnoty touto metodou přidejte do funkce následující kód. Použijte fragment kódu odpovídající jazyku vaší funkce.

    ```javascript
    context.bindings.outQueueItem = "Name passed to the function: " + 
                (req.query.name || req.body.name);
    ```

    ```cs
    outQueueItem.Add("Name passed to the function: " + name);     
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

1. Ve své funkci vyberte možnost **Integrace** a novou výstupní vazbu **Azure Queue Storage** a potom rozbalte položku **Dokumentace**. Zkopírujte nastavení **Název účtu** i **Klíč účtu**. Tyto přihlašovací údaje použijte k připojení k účtu úložiště.
 
    ![Získejte přihlašovací údaje účtu úložiště.](./media/functions-integrate-storage-queue-output-binding/function-get-storage-account-credentials.png)

2. Spusťte [Microsoft Azure Storage Explorer](http://storageexplorer.com/), vlevo vyberte ikonu připojení, zvolte **Použít název a klíč účtu úložiště** a vyberte **Další**.

    ![Spusťte nástroj Průzkumník účtu úložiště.](./media/functions-integrate-storage-queue-output-binding/functions-storage-manager-connect-1.png)
    
3. Do příslušných polí vložte **Název účtu** a **Klíč účtu** z kroku 1 a pak vyberte **Další** a **Připojit**. 
  
    ![Vložte přihlašovací údaje úložiště a připojte se.](./media/functions-integrate-storage-queue-output-binding/functions-storage-manager-connect-2.png)

4. Rozbalte připojený účet úložiště, klikněte pravým tlačítkem na **Fronty** a zkontrolujte, jestli existuje fronta s názvem **myqueue-items**. Fronta už by taky měla obsahovat zprávu.  
 
    ![Vytvořte frontu úložiště.](./media/functions-integrate-storage-queue-output-binding/function-queue-storage-output-view-queue.png)
 

## <a name="clean-up-resources"></a>Vyčištění prostředků

[!INCLUDE [Next steps note](../../includes/functions-quickstart-cleanup.md)]

## <a name="next-steps"></a>Další kroky

Přidali jste k existující funkci výstupní vazbu. 

[!INCLUDE [Next steps note](../../includes/functions-quickstart-next-steps.md)]

Další informace o vazbách do úložiště front najdete v tématu [Vazby front úložiště služby Azure Functions](functions-bindings-storage-queue.md). 




