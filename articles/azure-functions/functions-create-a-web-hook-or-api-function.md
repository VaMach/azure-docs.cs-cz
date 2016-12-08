---
title: "Vytvoření funkce Azure volané webhookem nebo rozhraním API | Dokumentace Microsoftu"
description: "Pomocí služby Azure Functions vytvoříte funkci, která je volána webhookem nebo rozhraním API."
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
ms.date: 08/30/2016
ms.author: glenga
translationtype: Human Translation
ms.sourcegitcommit: 47a89987b65b442c7b489984a4fc139eb1b82758
ms.openlocfilehash: 8efde94c5771212b4549f10882a4e55739231d61


---
# <a name="create-a-webhook-or-api-azure-function"></a>Vytvoření funkce Azure volané webhookem nebo rozhraním API
Azure Functions je výpočetní prostředí na vyžádání založené na událostech. Umožňuje vytvářet naplánované nebo aktivované jednotky kódu implementované v různých programovacích jazycích. Další informace o Azure Functions najdete v tématu [Přehled Azure Functions](functions-overview.md).

Toto téma ukazuje, jak vytvořit funkci Node.js, která je vyvolána webhookem GitHubu. Nová funkce je vytvořena na základě předem definované šablony na portálu Azure Functions. Můžete také zhlédnout krátké video a podívat se, jak se tyto kroky na portálu provádějí.

## <a name="watch-the-video"></a>Přehrát video
Následující video ukazuje, jak provést základní kroky obsažené v tomto kurzu. 

>[!VIDEO https://channel9.msdn.com/Series/Windows-Azure-Web-Sites-Tutorials/Create-a-Web-Hook-or-API-Azure-Function/player]
>
>

## <a name="create-a-webhook-triggered-function-from-the-template"></a>Vytvoření funkce aktivované webhookem ze šablony
Provádění funkcí v Azure je hostováno v aplikaci funkce. Než vytvoříte funkci, potřebujete mít aktivní účet Azure. Pokud ještě nemáte účet Azure, [můžete použít bezplatné účty](https://azure.microsoft.com/free/). 

1. Přejděte na stránku [portálu Azure Functions](https://functions.azure.com/signin) a přihlaste se pomocí účtu Azure.

2. Pokud máte existující aplikaci Function App, kterou můžete použít, vyberte ji v části **Your function apps** (Vaše aplikace Function App) a klikněte na **Open** (Otevřít). Pokud chcete vytvořit aplikaci Function App, zadejte pro ni jedinečnou položku **Name** (Název) nebo přijměte vygenerovaný název, vyberte upřednostňovanou položku **Region** (Oblast) a klikněte na **Create + get started** (Vytvořit a začít). 

3. Ve své aplikaci Function App klikněte na **+ New Function** (+ Nová funkce) > **GitHub Webhook - Node** (Webhook GitHubu - uzel) > **Create** (Vytvořit). Tento krok vytvoří funkci s výchozím názvem, která je založena na zadané šabloně. 
   
    ![Vytvoření funkce aktivované webhookem GitHubu](./media/functions-create-a-web-hook-or-api-function/functions-create-new-github-webhook.png) 

4. V části **Develop** (Vývoj) si povšimněte funkce express.js v okně **Code** (Kód). Tato funkce přijme požadavek GitHubu od webhooku komentáře k problému, vytvoří protokol problému a odešle webhooku odezvu v podobě `New GitHub comment: <Your issue comment text>`.

    ![Kontrola kódu funkce](./media/functions-create-a-web-hook-or-api-function/functions-new-webhook-in-portal.png) 

1. Zkopírujte hodnoty **Function URL** a **GitHub Secret**. Tyto hodnoty potřebujete k vytvoření webhooku na GitHubu. 

2. Přejděte dolů na tlačítko **Run** (Spustit), všimněte si předdefinovaného obsahu JSON komentáře k problému v textu žádosti a pak klikněte na tlačítko **Run** (Spustit). 
   
    Novou funkci založenou na šabloně můžete vždy otestovat přímo v záložce **Develop** (Vývoj) zadáním dat JSON a kliknutím na tlačítko **Run** (Spustit). V tomto případě má šablona předdefinovaný text komentáře k problému. 

Dále vytvoříte skutečný webhook ve vašem úložišti GitHubu.

## <a name="configure-the-webhook"></a>Konfigurace webhooku
1. V GitHubu přejděte do úložiště, které vlastníte. Můžete také použít libovolná úložiště, která máte rozvětvená.
 
2. Klikněte na tlačítko **Settings** (Nastavení)  > **Webhooks & services** (Webhooky a služby)  > **Add webhook** (Přidat webhook).
   
    ![Přidání webhooku GitHubu](./media/functions-create-a-web-hook-or-api-function/functions-create-new-github-webhook-2.png)   
3. Vložte adresu URL funkce a tajný klíč do polí **Payload URL** (URL datové části) a **Secret** (Tajný klíč), poté klikněte na položku **Let me select individual events** (Vyberu jednotlivé události), vyberte položku **Issue comment** (Komentář k problému) a klikněte na tlačítko **Add webhook** (Přidat webhook).
   
    ![Nastavení adresy URL a tajného klíče webhooku](./media/functions-create-a-web-hook-or-api-function/functions-create-new-github-webhook-3.png) 

V tomto okamžiku je webhook GitHubu nakonfigurován tak, aby aktivoval funkci v případě, že dojde k přidání nového komentáře k problému.  
Teď je čas na otestování.

## <a name="test-the-function"></a>Testování funkce
1. Ve svém úložišti GitHubu otevřete v novém okně prohlížeče záložku **Issues** (Problémy), klikněte na položku **New Issue** (Nový problém), zadejte název a stiskněte tlačítko **Submit new issue** (Odeslat nový problém). Můžete také otevřít stávající problém.

2. U problému zadejte komentář a klikněte na tlačítko **Comment** (Komentář). V tomto okamžiku se můžete vrátit k vašemu novému webhooku GitHubu a v části **Recent Deliveries** (Poslední doručené) naleznete informace o tom, že byl odeslán požadavek webhooku a text odpovědi je `New GitHub comment: <Your issue comment text>`.

3. Zpět na portálu Functions se posuňte dolů na protokoly a uvidíte, že došlo ke spuštění funkce a hodnota `New GitHub comment: <Your issue comment text>` je zapsána v datovém proudu protokolů.

## <a name="next-steps"></a>Další kroky
Další informace o službě Azure Functions najdete v těchto tématech.

* [Referenční informace pro vývojáře Azure Functions](functions-reference.md)  
  Příručka programátora pro psaní funkcí.
* [Testování Azure Functions](functions-test-a-function.md)  
  Toto téma popisuje různé nástroje a techniky pro testování funkcí.
* [Postup škálování Azure Functions](functions-scale.md)  
  Toto téma popisuje plány služby, které jsou dostupné se službou Azure Functions (včetně plánu hostování Consumption), a výběr správného plánu.  

[!INCLUDE [Getting Started Note](../../includes/functions-get-help.md)]




<!--HONumber=Nov16_HO4-->


