---
title: "Vytvoření funkce připojující se ke službám Azure | Dokumentace Microsoftu"
description: "Pomocí Azure Functions vytvořte aplikaci bez serveru, která se připojuje k jiným službám Azure."
services: functions
documentationcenter: dev-center-name
author: yochay
manager: manager-alias
editor: 
tags: 
keywords: "funkce azure, funkce, zpracování událostí, webhook, dynamické výpočty, architektura bez serverů"
ms.assetid: ab86065d-6050-46c9-a336-1bfc1fa4b5a1
ms.service: functions
ms.devlang: multiple
ms.topic: get-started-article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 03/01/2017
ms.author: rachelap; glenga
translationtype: Human Translation
ms.sourcegitcommit: d9dad6cff80c1f6ac206e7fa3184ce037900fc6b
ms.openlocfilehash: 4bea7f73871f8bfc755f30b9ef41a1960893152e
ms.lasthandoff: 04/18/2017


---
# <a name="use-azure-functions-to-create-a-function-that-connects-to-other-azure-services"></a>Pomocí Azure Functions vytvořte funkci, která se připojuje k jiným službám Azure.

Toto téma ukazuje, jak vytvořit funkci v Azure Functions, která naslouchá zprávám ve frontě služby Azure Storage a kopíruje zprávy do řádků v tabulce Azure Storage. Funkce aktivovaná časovačem slouží k načítání zpráv do fronty. Druhá funkce čte z fronty a zapisuje zprávy do tabulky. Frontu i tabulku za vás vytvoří služba Azure Functions na základě definic vazeb. 

Aby to bylo zajímavější, jedna funkce je napsaná v JavaScriptu, zatímco druhá je skript v jazyce C#. To ukazuje, jak aplikace Function App může obsahovat funkce v různých jazycích. 

Předvedení tohoto scénáře zobrazuje [video na webu Channel 9](https://channel9.msdn.com/Series/Windows-Azure-Web-Sites-Tutorials/Create-an-Azure-Function-which-binds-to-an-Azure-service/player).

## <a name="create-a-function-that-writes-to-the-queue"></a>Vytvoření funkce, která zapisuje do fronty

Než se budete moci připojit k frontě úložiště, je nutné vytvořit funkci, která načítá frontu zpráv. Tato funkce JavaScriptu využívá trigger časovače, který každých 10 sekund zapisuje zprávu do fronty. Pokud ještě nemáte účet Azure, [vyzkoušejte si službu Azure Functions](https://functions.azure.com/try) nebo [si vytvořte bezplatný účet Azure](https://azure.microsoft.com/free/).

1. Přejděte na Azure Portal a vyhledejte aplikaci Function App.

2. Klikněte na **Nová funkce** > **TimerTrigger-JavaScript**. 

3. Pojmenujte funkci **FunctionsBindingsDemo1**, jako **Plán** zadejte hodnotu výrazu Cron `0/10 * * * * *` a potom klikněte na **Vytvořit**.
   
    ![Přidání funkce aktivované časovačem](./media/functions-create-an-azure-connected-function/new-trigger-timer-function.png)

    Nyní jste vytvořili funkci aktivovanou časovačem, která se spouští každých 10 sekund.

5. Na kartě **Vývoj** klikněte na **Protokoly** a zobrazte aktivitu v protokolu. Jak vidíte, každých 10 sekund se zapíše položka protokolu.
   
    ![Zobrazení protokolu pro kontrolu funkčnosti funkce](./media/functions-create-an-azure-connected-function/functionsbindingsdemo1-view-log.png)

## <a name="add-a-message-queue-output-binding"></a>Přidání výstupní vazby fronty zpráv

1. Na kartě **Integrace** zvolte **Nový výstup** > **Azure Queue Storage** > **Vybrat**.

    ![Přidání funkce časovače triggeru](./media/functions-create-an-azure-connected-function/functionsbindingsdemo1-integrate-tab.png)

2. Zadejte `myQueueItem` jako **Název parametru zprávy** a `functions-bindings` jako **Název fronty**, vyberte existující **Připojení účtu úložiště** nebo kliknutím na **nový** vytvořte připojení účtu úložiště a potom klikněte na **Uložit**.  

    ![Vytvoření výstupní vazby na frontu úložiště](./media/functions-create-an-azure-connected-function/functionsbindingsdemo1-integrate-tab2.png)

1. Na kartě **Vývoj** přidejte k funkci následující kód:
   
    ```javascript
   
    function myQueueItem() 
    {
        return {
            msg: "some message goes here",
            time: "time goes here"
        }
    }
   
    ```
2. Vyhledejte výraz *if* kolem řádku 9 funkce a vložte za tento výraz následující kód.
   
    ```javascript
   
    var toBeQed = myQueueItem();
    toBeQed.time = timeStamp;
    context.bindings.myQueueItem = toBeQed;
   
    ```  
   
    Tento kód vytvoří **myQueueItem** a nastaví vlastnost **time** (čas) na aktuální časové razítko. Pak novou položku fronty přidá do vazby **myQueueItem** podle kontextu.

3. Klikněte na **Uložit a spustit**.

## <a name="view-storage-updates-by-using-storage-explorer"></a>Zobrazení aktualizací úložiště pomocí Storage Exploreru
Fungování vaší funkce můžete ověřit zobrazením zpráv ve frontě, kterou jste vytvořili.  Můžete se připojit k vaší frontě úložiště pomocí Průzkumníku cloudu v sadě Visual Studio. Portál však umožňuje snadné připojení k účtu úložiště pomocí Microsoft Azure Storage Exploreru.

1. Na kartě **Integrace** klikněte na výstupní vazbu fronty > **Dokumentace**, zobrazte Připojovací řetězec pro účet úložiště a zkopírujte jeho hodnotu. Tuto hodnotu použijete pro připojení k účtu úložiště.

    ![Stáhnutí Azure Storage Exploreru](./media/functions-create-an-azure-connected-function/functionsbindingsdemo1-integrate-tab3.png)


2. Pokud jste tak již neučinili, stáhněte a nainstalujte [Microsoft Azure Storage Explorer](http://storageexplorer.com). 
 
3. Ve Storage Exploreru klikněte na ikonu připojit ke službě Azure Storage, do pole vložte připojovací řetězec a dokončete průvodce.

    ![Přidání připojení ve Storage Exploreru](./media/functions-create-an-azure-connected-function/functionsbindingsdemo1-storage-explorer.png)

4. V části **Místní a připojené** rozbalte **Účty úložiště** > váš účet úložiště > **Fronty** > **functions-bindings** a ověřte, že se zprávy zapisují do fronty.

    ![Zobrazení zpráv ve frontě](./media/functions-create-an-azure-connected-function/functionsbindings-azure-storage-explorer.png)

    Pokud fronta neexistuje nebo je prázdná, pravděpodobně je nějaký problém s vazbami nebo kódem vaší funkce.

## <a name="create-a-function-that-reads-from-the-queue"></a>Vytvoření funkce, která čte z fronty

Teď, když se zprávy přidávají do fronty, můžete vytvořit další funkci, která neustále čte z fronty a zapisuje zprávy do tabulky Azure Storage.

1. Klikněte na **Nová funkce** > **QueueTrigger-CSharp**. 
 
2. Pojmenujte funkci `FunctionsBindingsDemo2`, do pole **Název fronty** zadejte **functions-bindings**, vyberte existující účet úložiště nebo vytvořte nový, a potom klikněte na **Vytvořit**.

    ![Přidání funkce časovače výstupní fronty](./media/functions-create-an-azure-connected-function/function-demo2-new-function.png) 

3. (Volitelné) Fungování nové funkce můžete ověřit zobrazením nové fronty ve Storage Exploreru stejně jako předtím. Můžete také použít Průzkumník cloudu v sadě Visual Studio.  

4. (Volitelné) Obnovte frontu **functions-bindings** a všimněte si, že z fronty byly odebrány položky. K odebrání dochází, protože je funkce vázaná na frontu **functions-bindings** jako vstupní trigger a tato funkce čte z fronty. 
 
## <a name="add-a-table-output-binding"></a>Přidání výstupní vazby tabulky

1. V aplikaci FunctionsBindingsDemo2 klikněte na **Integrace** > **Nový výstup** > **Azure Table Storage** > **Vybrat**.

    ![Přidání vazby na tabulku Azure Storage](./media/functions-create-an-azure-connected-function/functionsbindingsdemo2-integrate-tab.png) 

2. Zadejte `functionbindings` jako **Název tabulky** a `myTable` jako **Název parametru tabulky**, zvolte **Připojení účtu úložiště** nebo vytvořte nové, a potom klikněte na **Uložit**.

    ![Konfigurace vazby tabulky Storage](./media/functions-create-an-azure-connected-function/functionsbindingsdemo2-integrate-tab2.png)
   
3. Na kartě **Vývoj** nahraďte stávající kód funkce následujícím:
   
    ```cs
    
    using System;
    
    public static void Run(QItem myQueueItem, ICollector<TableItem> myTable, TraceWriter log)
    {    
        TableItem myItem = new TableItem
        {
            PartitionKey = "key",
            RowKey = Guid.NewGuid().ToString(),
            Time = DateTime.Now.ToString("hh.mm.ss.ffffff"),
            Msg = myQueueItem.Msg,
            OriginalTime = myQueueItem.Time    
        };
        
        // Add the item to the table binding collection.
        myTable.Add(myItem);
    
        log.Verbose($"C# Queue trigger function processed: {myItem.RowKey} | {myItem.Msg} | {myItem.Time}");
    }
    
    public class TableItem
    {
        public string PartitionKey {get; set;}
        public string RowKey {get; set;}
        public string Time {get; set;}
        public string Msg {get; set;}
        public string OriginalTime {get; set;}
    }
    
    public class QItem
    {
        public string Msg { get; set;}
        public string Time { get; set;}
    }
    ```
    Třída **TableItem** reprezentuje řádek v tabulce úložiště a položky přidáváte do kolekce `myTable` objektů **TableItem**. Abyste mohli vkládat do tabulky, je třeba nastavit vlastnosti **PartitionKey** a **RowKey**.

4. Klikněte na **Uložit**.  Nakonec můžete ověřit fungování funkce zobrazením tabulky ve Storage Exploreru nebo v Průzkumníku cloudu sady Visual Studio.

5. (Volitelné) Ve svém účtu úložiště ve Storage Exploreru rozbalte **Tabulky** > **functionsbindings** a ověřte, že se do tabulky přidávají řádky. To samé můžete udělat v Průzkumníku cloudu v sadě Visual Studio.

    ![Zobrazení řádků v tabulce](./media/functions-create-an-azure-connected-function/functionsbindings-azure-storage-explorer2.png)

    Pokud tabulka neexistuje nebo je prázdná, pravděpodobně je nějaký problém s vazbami nebo kódem vaší funkce. 
 
[!INCLUDE [More binding information](../../includes/functions-bindings-next-steps.md)]

## <a name="next-steps"></a>Další kroky
Další informace o službě Azure Functions najdete v těchto tématech:

* [Referenční informace pro vývojáře Azure Functions](functions-reference.md)  
  Referenční informace pro programátory týkající se kódování funkcí a definování triggerů a vazeb.
* [Testování Azure Functions](functions-test-a-function.md)  
  Toto téma popisuje různé nástroje a techniky pro testování funkcí.
* [Postup škálování Azure Functions](functions-scale.md)  
  Toto téma popisuje plány služby, které jsou dostupné se službou Azure Functions (včetně plánu hostování Consumption), a výběr správného plánu. 

[!INCLUDE [Getting help note](../../includes/functions-get-help.md)]


