---
title: "Vytvoří funkci, v Azure aktivovány obecný webhook | Microsoft Docs"
description: "Použití Azure Functions vytvořit funkci bez serveru, který je vyvolán webhooku v Azure."
services: functions
documentationcenter: na
author: ggailey777
manager: cfowler
editor: 
tags: 
ms.assetid: fafc10c0-84da-4404-b4fa-eea03c7bf2b1
ms.service: functions
ms.devlang: multiple
ms.topic: quickstart
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 08/12/2017
ms.author: glenga
ms.custom: mvc
ms.openlocfilehash: f283f8d79c5ae5fb6a72c84c9e9edb7bb8de4a83
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="create-a-function-triggered-by-a-generic-webhook"></a>Vytvoření funkce aktivovány obecný webhook

Služba Azure Functions umožňuje spuštění kódu v prostředí bez serveru, aniž byste nejdřív museli vytvořit virtuální počítač nebo publikovat webovou aplikaci. Můžete například nakonfigurovat funkce, která se aktivovat výstrahu aktivováno monitorování Azure. Toto téma ukazuje, jak provádět kód C#, pokud skupina prostředků se přidá do vašeho předplatného.   

![Obecný webhook aktivuje funkce na portálu Azure](./media/functions-create-generic-webhook-triggered-function/function-completed.png)

## <a name="prerequisites"></a>Požadavky 

K provedení kroků v tomto kurzu je potřeba:

+ Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) před tím, než začnete.

[!INCLUDE [functions-portal-favorite-function-apps](../../includes/functions-portal-favorite-function-apps.md)]

## <a name="create-an-azure-function-app"></a>Vytvoření aplikace Azure Function App

[!INCLUDE [Create function app Azure portal](../../includes/functions-create-function-app-portal.md)]

Dál vytvoříte v nové aplikaci Function App funkci.

## <a name="create-function"></a>Vytvoření obecný webhook aktivované funkce

1. Rozbalte aplikaci Function App a klikněte na tlačítko **+** vedle položky **Funkce**. Pokud tuto funkci je první z nich v aplikaci funkce, vyberte **vlastní funkce**. Zobrazí se kompletní sada šablon funkcí.

    ![Stručný úvod do služby Functions na webu Azure Portal](./media/functions-create-generic-webhook-triggered-function/add-first-function.png)

2. Vyberte **obecný WebHook - C#** šablony. Zadejte název pro funkce C# a pak vyberte **vytvořit**.

     ![Vytvoří funkci, obecný webhook aktivuje na portálu Azure](./media/functions-create-generic-webhook-triggered-function/functions-create-generic-webhook-trigger.png) 

2. V nové funkce, klikněte na **URL funkce <> / Get**, zkopírujte a uložte hodnotu. Tuto hodnotu použijete ke konfiguraci webhooku. 

    ![Kontrola kódu funkce](./media/functions-create-generic-webhook-triggered-function/functions-copy-function-url.png)
         
Dále vytvoříte koncový bod webhooku ve výstraze protokolu aktivit v Azure monitorování. 

## <a name="create-an-activity-log-alert"></a>Vytvořit výstrahu protokolu aktivit

1. Na portálu Azure přejděte do **monitorování** služby, vyberte **výstrahy**a klikněte na tlačítko **přidat aktivitu protokolu upozornění**.   

    ![Monitorování](./media/functions-create-generic-webhook-triggered-function/functions-monitor-add-alert.png)

2. Použijte nastavení uvedená v tabulce:

    ![Vytvořit výstrahu protokolu aktivit](./media/functions-create-generic-webhook-triggered-function/functions-monitor-add-alert-settings.png)

    | Nastavení      |  Navrhovaná hodnota   | Popis                              |
    | ------------ |  ------- | -------------------------------------------------- |
    | **Název výstrahy protokolu aktivit** | prostředek skupiny vytvořit – upozornění | Název protokolu výstraha aktivity. |
    | **Předplatné** | Vaše předplatné | Odběr, který používáte pro účely tohoto kurzu. | 
    |  **Skupina prostředků** | myResourceGroup | Výstrahy prostředky jsou nasazeny do skupiny prostředků. Pomocí stejné skupině prostředků jako aplikace funkce usnadňuje odstraňování až po dokončení kurzu. |
    | **Kategorie události** | Pro správu | Tato kategorie zahrnuje změny prostředků Azure.  |
    | **Typ prostředku** | Skupiny prostředků | Filtrování výstrah pro aktivity skupiny prostředků. |
    | **Skupina prostředků**<br/>a **prostředků** | Všechny | Monitorujte všechny prostředky. |
    | **Název operace** | Vytvoření skupiny prostředků | Filtrování výstrah pro vytvoření operací. |
    | **Úroveň** | Informační | Zahrnout informační úrovně výstrahy. | 
    | **Stav** | Úspěch | Filtrování výstrah na akce, které byly úspěšně dokončeny. |
    | **Akce skupiny** | Nový | Vytvořte novou skupinu akce, který definuje akce trvá, když je vydána výstraha. |
    | **Název skupiny akce** | Funkce webhooku | Název pro identifikaci skupiny akce.  | 
    | **Krátký název** | funcwebhook | Krátký název pro skupinu akce. |  

3. V **akce**, přidat akci pomocí nastavení uvedeného v tabulce: 

    ![Přidat skupinu akce](./media/functions-create-generic-webhook-triggered-function/functions-monitor-add-alert-settings-2.png)

    | Nastavení      |  Navrhovaná hodnota   | Popis                              |
    | ------------ |  ------- | -------------------------------------------------- |
    | **Název** | CallFunctionWebhook | Název akce. |
    | **Typ akce** | Webhook | Odpověď na výstrahy je, že adresa URL Webhooku se nazývá. |
    | **Podrobnosti** | URL – funkce | Vložte adresu URL webhooku funkce, který jste zkopírovali dříve. |v

4. Klikněte na tlačítko **OK** vytvořit skupinu výstrahy a akce.  

Webhook se nyní nazývá při vytvoření skupiny prostředků v rámci vašeho předplatného. Potom aktualizujte kód ve vaší funkci pro zpracování dat protokolu JSON v textu požadavku.   

## <a name="update-the-function-code"></a>Aktualizace kódu funkce

1. Přejděte zpět do aplikace pro funkce na portálu a rozšířit funkce. 

2. Nahraďte kód skriptu jazyka C# ve funkci portálu s následujícím kódem:

    ```csharp
    #r "Newtonsoft.Json"
    
    using System;
    using System.Net;
    using Newtonsoft.Json;
    using Newtonsoft.Json.Linq;
    
    public static async Task<object> Run(HttpRequestMessage req, TraceWriter log)
    {
        log.Info($"Webhook was triggered!");
    
        // Get the activityLog object from the JSON in the message body.
        string jsonContent = await req.Content.ReadAsStringAsync();
        JToken activityLog = JObject.Parse(jsonContent.ToString())
            .SelectToken("data.context.activityLog");
    
        // Return an error if the resource in the activity log isn't a resource group. 
        if (activityLog == null || !string.Equals((string)activityLog["resourceType"], 
            "Microsoft.Resources/subscriptions/resourcegroups"))
        {
            log.Error("An error occured");
            return req.CreateResponse(HttpStatusCode.BadRequest, new
            {
                error = "Unexpected message payload or wrong alert received."
            });
        }
    
        // Write information about the created resource group to the streaming log.
        log.Info(string.Format("Resource group '{0}' was {1} on {2}.",
            (string)activityLog["resourceGroupName"],
            ((string)activityLog["subStatus"]).ToLower(), 
            (DateTime)activityLog["submissionTimestamp"]));
    
        return req.CreateResponse(HttpStatusCode.OK);    
    }
    ```

Nyní můžete otestovat funkci tak, že vytvoříte novou skupinu prostředků ve vašem předplatném.

## <a name="test-the-function"></a>Testování funkce

1. Klikněte na ikonu skupiny prostředků v levé části portálu Azure, vyberte **+ přidat**, zadejte **název skupiny prostředků**a vyberte **vytvořit** k vytvoření skupiny prostředků prázdný.
    
    ![Vytvořte skupinu prostředků.](./media/functions-create-generic-webhook-triggered-function/functions-create-resource-group.png)

2. Vraťte se zpátky a funkce a rozbalte **protokoly** okno. Po vytvoření skupiny prostředků webhooku se aktivuje výstraha protokolu aktivity a provádí funkce. Zobrazí název nové skupiny prostředků zapisují do protokolů.  

    ![Přidáte nastavení aplikace testu.](./media/functions-create-generic-webhook-triggered-function/function-view-logs.png)

3. (Volitelné) Přejděte zpět a odstraňte skupinu prostředků, kterou jste vytvořili. Všimněte si, že tato aktivita nebude aktivovat funkce. Důvodem je, že odstranění operations filtrují výstraha. 

## <a name="clean-up-resources"></a>Vyčištění prostředků

[!INCLUDE [Next steps note](../../includes/functions-quickstart-cleanup.md)]

## <a name="next-steps"></a>Další kroky

Funkce, která se spouští při přijetí požadavku z obecný webhook jste vytvořili. 

[!INCLUDE [Next steps note](../../includes/functions-quickstart-next-steps.md)]

Další informace o aktivačních událostech webhooků najdete v tématu [Vazby protokolu HTTP služby Azure Functions a vazby webhooku](functions-bindings-http-webhook.md). Další informace o vývoji funkce v jazyce C#, najdete v části [Azure funkcí jazyka C# skript referenční informace pro vývojáře](functions-reference-csharp.md).

