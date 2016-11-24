---
title: "Vytvoření funkce Azure s vazbou na službu Azure | Dokumentace Microsoftu"
description: "Sestavení funkce Azure, aplikace bez serveru, která komunikuje s dalšími službami Azure."
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
ms.date: 10/25/2016
ms.author: rachelap@microsoft.com
translationtype: Human Translation
ms.sourcegitcommit: a0a46708645be91f89b0a6ae9059468bc84aeb11
ms.openlocfilehash: c6905452951910d3c62bc5152741a8ead26196ef


---
# <a name="create-an-azure-function-which-binds-to-an-azure-service"></a>Vytvoření funkce Azure s vazbou na službu Azure
[!INCLUDE [Getting Started Note](../../includes/functions-getting-started.md)]

V tomto krátkém videu se dozvíte, jak vytvořit funkci Azure, která naslouchá zprávám ve frontě Azure a kopíruje zprávy do objektu Azure Blob.

## <a name="watch-the-video"></a>Přehrát video
>[!VIDEO https://channel9.msdn.com/Series/Windows-Azure-Web-Sites-Tutorials/Create-an-Azure-Function-which-binds-to-an-Azure-service/player]
>
>

## <a name="create-an-input-queue-trigger-function"></a>Vytvoření funkce pro aktivaci vstupní fronty
Cílem této funkce je každých 10 sekund napsat zprávu do fronty. K tomu musíte vytvořit funkci a fronty zpráv a do nově vytvořených front přidat kód pro zápis zpráv.

1. Přejděte na Azure Portal a najděte Azure Function App.
2. Klikněte na **Nová funkce** > **TimerTrigger – Node**. Pojmenujte funkci **FunctionsBindingsDemo1**
3. Zadejte hodnotu „0/10 * * * * *“ pro Plán. Tato hodnota má formu výrazu cron. Naplánuje, aby se časovač spouštěl každých 10 sekund.
4. Kliknutím na **Vytvořit** vytvořte novou funkci.
   
    ![Přidání funkcí časovače triggeru](./media/functions-create-an-azure-connected-function/new-trigger-timer-function.png)
5. Zobrazením aktivity v protokolu zkontrolujte, jestli funkce funguje. Možná budete muset kvůli zobrazení podokna protokolů kliknout na odkaz **Protokoly** v pravém horním rohu.
   
   ![Kontrola funkčnosti funkce zobrazením protokolu](./media/functions-create-an-azure-connected-function/functionsbindingsdemo1-view-log.png)

### <a name="add-a-message-queue"></a>Přidání fronty zpráv
1. Přejděte na kartu **Integrace**.
2. Zvolte **Nový výstup** > **Fronta Azure Storage** > **Vybrat**.
3. Do textového pole **Název parametru zprávy** zadejte **myQueueItem**.
4. Vyberte účet úložiště nebo kliknutím na **Nový** účet úložiště vytvořte, pokud ho ještě nemáte.
5. Do textového pole **Název fronty** zadejte **functions-bindings**.
6. Klikněte na **Uložit**.  
   
   ![Přidání funkcí časovače triggeru](./media/functions-create-an-azure-connected-function/functionsbindingsdemo1-integrate-tab.png)

### <a name="write-to-the-message-queue"></a>Zápis do fronty zpráv
1. Vraťte se na kartu **Vývoj** a přidejte k funkci následující kód za existující kód:
   
    ```javascript
   
    function myQueueItem() 
      {
        return {
        msg: "some message goes here",
        time: "time goes here"
      }
    }
   
    ```
2. Upravte existující kód funkce, aby volal kód přidaný v kroku 1. Vložte následující kód kolem řádku 9 funkce za výraz *if*.
   
    ```javascript
   
    var toBeQed = myQueueItem();
    toBeQed.time = timeStamp;
    context.bindings.myQueue = toBeQed;
   
    ```
   
    Tento kód vytvoří **myQueueItem** a nastaví vlastnost **time** (čas) na aktuální časové razítko. Pak novou položku fronty přidá do vazby myQueue podle kontextu.
3. Klikněte na **Uložit a spustit**.
4. Zkontrolujte, jestli kód funguje, zobrazením fronty v sadě Visual Studio.
   
   * Otevřete Visual Studio a přejděte na **Zobrazení** > **Průzkumník** **cloudu**.
   * Najděte účet úložiště a frontu **functions-bindings**, kterou jste použili při vytváření fronty myQueue. Měli byste vidět řádky dat protokolu. Možná se budete muset přihlásit k Azure pomocí sady Visual Studio.  

## <a name="create-an-output-queue-trigger-function"></a>Vytvoření funkce pro aktivaci výstupní fronty
1. Klikněte na **Nová funkce** > **QueueTrigger – C#**. Pojmenujte funkci **FunctionsBindingsDemo2** Všimněte si, že je možné ve stejné aplikaci funkce kombinovat jazyky (v tomto případě Node a C#).
2. Do pole **Název fronty** zadejte **functions-bindings**.
3. Vyberte účet úložiště, který chcete použít, nebo vytvořte nový.
4. Klikněte na **Vytvořit**
5. Zkontrolujte, jestli nová funkce funguje, zobrazením protokolu funkce i sady Visual Studio s aktualizovanými informacemi. Protokol funkce ukazuje, že funkce běží a položky jsou odstraněné z fronty. Vzhledem k tomu, že je funkce vázaná k výstupní funkci **functions-bindings** jako vstupní aktivační událost, mělo by se při aktualizaci fronty **functions-bindings** v sadě Visual Studio ukázat, že položky zmizely. Jsou z fronty odstraněné.   
   
   ![Přidání funkcí časovače výstupní fronty](./media/functions-create-an-azure-connected-function/functionsbindingsdemo2-integrate-tab.png)   

### <a name="modify-the-queue-item-type-from-json-to-object"></a>Změna typu položky fronty z JSON na objekt
1. Nahraďte kód ve **FunctionsBindingsDemo2** následujícím kódem:    
   
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
      log.Verbose($"C# Queue trigger function processed: {myQueueItem.Msg} | {myQueueItem.Time}");
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
   
    Tento kód přidá dvě třídy, **TableItem** a **QItem**, který slouží ke čtení z front a zápisu do front. Kromě toho byla změněna funkce **Run** (Spustit), aby přijímala parametr **QItem** a **TraceWriter** místo parametru **string** (řetězec) a **TraceWriter**. 
2. Klikněte na tlačítko **Uložit**.
3. Ověřte, jestli kód funguje, zkontrolováním protokolu. Všimněte si, že funkce Azure vám objekt automaticky serializují a deserializují. Je tak snadné přistupovat k frontě s orientací na objekt k předávání dat. 

## <a name="store-messages-in-an-azure-table"></a>Ukládání zpráv v tabulce Azure
Když teď máte spolupracující fronty, je čas přidat tabulku Azure pro trvalé uložení dat front.

1. Přejděte na kartu **Integrace**.
2. Vytvořte tabulku úložišť v Azure pro výstup a pojmenujte ji **myTable**.
3. Na otázku „To which table should the data be written?“ (Do které tabulky se mají data zapsat?) odpovězte **functionsbindings**.
4. Změňte nastavení **PartitionKey** z **{project-id}** na **{partition}**.
5. Vyberte účet úložiště nebo vytvořte nový.
6. Klikněte na **Uložit**.
7. Přejděte na kartu **Vývoj**.
8. Vytvořte třídu **TableItem**, která bude reprezentovat tabulku Azure, a upravte funkci Run tak, aby přijímala nově vytvořený objekt TableItem. Všimněte si, že je nutné použít vlastnosti **PartitionKey** a **RowKey**, aby to fungovalo.
   
    ```cs
   
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
   
      log.Verbose($"C# Queue trigger function processed: {myQueueItem.RowKey} | {myQueueItem.Msg} | {myQueueItem.Time}");
    }
   
    public class TableItem
    {
      public string PartitionKey {get; set;}
      public string RowKey {get; set;}
      public string Time {get; set;}
      public string Msg {get; set;}
      public string OriginalTime {get; set;}
    }
    ```
9. Klikněte na **Uložit**.
10. Zkontrolujte, jestli kód funguje, zobrazením protokolů funkce a v sadě Visual Studio. V sadě Visual Studio to zkontrolujete tak, že pomocí **Průzkumníku cloudu** přejdete na tabulku Azure **functionbindings** a zkontrolujete, jestli obsahuje řádky.

[!INCLUDE [Getting Started Note](../../includes/functions-bindings-next-steps.md)]

[!INCLUDE [Getting Started Note](../../includes/functions-get-help.md)]




<!--HONumber=Nov16_HO2-->


