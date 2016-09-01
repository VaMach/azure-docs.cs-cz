<properties
   pageTitle="Vytvoření funkce zpracování událostí | Microsoft Azure"
   description="Pomocí služby Azure Functions vytvoříte funkci v jazyce C#, která se spouští na základě časovače událostí."
   services="azure-functions"
   documentationCenter="na"
   authors="ggailey777"
   manager="erikre"
   editor=""
   tags=""
   />

<tags
   ms.service="functions"
   ms.devlang="multiple"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="multiple"
   ms.workload="na"
   ms.date="06/05/2016"
   ms.author="glenga"/>
   
# Vytvoření funkce Azure pro zpracování událostí

Azure Functions je výpočetní prostředí na vyžádání založené na událostech, které umožňuje vytvářet naplánované nebo aktivované jednotky kódu implementované v různých programovacích jazycích. Další informace o Azure Functions najdete v tématu [Přehled Azure Functions](functions-overview.md).

V tomto tématu se dozvíte, jak vytvořit novou funkci v jazyce C#, která se spouští na základě časovače událostí a přidává zprávy do fronty úložiště. 

## Požadavky 

Než vytvoříte funkci, potřebujete mít aktivní účet Azure. Pokud ještě nemáte účet Azure, [můžete použít bezplatné účty](https://azure.microsoft.com/free/).

## Vytvoření funkce spouštěné časovačem ze šablony

Provádění funkcí v Azure je hostováno v aplikaci funkce. Než vytvoříte funkci, potřebujete mít aktivní účet Azure. Pokud ještě nemáte účet Azure, [můžete použít bezplatné účty](https://azure.microsoft.com/free/). 

1. Přejděte na stránku [portálu Azure Functions](https://functions.azure.com/signin) a přihlaste se pomocí účtu Azure.

2. Pokud máte existující aplikaci Function App, kterou můžete použít, vyberte ji v části **Your function apps** (Vaše aplikace Function App) a klikněte na **Open** (Otevřít). Pokud chcete vytvořit novou aplikaci Function App, zadejte pro ni jedinečnou položku **Name** (Název) nebo přijměte vygenerovaný název, vyberte upřednostňovanou položku **Region** (Oblast) a klikněte na **Create + get started** (Vytvořit a začít). 

3. Ve své aplikaci Function App klikněte na **+ New Function** (+ Nová funkce) > **TimerTrigger - C#** (Časovač/trigger – C#) > **Create** (Vytvořit). Vytvoří se funkce s výchozím názvem, která se bude spouštět podle výchozího plánu, tedy jednou za minutu. 

    ![Vytvoření nové funkce aktivované pomocí časovače](./media/functions-create-an-event-processing-function/functions-create-new-timer-trigger.png)

4. V nové funkci klikněte na kartu **Integrate** (Integrace) > **New Output** (Nový výstup) > **Azure Storage Queue** (Fronta úložiště Azure) > **Select** (Vybrat).

    ![Vytvoření nové funkce aktivované pomocí časovače](./media/functions-create-an-event-processing-function/functions-create-storage-queue-output-binding.png)

5. V části **Azure Storage Queue output** (Výstup fronty úložiště Azure) klikněte na **Select** (Vybrat) vedle položky **Storage account connection** (Připojení účtu úložiště), vyberte existující účet úložiště nebo vytvořte nový a potom klikněte na **Save** (Uložit). 

    ![Vytvoření nové funkce aktivované pomocí časovače](./media/functions-create-an-event-processing-function/functions-create-storage-queue-output-binding-2.png)

6. Zpátky na kartě **Develop** (Vývoj) nahraďte existující skript v jazyce C# v okně **Code** (Kód) následujícím kódem:

        using System;
        
        public static void Run(TimerInfo myTimer, out string outputQueueItem, TraceWriter log)
        {
            // Add a new scheduled message to the queue.
            outputQueueItem = $"Ping message added to the queue at: {DateTime.Now}.";
            
            // Also write the message to the logs.
            log.Info(outputQueueItem);
        }

    Při provedení funkce tento kód přidá do fronty novou zprávu s aktuálním datem a časem.

7. Klikněte na **Save** (Uložit) a sledujte okna **Logs** (Protokoly), ve kterých se zobrazí další provádění funkce.

8. (Volitelné) Přejděte do účtu úložiště a ověřte si, jestli se do fronty přidávají zprávy.

9. Vraťte se na kartu **Integrate** (Integrace) a změňte pole plánu na hodnotu `0 0 * * * *`. Funkce se teď bude spouštět jednou za hodinu. 

Tohle je velmi zjednodušený příklad svázání výstupu časovacího triggeru a fronty úložiště. Další informace najdete v tématech [Časovací trigger služby Azure Functions](functions-bindings-timer.md) a [Triggery a vazby služby Azure Functions pro službu Azure Storage](functions-bindings-storage.md).

##Další kroky

Další informace o službě Azure Functions najdete v těchto tématech.

+ [Referenční informace pro vývojáře Azure Functions](functions-reference.md)  
Referenční informace pro programátory týkající se kódování funkcí a definování triggerů a vazeb.
+ [Testování funkcí Azure Functions](functions-test-a-function.md)  
Toto téma popisuje různé nástroje a techniky pro testování funkcí.
+ [Postup škálování Azure Functions](functions-scale.md)  
Toto téma popisuje plány služby, které jsou dostupné s Azure Functions (včetně plánu služby Dynamic), a výběr správného plánu.  

[AZURE.INCLUDE [Getting Started Note](../../includes/functions-get-help.md)]



<!--HONumber=Aug16_HO4-->


