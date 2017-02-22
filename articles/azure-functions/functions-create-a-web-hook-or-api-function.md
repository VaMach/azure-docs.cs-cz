---
title: "Vytvoření funkce Azure volané webhookem nebo rozhraním API | Dokumentace Microsoftu"
description: "Pomocí služby Azure Functions vytvoříte funkci bez serveru, která je vyvolána webhookem nebo voláním rozhraní API."
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
ms.date: 02/02/2017
ms.author: glenga
translationtype: Human Translation
ms.sourcegitcommit: a8f6d111a010666bf4aaaf05e061381cc8fffed0
ms.openlocfilehash: 23a65319fe1825e2ba51f2fd5a2d0b65ca499472


---
# <a name="create-a-webhook-or-api-azure-function"></a>Vytvoření funkce Azure volané webhookem nebo rozhraním API
Azure Functions je výpočetní prostředí na vyžádání založené na událostech. Umožňuje vytvářet naplánované nebo aktivované jednotky kódu implementované v různých programovacích jazycích. Další informace o Azure Functions najdete v tématu [Přehled Azure Functions](functions-overview.md).

Toto téma ukazuje, jak vytvořit funkci JavaScriptu, která je vyvolána webhookem GitHubu. Nová funkce je vytvořena na základě předem definované šablony na portálu Azure Functions. Můžete také zhlédnout krátké video a podívat se, jak se tyto kroky na portálu provádějí.

Obecné kroky v tomto kurzu lze použít i k vytvoření funkce jazyka C# nebo F# místo JavaScriptu. 

## <a name="watch-the-video"></a>Přehrát video
Následující video ukazuje, jak provést základní kroky obsažené v tomto kurzu. 

>[!VIDEO https://channel9.msdn.com/Series/Windows-Azure-Web-Sites-Tutorials/Create-a-Web-Hook-or-API-Azure-Function/player]
>
>

## <a name="prerequisites"></a>Požadavky

K dokončení tohoto kurzu potřebujete:

+ Aktivní účet Azure. Pokud ještě účet nemáte, můžete si [zaregistrovat bezplatný účet Azure](https://azure.microsoft.com/free/).  
 Můžete si také [vyzkoušet službu Functions](https://functions.azure.com/try) a dokončit tento kurz bez účtu Azure.
+ Účet GitHub. Můžete si [zaregistrovat bezplatný účet GitHub](https://github.com/join), pokud jej ještě nemáte. 

## <a name="create-a-webhook-triggered-function-from-the-template"></a>Vytvoření funkce aktivované webhookem ze šablony
Provádění funkcí v Azure je hostováno v aplikaci funkce. 

1. Přejděte na stránku [portálu Azure Functions](https://functions.azure.com/signin) a přihlaste se pomocí účtu Azure.

2. Pokud máte existující aplikaci Function App, kterou můžete použít, vyberte ji v části **Your function apps** (Vaše aplikace Function App) a klikněte na **Open** (Otevřít). Pokud chcete vytvořit aplikaci Function App, zadejte pro ni jedinečnou položku **Name** (Název) nebo přijměte vygenerovaný název, vyberte upřednostňovanou položku **Region** (Oblast) a klikněte na **Create + get started** (Vytvořit a začít). 

3. Ve své aplikaci Function App klikněte na **+ New Function** (+ Nová funkce) > **GitHub Webhook – JavaScript** (Webhook GitHubu – JavaScript) > **Create** (Vytvořit). Tento krok vytvoří funkci s výchozím názvem, která je založena na zadané šabloně. Případně můžete vytvořit funkci jazyka C# nebo F#.
   
    ![Vytvoření funkce aktivované webhookem GitHubu](./media/functions-create-a-web-hook-or-api-function/functions-create-new-github-webhook.png) 

4. V části **Develop** (Vývoj) si povšimněte funkce express.js v okně **Code** (Kód). Tato funkce přijme požadavek GitHubu od webhooku komentáře k problému, vytvoří protokol problému a odešle webhooku odezvu v podobě `New GitHub comment: <Your issue comment text>`.

    ![Kontrola kódu funkce](./media/functions-create-a-web-hook-or-api-function/functions-new-webhook-in-portal.png) 

1. Zkopírujte a uložte hodnoty **Function URL** a **GitHub Secret**. Tyto hodnoty použijete v další části ke konfiguraci webhooku na GitHubu. 

2. Klikněte na **Test** (Testovat), všimněte si předdefinovaného obsahu JSON komentáře k problému v **textu žádosti** a pak klikněte na **Run** (Spustit). 

    ![Testování funkce webhooku v portálu](./media/functions-create-a-web-hook-or-api-function/functions-test-webhook-in-portal.png)
   
    > [!NOTE]
    > Novou funkci založenou na šabloně můžete vždy otestovat přímo v záložce **Develop** (Vývoj) zadáním dat JSON a kliknutím na tlačítko **Run** (Spustit). V tomto případě má šablona předdefinovaný text komentáře k problému. 

Dále vytvoříte skutečný webhook ve vašem úložišti GitHubu.

## <a name="configure-the-webhook"></a>Konfigurace webhooku
1. V GitHubu přejděte do úložiště, které vlastníte. Můžete také použít libovolná úložiště, která máte rozvětvená.
 
2. Klikněte na tlačítko **Settings** (Nastavení)  > **Webhooks & services** (Webhooky a služby)  > **Add webhook** (Přidat webhook).
   
    ![Přidání webhooku GitHubu](./media/functions-create-a-web-hook-or-api-function/functions-create-new-github-webhook-2.png)   

3. Vložte adresu URL funkce a tajný klíč do polí **Payload URL** (URL datové části) a **Secret** (Tajný klíč) a jako **Content type** (Typ obsahu) vyberte **application/json**.

4. Klikněte na **Let me select individual events** (Vyberu jednotlivé události), vyberte **Issue comment** (Komentář k problému) a klikněte na **Add webhook** (Přidat webhook).
   
    ![Nastavení adresy URL a tajného klíče webhooku](./media/functions-create-a-web-hook-or-api-function/functions-create-new-github-webhook-3.png) 

V tomto okamžiku je webhook GitHubu nakonfigurován tak, aby aktivoval funkci v případě, že dojde k přidání nového komentáře k problému.  
Teď je čas na otestování.

## <a name="test-the-function"></a>Testování funkce
1. Ve vašem úložišti GitHub otevřete kartu **Issues** (Problémy) na nové kartě prohlížeče.

2. V novém okně klikněte na **New Issue** (Nový problém), zadejte název a klikněte na **Submit new issue** (Odeslat nový problém). Můžete také otevřít stávající problém.

2. U problému zadejte komentář a klikněte na tlačítko **Comment** (Komentář). 

3. V druhém okně s GitHubem klikněte na **Edit** (Upravit) vedle vašeho nového webhooku, přejděte dolů k části **Recent Deliveries** (Poslední doručené) a ověřte, že byl odeslán požadavek webhooku a text odpovědi je `New GitHub comment: <Your issue comment text>`.

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




<!--HONumber=Feb17_HO1-->


