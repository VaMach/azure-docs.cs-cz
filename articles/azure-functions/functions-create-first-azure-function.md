---
title: "Vytvoření první funkce na webu Azure Portal | Dokumentace Microsoftu"
description: "Naučíte se postup vytvoření první funkce Azure Function pro provádění pomocí webu Azure Portal bez serveru."
services: functions
documentationcenter: na
author: ggailey777
manager: erikre
editor: 
tags: 
ms.assetid: 96cf87b9-8db6-41a8-863a-abb828e3d06d
ms.service: functions
ms.devlang: multiple
ms.topic: hero-article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 06/08/2017
ms.author: glenga
ms.custom: mvc
ms.translationtype: Human Translation
ms.sourcegitcommit: 245ce9261332a3d36a36968f7c9dbc4611a019b2
ms.openlocfilehash: f00ca3b8a35c0c49277457bd42fe8a314520d5a5
ms.contentlocale: cs-cz
ms.lasthandoff: 06/09/2017


---
# Vytvoření první funkce na webu Azure Portal
<a id="create-your-first-function-in-the-azure-portal" class="xliff"></a>

Služba Azure Functions umožňuje spuštění kódu v prostředí bez serveru, aniž byste nejdřív museli vytvořit virtuální počítač nebo publikovat webovou aplikaci. V tomto tématu se dozvíte, jak pomocí služby Functions vytvořit na webu Azure Portal funkci Hello World.

![Vytvoření aplikace Function App na webu Azure Portal](./media/functions-create-first-azure-function/function-app-in-portal-editor.png)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## Přihlaste se k Azure.
<a id="log-in-to-azure" class="xliff"></a>

Přihlaste se k portálu [Azure Portal](https://portal.azure.com/).

## Vytvoření Function App
<a id="create-a-function-app" class="xliff"></a>

K hostování provádění funkcí musíte mít aplikaci Function App. Aplikace Function App umožňuje seskupit funkce jako logickou jednotku pro snadnější správu, nasazování a sdílení prostředků. 

[!INCLUDE [Create function app Azure portal](../../includes/functions-create-function-app-portal.md)]

![Aplikace Function App byla úspěšně vytvořena.](./media/functions-create-first-azure-function/function-app-create-success.png)

[!INCLUDE [functions-portal-favorite-function-apps](../../includes/functions-portal-favorite-function-apps.md)]

Dál vytvoříte v nové aplikaci Function App funkci.

## <a name="create-function"></a>Vytvoření funkce aktivované protokolem HTTP

1. Rozbalte novou aplikaci Function App a potom klikněte na tlačítko  **+**  vedle položky **Funkce**.

2.  Na stránce **Začněte rychle** vyberte **Webhook + API**, vyberte jazyk funkce a klikněte na **Vytvořit tuto funkci**. 
   
    ![Stručný úvod do služby Functions na webu Azure Portal.](./media/functions-create-first-azure-function/function-app-quickstart-node-webhook.png)

Funkce ve vybraném jazyce se vytvoří pomocí šablony funkce aktivované protokolem HTTP. Novou funkci můžete spustit odesláním požadavku HTTP.

## Testování funkce
<a id="test-the-function" class="xliff"></a>

1. V nové funkci klikněte na **</> Získat adresu URL funkce**, vyberte **výchozí (klíč funkce)** a pak na **Kopírovat**. 

    ![Kopírování adresy URL funkce z webu Azure Portal](./media/functions-create-first-azure-function/function-app-develop-tab-testing.png)

2. Vložte adresu URL pro požadavek HTTP do panelu adresy prohlížeče. K této adrese URL připojte řetězec dotazu `&name=<yourname>` a proveďte požadavek. Následuje ukázka odezvy na požadavek GET vrácené funkcí v prohlížeči:

    ![Odezva funkce v prohlížeči.](./media/functions-create-first-azure-function/function-app-browser-testing.png)

    Adresa URL požadavku obsahuje klíč, který je ve výchozím nastavení nezbytný pro přístup k funkci přes protokol HTTP.   

## Zobrazení protokolů funkce
<a id="view-the-function-logs" class="xliff"></a> 

Při spuštění funkce se do protokolů zaznamenávají informace o trasování. Pokud chcete zobrazit výstup trasování z předchozího zpracování, vraťte se na funkce na portálu a kliknutím na šipku nahoru ve spodní části obrazovky rozbalte položku **Protokoly**. 

![Prohlížeč protokolu funkcí na webu Azure Portal.](./media/functions-create-first-azure-function/function-view-logs.png)

## Vyčištění prostředků
<a id="clean-up-resources" class="xliff"></a>

[!INCLUDE [Clean up resources](../../includes/functions-quickstart-cleanup.md)]

## Další kroky
<a id="next-steps" class="xliff"></a>

Vytvořili jste aplikaci Function App s jednoduchou funkcí aktivovanou protokolem HTTP.  

[!INCLUDE [Next steps note](../../includes/functions-quickstart-next-steps.md)]

Další informace najdete v tématu [Vazby protokolu HTTP služby Azure Functions a vazby webhooku](functions-bindings-http-webhook.md).




