---
title: "Vytvoření funkce aktivované webhookem GitHubu v Azure | Dokumentace Microsoftu"
description: "Pomocí služby Azure Functions vytvoříte funkci bez serveru, která je vyvolána webhookem GitHubu."
services: azure-functions
documentationcenter: na
author: ggailey777
manager: erikre
editor: 
tags: 
ms.assetid: 36ef34b8-3729-4940-86d2-cb8e176fcc06
ms.service: functions
ms.devlang: multiple
ms.topic: get-started-article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 05/02/2017
ms.author: glenga
ms.translationtype: Human Translation
ms.sourcegitcommit: fc4172b27b93a49c613eb915252895e845b96892
ms.openlocfilehash: 7423ef26a4a1c49245a1f8df79bbcaabea222596
ms.contentlocale: cs-cz
ms.lasthandoff: 05/12/2017


---
# <a name="create-a-function-triggered-by-a-github-webhook"></a>Vytvoření funkce aktivované webhookem GitHubu

Naučte se vytvořit funkci, která se aktivuje požadavkem webhooku HTTP s datovou částí specifickou pro GitHub.

![Funkce aktivovaná webhookem GitHubu na webu Azure Portal](./media/functions-create-github-webhook-triggered-function/function-app-in-portal-editor.png)

K dokončení všech kroků v tomto tématu by vám mělo stačit méně než 5 minut.

## <a name="prerequisites"></a>Požadavky

Před spuštěním této ukázky musíte mít následující:

- Účet GitHubu s alespoň jedním projektem.

Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) před tím, než začnete.

[!INCLUDE [functions-portal-favorite-function-apps](../../includes/functions-portal-favorite-function-apps.md)]

## <a name="create-an-azure-function-app"></a>Vytvoření aplikace Azure Function App

[!INCLUDE [Create function app Azure portal](../../includes/functions-create-function-app-portal.md)]

![Aplikace Function App byla úspěšně vytvořena.](./media/functions-create-first-azure-function/function-app-create-success.png)

Dál vytvoříte v nové aplikaci Function App funkci.

<a name="create-function"></a>

## <a name="create-a-github-webhook-triggered-function"></a>Vytvoření funkce aktivované webhookem GitHubu

1. Rozbalte aplikaci Function App, klikněte na tlačítko **+** vedle možnosti **Funkce** a klikněte na šablonu **GitHubWebHook** pro požadovaný jazyk. **Pojmenujte svoji funkci** a potom klikněte na **Vytvořit**.

1. V nové funkci klikněte na **</> Získat adresu URL funkce** a potom zkopírujte a uložte příslušné hodnoty. To samé udělejte s možností **</> Získat tajný klíč GitHubu**. Tyto hodnoty použijete ke konfiguraci webhooku na GitHubu.

    ![Kontrola kódu funkce](./media/functions-create-github-webhook-triggered-function/functions-copy-function-url-github-secret.png)

Dále vytvoříte webhook ve vašem úložišti GitHub.

## <a name="configure-the-webhook"></a>Konfigurace webhooku

1. V GitHubu přejděte do úložiště, které vlastníte. Můžete také použít libovolné úložiště, které máte rozvětvené. Pokud potřebujete úložiště rozvětvit, použijte stránku <https://github.com/Azure-Samples/functions-quickstart>.

1. Klikněte na **Settings** (Nastavení), pak na **Webhooks** (Webhooky) a **Add webhook** (Přidat webhook).

    ![Přidání webhooku GitHubu](./media/functions-create-github-webhook-triggered-function/functions-create-new-github-webhook-2.png)

1. Použijte nastavení uvedené v tabulce a potom klikněte na **Přidat webhook**.

    ![Nastavení adresy URL a tajného klíče webhooku](./media/functions-create-github-webhook-triggered-function/functions-create-new-github-webhook-3.png)

| Nastavení | Navrhovaná hodnota | Popis |
|---|---|---|
| **Datová část adresy URL** | Zkopírovaná hodnota | Použijte hodnotu vrácenou příkazem **</> Získat adresu URL funkce**. |
| **Tajný kód**   | Zkopírovaná hodnota | Použijte hodnotu vrácenou příkazem **</> Získat tajný kód GitHubu**. |
| **Typ obsahu** | application/json | Funkce očekává datovou část JSON. |
| Aktivační události | Nechat mě vybrat jednotlivé události | Chceme provést aktivaci jenom při událostech komentářů k problémům.  |
| | Komentář k problému |  |

Teď je webhook nakonfigurován tak, aby aktivoval vaši funkci v případě, že dojde k přidání nového komentáře k problému.

## <a name="test-the-function"></a>Testování funkce

1. Ve vašem úložišti GitHub otevřete kartu **Issues** (Problémy) v novém okně prohlížeče.

1. V novém okně klikněte na **New Issue** (Nový problém), zadejte název a klikněte na **Submit new issue** (Odeslat nový problém).

1. U problému zadejte komentář a klikněte na tlačítko **Comment** (Komentář).

    ![Přidejte komentář k problému Githubu.](./media/functions-create-github-webhook-triggered-function/functions-github-webhook-add-comment.png)

1. Vraťte se na portál a podívejte se do protokolů. Měli byste vidět položku trasování s textem nového komentáře.

     ![Zobrazte si text komentáře v protokolech.](./media/functions-create-github-webhook-triggered-function/function-app-view-logs.png)

## <a name="clean-up-resources"></a>Vyčištění prostředků

[!INCLUDE [Next steps note](../../includes/functions-quickstart-cleanup.md)]

## <a name="next-steps"></a>Další kroky

Vytvořili jste funkci, která se spustí při přijetí požadavku z webhooku Githubu. 
[!INCLUDE [Next steps note](../../includes/functions-quickstart-next-steps.md)] Další informace o aktivačních webhoocích najdete v tématu [Vazby protokolu HTTP služby Azure Functions a vazby webhooku](functions-bindings-http-webhook.md).
