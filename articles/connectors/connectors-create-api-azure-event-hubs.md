---
title: "Nastavení monitorování událostí s Azure Event Hubs pro Azure Logic Apps | Microsoft Docs"
description: "Monitorování datových proudů pro příjem událostí a odesílání událostí pro Azure Logic Apps s Azure Event Hubs"
services: logic-apps
keywords: "datový proud, monitorování událostí, služba event hubs"
author: ecfan
manager: anneta
editor: 
documentationcenter: 
tags: connectors
ms.assetid: 
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/31/2017
ms.author: estfan; LADocs
ms.openlocfilehash: a7f31c2c17d326d58ede0bb00cdc0f701069ea14
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/19/2018
---
# <a name="monitor-receive-and-send-events-with-the-event-hubs-connector"></a>Monitorování, příjem a odesílání událostí s konektorem služby Event Hubs

Chcete-li nastavit monitorování událostí tak, aby aplikace logiky můžete zjišťovat události, přijímat události a odesílat události, připojte se ke [centra událostí Azure](https://azure.microsoft.com/services/event-hubs) z aplikace logiky. Další informace o [Azure Event Hubs](../event-hubs/event-hubs-what-is-event-hubs.md).

## <a name="requirements"></a>Požadavky

* Musíte mít [Event Hubs obor názvů a centra událostí](../event-hubs/event-hubs-create.md) v Azure. Další informace [postup vytvoření oboru názvů služby Event Hubs a Centrum událostí](../event-hubs/event-hubs-create.md). 

* Chcete-li použít [všechny konektory](https://docs.microsoft.com/azure/connectors/apis-list) ve vaší aplikaci logiky, musíte nejprve vytvořte aplikaci logiky. Další informace [postup vytvoření aplikace logiky](../logic-apps/quickstart-create-first-logic-app-workflow.md).

<a name="permissions-connection-string"></a>
## <a name="check-event-hubs-namespace-permissions-and-find-the-connection-string"></a>Zkontrolujte oprávnění k oboru názvů služby Event Hubs a najděte připojovací řetězec

Aplikace logiky pro přístup k žádné službě, je nutné vytvořit [ *připojení* ](./connectors-overview.md) mezi svou aplikaci logiky a služby, pokud jste tak ještě neučinili. Toto připojení autorizuje aplikace logiky pro přístup k datům.
Aplikace logiky pro přístup k vašeho centra událostí, musíte mít **spravovat** oprávnění a připojovací řetězec pro obor názvů služby Event Hubs.

Zkontrolujte svá oprávnění a získat připojovací řetězec, postupujte podle těchto kroků.

1.  Přihlaste se na web [Azure Portal](https://portal.azure.com "Azure Portal"). 

2.  Přejděte do vašeho centra událostí *obor názvů*, nikoli konkrétní Centrum událostí. V okně oboru názvů v části **nastavení**, zvolte **zásady sdíleného přístupu**. V části **deklarace identity**, zkontrolujte, zda máte **spravovat** oprávnění pro tento obor názvů.

    ![Správa oprávnění pro obor názvů centra událostí](./media/connectors-create-api-azure-event-hubs/event-hubs-namespace.png)

3.  Chcete-li zkopírujte připojovací řetězec pro obor názvů služby Event Hubs, zvolte **RootManageSharedAccessKey**. Vedle primární klíče připojovacího řetězce vyberte tlačítko Kopírovat.

    ![Zkopírujte Event Hubs obor názvů připojovacího řetězce](media/connectors-create-api-azure-event-hubs/find-event-hub-namespace-connection-string.png)

    > [!TIP]
    > Chcete-li ověřit, zda je připojovací řetězec přidružené vašeho oboru názvů služby Event Hubs nebo s konkrétní Centrum událostí, zkontrolujte připojovací řetězec pro `EntityPath` parametr. Pokud zjistíte, tento parametr, připojovací řetězec je pro konkrétní Centrum událostí "entita" a není správný řetězec, který má používat s svou aplikaci logiky.

4.  Nyní když se zobrazí výzva k zadání pověření po přidání Event Hubs aktivační události nebo akce do aplikace logiky, můžete připojit k oboru názvů služby Event Hubs. Zadejte název připojení, zadejte připojovací řetězec, který jste zkopírovali a zvolte **vytvořit**.

    ![Zadejte připojovací řetězec pro obor názvů služby Event Hubs](./media/connectors-create-api-azure-event-hubs/event-hubs-connection.png)

    Po vytvoření připojení, název připojení by se zobrazit události rozbočovače aktivační události nebo akce. 
    Pak můžete pokračovat v další kroky v aplikaci logiky.

    ![Připojení obor názvů centra událostí vytvořit](./media/connectors-create-api-azure-event-hubs/event-hubs-connection-created.png)

## <a name="start-workflow-when-your-event-hub-receives-new-events"></a>Spustit pracovní postup v případě, že Centrum událostí obdrží nové události

A [ *aktivační událost* ](../logic-apps/logic-apps-overview.md#logic-app-concepts) je událost, která se spouští v aplikaci logiky pracovního postupu. Spuštění pracovního postupu odeslání nové události do vašeho centra událostí, postupujte podle těchto kroků pro přidání aktivační událost, která zjistí tuto událost.

1.  V [portál Azure](https://portal.azure.com "portál Azure"), přejděte do existující aplikace logiky nebo vytvoření aplikace logiky prázdné.

2.  Do vyhledávacího pole pro návrhář aplikace logiky, zadejte `event hubs` filtru. Vyberte této aktivační události: **když jsou události dostupné v Centru událostí**

    ![Vyberte aktivační událost pro Jakmile Centrum událostí obdrží nové události](./media/connectors-create-api-azure-event-hubs/find-event-hubs-trigger.png)

    Pokud již nemáte připojení k oboru názvů služby Event Hubs, se zobrazí výzva k vytvoření nyní toto připojení. Zadejte název připojení a zadat připojovací řetězec pro obor názvů služby Event Hubs. 
    V případě potřeby další [jak najít připojovací řetězec](#permissions-connection-string).

    ![Zadejte připojovací řetězec pro obor názvů služby Event Hubs](./media/connectors-create-api-azure-event-hubs/event-hubs-connection.png)

    Po vytvoření připojení, nastavení **při událost v dostupné v Centru událostí** zobrazí aktivační události.

    ![Aktivační událost nastavení po Centrum událostí obdrží nové události](./media/connectors-create-api-azure-event-hubs/event-hubs-trigger.png)

3.  Zadejte nebo vyberte název centra událostí, které chcete monitorovat. Vyberte četnost a interval jak často chcete zkontrolovat centra událostí.

    > [!TIP]
    > Chcete-li můžete volitelně vybrat skupinu uživatelů pro čtení událostí, zvolte **zobrazit rozšířené možnosti**. 

    ![Zadejte centra událostí nebo skupiny uživatelů](./media/connectors-create-api-azure-event-hubs/event-hubs-trigger-details.png)

    Jste nyní nastavili aktivační událost spuštění pracovního postupu pro svou aplikaci logiky. 
    Aplikace logiky ověří zadaný centra událostí podle plánu, který nastavíte. 
    Pokud vaše aplikace vyhledá nové události události rozbočovače, aktivační událost spustí další akce nebo aktivuje v aplikaci logiky.

## <a name="send-events-to-your-event-hub-from-your-logic-app"></a>Odesílání událostí do vašeho centra událostí z aplikace logiky

[*Akce*](../logic-apps/logic-apps-overview.md#logic-app-concepts) je úloha, kterou provádí pracovní postup vaší aplikace logiky. Když do aplikace logiky přidáte trigger, můžete přidat akci, která bude provádět určitou operaci s daty generovanými triggerem. Pro odeslání události do vašeho centra událostí z aplikace logiky, postupujte podle těchto kroků.

1.  V návrháři aplikace logiky, vyberte v části vaší aktivační událostí aplikace logiky **nový krok** > **přidat akci**.

    ![Pak zvolte "Nový krok", "Přidat akci."](./media/connectors-create-api-azure-event-hubs/add-action.png)

    Nyní můžete najít a vyberte akci, kterou chcete provést. 
    I když můžete vybrat všechny akce, například chceme akce Event Hubs odesílat události.

2.  Do vyhledávacího pole zadejte `event hubs` filtru.
Vyberte tuto akci: **odesílání událostí**

    ![Vyberte "Event Hubs - odesílání událostí" akce](./media/connectors-create-api-azure-event-hubs/find-event-hubs-action.png)

3.  Zadejte požadované podrobnosti události, jako je například název centra událostí, ve které chcete odeslat událost. Zadejte další volitelné podrobnosti o události, jako je třeba obsah pro tuto událost.

    > [!TIP]
    > Volitelně zadejte oddílu centra událostí, kam chcete odeslat událost, zvolte **zobrazit rozšířené možnosti**. 

    ![Zadejte název centra událostí a podrobnosti volitelné události](./media/connectors-create-api-azure-event-hubs/event-hubs-send-event-action.png)

6.  Uložte provedené změny.

    ![Uložení aplikace logiky](./media/connectors-create-api-azure-event-hubs/save-logic-app.png)

    Jste nyní nastavili akce pro odeslání událostí ze svou aplikaci logiky. 

## <a name="connector-specific-details"></a>Podrobnosti o konkrétní konektor

Zobrazit všechny aktivační události a akce definované v swagger a také zobrazit žádné limity v [connector – podrobnosti](/connectors/eventhubs/). 

## <a name="get-help"></a>Podpora

Klást otázky, odpovídat na ně a poučit se ze zkušeností jiných uživatelů Azure Logic Apps můžete ve [fóru Azure Logic Apps](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).

Pokud chcete pomoci při vylepšování Logic Apps a konektorů, hlasujte nebo zanechte své nápady na [webu zpětné vazby uživatelů Logic Apps](http://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Další postup

*  [Najít další konektory pro Azure Logic apps](./apis-list.md)