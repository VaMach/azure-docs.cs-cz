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
ms.date: 04/18/2017
ms.author: glenga
translationtype: Human Translation
ms.sourcegitcommit: 9eafbc2ffc3319cbca9d8933235f87964a98f588
ms.openlocfilehash: d4354546f3342d65353a86a4cec7d02547ab92e7
ms.lasthandoff: 04/22/2017


---
# <a name="create-a-function-triggered-by-a-github-webhook"></a>Vytvoření funkce aktivované webhookem GitHubu

Naučte se vytvořit funkci, která je aktivována webhookem GitHubu. 

![Vytvoření aplikace Function App na webu Azure Portal](./media/functions-create-github-webhook-triggered-function/function-app-in-portal-editor.png)

Toto téma vyžaduje prostředky vytvořené v tématu [Vytvoření první funkce na webu Azure Portal](functions-create-first-azure-function.md).

Potřebujete také účet GitHub. Můžete si [zaregistrovat bezplatný účet GitHub](https://github.com/join), pokud jej ještě nemáte. 

K dokončení všech kroků v tomto tématu by vám mělo stačit méně než 5 minut.

## <a name="find-your-function-app"></a>Vyhledání aplikace Function App    

1. Přihlaste se k [portálu Azure](https://portal.azure.com/). 

2. Na panelu hledání v horní části portálu zadejte název vaší aplikace Function App a vyberte ji ze seznamu.

## <a name="create-function"></a>Vytvoření funkce aktivované webhookem GitHubu

1. V aplikaci Function App klikněte na tlačítko **+** vedle možnosti **Funkce**, klikněte na šablonu **GitHubWebHook** pro požadovaný jazyk a klikněte na **Vytvořit**.
   
    ![Vytvoření funkce aktivované webhookem GitHubu na webu Azure Portal.](./media/functions-create-github-webhook-triggered-function/functions-create-github-webhook-trigger.png) 

2. Klikněte na **</> Získat adresu URL funkce** a pak zkopírujte a uložte příslušné hodnoty. To samé udělejte s možností **</> Získat tajný klíč GitHubu**. Tyto hodnoty použijete ke konfiguraci webhooku na GitHubu. 

    ![Kontrola kódu funkce](./media/functions-create-github-webhook-triggered-function/functions-copy-function-url-github-secret.png) 
         
Dále vytvoříte webhook ve vašem úložišti GitHub. 

## <a name="configure-the-webhook"></a>Konfigurace webhooku
1. V GitHubu přejděte do úložiště, které vlastníte. Můžete také použít libovolné úložiště, které máte rozvětvené.
 
2. Klikněte na **Settings** (Nastavení), pak na **Webhooks** (Webhooky) a **Add webhook** (Přidat webhook).
   
    ![Přidání webhooku GitHubu](./media/functions-create-github-webhook-triggered-function/functions-create-new-github-webhook-2.png)

3. Vložte adresu URL funkce a tajný klíč do polí **Payload URL** (URL datové části) a **Secret** (Tajný klíč) a jako **Content type** (Typ obsahu) vyberte **application/json**.

4. Klikněte na **Let me select individual events** (Vyberu jednotlivé události), vyberte **Issue comment** (Komentář k problému) a klikněte na **Add webhook** (Přidat webhook).
   
    ![Nastavení adresy URL a tajného klíče webhooku](./media/functions-create-github-webhook-triggered-function/functions-create-new-github-webhook-3.png)

Teď je webhook nakonfigurován tak, aby aktivoval vaši funkci v případě, že dojde k přidání nového komentáře k problému. 

## <a name="test-the-function"></a>Testování funkce
1. Ve vašem úložišti GitHub otevřete kartu **Issues** (Problémy) v novém okně prohlížeče.

2. V novém okně klikněte na **New Issue** (Nový problém), zadejte název a klikněte na **Submit new issue** (Odeslat nový problém). 

2. U problému zadejte komentář a klikněte na tlačítko **Comment** (Komentář). 

3. V druhém okně s GitHubem klikněte na **Edit** (Upravit) vedle vašeho nového webhooku, přejděte dolů k části **Recent Deliveries** (Poslední doručené) a ověřte, že vaše funkce zpracovala požadavek webhooku. 
 
    ![Nastavení adresy URL a tajného klíče webhooku](./media/functions-create-github-webhook-triggered-function/functions-github-webhook-triggered.png)

   Odpověď z vaší funkce by měla obsahovat `New GitHub comment: <Your issue comment text>` (Nový komentář na GitHubu: <Text vašeho komentáře k problému>).

## <a name="next-steps"></a>Další kroky

[!INCLUDE [Next steps note](../../includes/functions-quickstart-next-steps.md)]

[!INCLUDE [Getting Started Note](../../includes/functions-get-help.md)]


