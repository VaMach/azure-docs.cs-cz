---
title: "Vytvoření aplikace logiky | Dokumentace Microsoftu"
description: "Podívejte se, jak vytvořit aplikaci logiky propojením služeb SaaS."
author: jeffhollan
manager: dwrede
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: ce3582b5-9c58-4637-9379-75ff99878dcd
ms.service: logic-apps
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 10/18/2016
ms.author: jehollan
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: ab26637d0f49abd73b55e3ccf9ace0f33f6f8731


---
# <a name="create-a-new-logic-app-connecting-saas-services"></a>Vytvoření nové aplikace logiky propojením služeb SaaS
Toto téma popisuje, jak během několika minut začít s [Azure Logic Apps](app-service-logic-what-are-logic-apps.md). Projdeme jednoduchý pracovní postup, který vám umožní odeslat zajímavé tweety na váš e-mail.

Abyste mohli použít tento scénář, potřebujete:

* Předplatné Azure
* Účet na Twitteru
* Outlook.com nebo hostovaná poštovní schránka Office 365

## <a name="create-a-new-logic-app-to-email-you-tweets"></a>Vytvoření nové aplikace logiky k posílání tweetů e-mailem
1. Na [řídicím panelu Azure Portal](https://portal.azure.com) vyberte **Nový**. 
2. V panelu vyhledávání vyhledejte „aplikace logiky“ a vyberte **Aplikace logiky**. Můžete také vybrat **Nový**, **Web + mobilní zařízení** a potom **Aplikace logiky**. 
3. Zadejte název aplikace logiky, vyberte umístění a skupinu prostředků a zvolte **Vytvořit**.  Pokud vyberete **Připnout na řídicí panel**, aplikace logiky se po nasazení automaticky otevře.  
4. Po prvním otevření aplikace logiky můžete vybrat šablonu, se kterou začnete.  Nyní klikněte na **Prázdná aplikace logiky**, abyste začali vytvářet od začátku. 
5. První položka, kterou je potřeba vytvořit, je trigger.  Toto je událost, která spustí vaši aplikaci logiky.  Do vyhledávacího pole triggeru zadejte **twitter** a vyberte jej.
6. Nyní zadáte hledaný termín pro aktivaci.  **Frekvence** a **Interval** určí, jak často bude aplikace logiky kontrolovat nové tweety (a vrátí všechny tweety během tohoto časového intervalu).
    ![Hledání na Twitteru](./media/app-service-logic-create-a-logic-app/twittersearch.png)
7. Vyberte tlačítko **Nový krok** a pak zvolte **Přidat akci** nebo **Přidat podmínku**.
8. Když vyberete **Přidat akci**, můžete vyhledávat z [dostupných konektorů](../connectors/apis-list.md) a vybrat akci. Například můžete vybrat **Outlook.com – odeslání e-mailu**, aby se odeslali e-mail z adresy outlook.com:  
    ![Akce](./media/app-service-logic-create-a-logic-app/actions.png)
9. Nyní je třeba vyplnit požadované parametry pro e-mail:  ![Parametry](./media/app-service-logic-create-a-logic-app/parameters.png)
10. Nakonec můžete vybrat **Uložit** a tím aplikaci logiky uvést do provozu.

## <a name="manage-your-logic-app-after-creation"></a>Správa aplikace logiky po vytvoření
Nyní je vaše aplikace logiky spuštěná a funkční. Bude pravidelně kontrolovat tweety se zadaným hledaným termínem. Pokud se najde odpovídající tweet, pošle vám e-mail. Nakonec uvidíte, jak je možné aplikaci vypnout nebo jak zkontrolovat její funkčnost.

1. Přejděte na web [Azure Portal](https://portal.azure.com).
2. Klikněte na **Procházet** na levé straně obrazovky a vyberte **Logic Apps**.
3. Klikněte na novou aplikaci logiky, kterou jste právě vytvořili: zobrazí se aktuální stav a obecné informace.
4. Pokud chcete svou novou aplikaci logiky upravit, klikněte na **Upravit**.
5. Aplikaci deaktivujete tak, že na panelu příkazů kliknete na **Vypnout**.
6. Zobrazte historie spouštění a triggerů, abyste sledovali, kdy se vaše aplikace logiky spouští.  Kliknutím na **Aktualizovat** můžete zobrazit nejnovější data.

Za méně než 5 minut jste dokázali nastavit jednoduchou aplikaci logiky, která běží v cloudu. Další informace o používání funkcí Logic Apps najdete v tématu o [Používání funkcí aplikací logiky]. Informace o definicích samotných aplikaci logiky najdete v tématu o [vytváření definic Logic Apps](app-service-logic-author-definitions.md).

<!-- Shared links -->
[Azure Portal]: https://portal.azure.com
[Používání funkcí aplikací logiky]: app-service-logic-create-a-logic-app.md



<!--HONumber=Nov16_HO2-->


