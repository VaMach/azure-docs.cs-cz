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
ms.translationtype: HT
ms.sourcegitcommit: 54454e98a2c37736407bdac953fdfe74e9e24d37
ms.openlocfilehash: 01b23996a9cd3a8368a78085dec905d020612b48
ms.contentlocale: cs-cz
ms.lasthandoff: 07/13/2017

---
# <a name="create-your-first-function-in-the-azure-portal"></a>Vytvoření první funkce na webu Azure Portal

Služba Azure Functions umožňuje spuštění kódu v prostředí bez serveru, aniž byste nejdřív museli vytvořit virtuální počítač nebo publikovat webovou aplikaci. V tomto tématu se dozvíte, jak pomocí služby Functions vytvořit na webu Azure Portal funkci Hello World.

![Vytvoření aplikace Function App na webu Azure Portal](./media/functions-create-first-azure-function/function-app-in-portal-editor.png)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="log-in-to-azure"></a>Přihlaste se k Azure.

Přihlaste se k portálu [Azure Portal](https://portal.azure.com/).

## <a name="create-a-function-app"></a>Vytvoření Function App

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

## <a name="test-the-function"></a>Testování funkce

1. V nové funkci klikněte na **</> Získat adresu URL funkce**, vyberte **výchozí (klíč funkce)** a pak na **Kopírovat**. 

    ![Kopírování adresy URL funkce z webu Azure Portal](./media/functions-create-first-azure-function/function-app-develop-tab-testing.png)

2. Vložte adresu URL funkce do panelu Adresa vašeho prohlížeče. Připojte k této adrese URL řetězec dotazu `&name=<yourname>` a stisknutím klávesy `Enter` na klávesnici požadavek proveďte. Následuje příklad odpovědi vrácené funkcí v prohlížeči Edge:

    ![Odezva funkce v prohlížeči.](./media/functions-create-first-azure-function/function-app-browser-testing.png)

    Adresa URL požadavku obsahuje klíč, který je ve výchozím nastavení nezbytný pro přístup k funkci přes protokol HTTP.   

## <a name="view-the-function-logs"></a>Zobrazení protokolů funkce 

Při spuštění funkce se do protokolů zaznamenávají informace o trasování. Pokud chcete zobrazit výstup trasování z předchozího zpracování, vraťte se na funkce na portálu a kliknutím na šipku nahoru ve spodní části obrazovky rozbalte položku **Protokoly**. 

![Prohlížeč protokolu funkcí na webu Azure Portal.](./media/functions-create-first-azure-function/function-view-logs.png)

## <a name="clean-up-resources"></a>Vyčištění prostředků

[!INCLUDE [Clean up resources](../../includes/functions-quickstart-cleanup.md)]

## <a name="next-steps"></a>Další kroky

Vytvořili jste aplikaci Function App s jednoduchou funkcí aktivovanou protokolem HTTP.  

[!INCLUDE [Next steps note](../../includes/functions-quickstart-next-steps.md)]

Další informace najdete v tématu [Vazby protokolu HTTP služby Azure Functions a vazby webhooku](functions-bindings-http-webhook.md).




