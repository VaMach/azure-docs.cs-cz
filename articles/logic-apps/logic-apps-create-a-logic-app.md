---
title: "Vytváření pracovních postupů s vaší první aplikací logiky v Azure | Dokumentace Microsoftu"
description: "Začínáme s propojováním aplikací a služeb SaaS s vaší první aplikací logiky"
author: jeffhollan
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: ce3582b5-9c58-4637-9379-75ff99878dcd
ms.service: logic-apps
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 01/25/2017
ms.author: jehollan
translationtype: Human Translation
ms.sourcegitcommit: 6e0ad6b5bec11c5197dd7bded64168a1b8cc2fdd
ms.openlocfilehash: 221f1b9f0985bbaf0553f6ca01f0f048b9976315
ms.lasthandoff: 03/28/2017


---
# <a name="create-a-new-logic-app-connecting-saas-services"></a>Vytvoření nové aplikace logiky propojením služeb SaaS
Toto téma popisuje, jak během několika minut začít s [Azure Logic Apps](logic-apps-what-are-logic-apps.md). Projdeme jednoduchý pracovní postup, který vám umožní odeslat zajímavé tweety na váš e-mail.

Abyste mohli použít tento scénář, potřebujete:

* Předplatné Azure
* Účet na Twitteru
* Účet Outlook.com nebo Office 365 Outlook

## <a name="create-a-new-logic-app-to-email-you-tweets"></a>Vytvoření nové aplikace logiky k posílání tweetů e-mailem

1. Přihlaste se k webu [Azure Portal](https://portal.azure.com). 

2. V levé nabídce zvolte **Nový** > **Podniková integrace** > **Aplikace logiky**.

    Můžete také zvolit **Nový**, do vyhledávacího pole zadat `logic app` a stisknout klávesu Enter. Zvolte **Aplikace logiky** > **Vytvořit**.

3. Zadejte název pro svou aplikaci logiky, vyberte předplatné Azure, vytvořte nebo vyberte skupinu prostředků Azure, vyberte umístění a zvolte **Vytvořit**.

    Pokud vyberete **Připnout na řídicí panel**, aplikace logiky se po nasazení automaticky otevře.

4. Při prvním otevření aplikace logiky můžete vybrat šablonu, se kterou začnete.
Nyní klikněte na **Prázdná aplikace logiky**, abyste začali vytvářet od začátku. 

5. První položka, kterou je potřeba vytvořit, je trigger. Toto je událost, která spustí vaši aplikaci logiky. Pomocí vyhledávacího pole vyhledejte **twitter** a vyberte **Když se publikuje nový tweet**. Přihlaste se pomocí uživatelského jména a hesla pro váš účet Twitteru.

6. Nyní zadejte hledaný termín pro aktivaci vaší aplikace logiky.

   ![Vyhledávání na Twitteru](media/logic-apps-create-a-logic-app/twittersearch.png)

    **Frekvence** a **Interval** určují, jak často aplikace logiky kontroluje nové tweety (a vrací všechny tweety během tohoto časového intervalu).

7. Vyberte **Nový krok** a zvolte **Přidat akci** nebo **Přidat podmínku**.

    Když vyberete **Přidat akci**, můžete vyhledat [dostupné konektory](../connectors/apis-list.md) a vybrat akci. 

8. Pomocí vyhledávacího pole vyhledejte **outlook** a vyberte **Odeslat e-mail**, aby se odeslal e-mail z vašeho účtu Outlook na jakoukoli zadanou e-mailovou adresu.

   ![Akce](media/logic-apps-create-a-logic-app/actions.png)

9. Nyní je třeba vyplnit požadované parametry pro e-mail:

   ![Parametry](media/logic-apps-create-a-logic-app/parameters.png)

10. Nakonec můžete vybrat **Uložit** a tím aplikaci logiky uvést do provozu.

## <a name="manage-your-logic-app-after-creation"></a>Správa aplikace logiky po vytvoření

Nyní je vaše aplikace logiky spuštěná a funkční. Bude pravidelně kontrolovat tweety se zadaným hledaným termínem. Pokud se najde odpovídající tweet, pošle vám e-mail. Nakonec uvidíte, jak je možné aplikaci vypnout nebo jak zkontrolovat její funkčnost.

1. Přejděte na [portál Azure](https://portal.azure.com).

2. V nabídce vlevo klikněte na **Další služby**. V části **Podniková integrace** zvolte **Logic Apps**. Vyberte svou aplikaci logiky.

    *    Pokud chcete zobrazit stav vaší aplikace, historii provádění a obecné informace, zvolte v nabídce aplikace logiky **Přehled**. Pokud nenajdete data, která očekáváte, na panelu příkazů zvolte **Aktualizovat**.

    *    Pokud chcete aplikaci upravit, v nabídce aplikace logiky zvolte **Návrhář aplikací logiky**.

    *    Pokud chcete aplikaci dočasně vypnout, v nabídce aplikace logiky zvolte **Přehled**. Na panelu příkazů zvolte **Zakázat**.

    *    Pokud chcete aplikaci odstranit, v nabídce aplikace logiky zvolte **Přehled**. 
    Na panelu příkazů zvolte **Odstranit**. Zadejte název aplikace logiky a zvolte **Odstranit**.

Za méně než 5 minut jste dokázali nastavit jednoduchou aplikaci logiky, která běží v cloudu. Další informace o používání funkcí Logic Apps najdete v tématu o [Používání funkcí aplikací logiky]. Informace o definicích samotných aplikaci logiky najdete v tématu o [vytváření definic Logic Apps](../logic-apps/logic-apps-author-definitions.md).

<!-- Shared links -->
[Azure portal]: https://portal.azure.com
[Používání funkcí aplikací logiky]: logic-apps-create-a-logic-app.md
