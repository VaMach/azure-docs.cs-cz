---
title: "Vytvoření funkce v Azure, která se spouští podle plánu | Dokumentace Microsoftu"
description: "Zjistěte, jak v Azure vytvořit funkci, která se spouští na základě vámi definovaného plánu."
services: functions
documentationcenter: na
author: ggailey777
manager: erikre
editor: 
tags: 
ms.assetid: ba50ee47-58e0-4972-b67b-828f2dc48701
ms.service: functions
ms.devlang: multiple
ms.topic: get-started-article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 05/31/2017
ms.author: glenga
ms.custom: mvc
ms.translationtype: Human Translation
ms.sourcegitcommit: 09f24fa2b55d298cfbbf3de71334de579fbf2ecd
ms.openlocfilehash: 4442d0038a0604d3297871907c1d05d8d3916dcf
ms.contentlocale: cs-cz
ms.lasthandoff: 06/07/2017

---
# <a name="create-a-function-in-azure-that-is-triggered-by-a-timer"></a>Vytvoření funkce v Azure aktivované časovačem

Zjistěte, jak ve službě Azure Functions vytvořit funkci, která se spouští na základě vámi definovaného plánu.

![Vytvoření aplikace Function App na webu Azure Portal](./media/functions-create-scheduled-function/function-app-in-portal-editor.png)

## <a name="prerequisites"></a>Požadavky

K provedení kroků v tomto kurzu je potřeba:

+ Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) před tím, než začnete.

[!INCLUDE [functions-portal-favorite-function-apps](../../includes/functions-portal-favorite-function-apps.md)]

## <a name="create-an-azure-function-app"></a>Vytvoření aplikace Azure Function App

[!INCLUDE [Create function app Azure portal](../../includes/functions-create-function-app-portal.md)]

![Aplikace Function App byla úspěšně vytvořena.](./media/functions-create-first-azure-function/function-app-create-success.png)

Dál vytvoříte v nové aplikaci Function App funkci.

<a name="create-function"></a>

## <a name="create-a-timer-triggered-function"></a>Vytvoření funkce aktivované časovačem

1. Rozbalte aplikaci Function App a klikněte na tlačítko **+** vedle položky **Funkce**. Pokud jde o první funkci ve vaší aplikaci Function App, vyberte možnost **Vlastní funkce**. Zobrazí se kompletní sada šablon funkcí.

    ![Stručný úvod do služby Functions na webu Azure Portal](./media/functions-create-scheduled-function/add-first-function.png)

2. Vyberte šablonu **TimerTrigger** pro vámi požadovaný jazyk. Potom použijte nastavení uvedená v tabulce:

    ![Vytvořte na portálu Azure Portal funkci aktivovanou časovačem.](./media/functions-create-scheduled-function/functions-create-timer-trigger.png)

    | Nastavení | Navrhovaná hodnota | Popis |
    |---|---|---|
    | **Pojmenujte svoji funkci** | TimerTriggerCSharp1 | Určuje název funkce aktivované časovačem. |
    | **[Plán](http://en.wikipedia.org/wiki/Cron#CRON_expression)** | 0 \*/1 \* \* \* \* | Pole [Výraz CRON](http://en.wikipedia.org/wiki/Cron#CRON_expression) v šestkové soustavě, ve kterém naplánujete spouštění funkce každou minutu. |

2. Klikněte na možnost **Vytvořit**. Ve zvoleném jazyce se vytvoří funkce, která se bude spouštět každou minutu.

3. Podívejte se na informace o trasování zaznamenané v protokolech a ověřte provedení.

    ![Prohlížeč protokolu funkcí na webu Azure Portal.](./media/functions-create-scheduled-function/functions-timer-trigger-view-logs2.png)

Teď můžete změnit plán funkce tak, aby se funkce spouštěla méně často, třeba jednou za hodinu. 

## <a name="update-the-timer-schedule"></a>Aktualizace plánu časovače

1. Rozbalte funkci a klikněte na **Integrace**. Tady se určují vstupní a výstupní vazby funkce a nastavuje plán. 

2. V poli **Plán** zadejte novou hodnotu `0 0 */1 * * *` a potom klikněte na **Uložit**.  

![Aktualizace plánu časovače funkcí na webu Azure Portal.](./media/functions-create-scheduled-function/functions-timer-trigger-change-schedule.png)

Teď máte funkci, která se spouští jednou za hodinu. 

## <a name="clean-up-resources"></a>Vyčištění prostředků

[!INCLUDE [Next steps note](../../includes/functions-quickstart-cleanup.md)]

## <a name="next-steps"></a>Další kroky

Vytvořili jste funkci, která se spouští na základě plánu.

[!INCLUDE [Next steps note](../../includes/functions-quickstart-next-steps.md)]

Další informace o aktivačních časovačích najdete v tématu [Plánování spouštění kódu v Azure Functions](functions-bindings-timer.md).
