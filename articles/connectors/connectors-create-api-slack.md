---
title: "Pomocí konektoru systému Slack v Azure logic apps | Microsoft Docs"
description: "Připojení k systému Slack ve vašich logic apps"
services: logic-apps
documentationcenter: 
author: MandiOhlinger
manager: anneta
editor: 
tags: connectors
ms.assetid: 234cad64-b13d-4494-ae78-18b17119ba24
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/18/2016
ms.author: mandia; ladocs
ms.openlocfilehash: fc5fc128efe01bd0727e3ff30d8938918e89ac3a
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="get-started-with-the-slack-connector"></a>Začínáme s Slack konektoru
Slack je nástroj komunikace týmu, který spojuje všechny team komunikace v jednom umístění, okamžitě vyhledávat a bez ohledu na přejdete k dispozici. 

Začněte vytvořením aplikace logiky nyní; v tématu [vytvoření aplikace logiky](../logic-apps/logic-apps-create-a-logic-app.md).

## <a name="create-a-connection-to-slack"></a>Umožňuje vytvořit připojení k systému Slack
K používání konektoru Slack, je třeba nejprve vytvořit **připojení** pak zadejte podrobnosti pro tyto vlastnosti: 

| Vlastnost | Požaduje se | Popis |
| --- | --- | --- |
| Token |Ano |Zadejte přihlašovací údaje Slack |

Postupujte podle těchto kroků pro přihlášení do systému Slack a dokončete konfiguraci rezervy **připojení** v aplikaci logiky:

1. Vyberte **opakování**
2. Vyberte **frekvence** a zadejte **intervalu**
3. Vyberte **přidat akci**  
   ![Konfigurace systému Slack][1]  
4. Do vyhledávacího pole zadejte Slack a počkejte, než pro vyhledávání, která vrátí všechny záznamy s Slack v názvu
5. Vyberte **Slack - Post zpráv**
6. Vyberte **přihlásit k systému Slack**:  
   ![Konfigurace systému Slack][2]
7. Zadejte svoje Slack přihlašovací údaje pro přihlášení k aplikaci autorizovat    
   ![Konfigurace systému Slack][3]  
8. Budete přesměrováni na přihlašovací stránku vaší organizace. **Autorizovat** Slack k interakci se svou aplikaci logiky:      
   ![Konfigurace systému Slack][5] 
9. Po dokončení registrace budete přesměrováni na svou aplikaci logiky dokončit, protože se tím, že nakonfigurujete **Slack – získat všechny zprávy** části. Přidejte další aktivační události a akce, které potřebujete.  
   ![Konfigurace systému Slack][6]
10. Uložte si práci, výběrem **Uložit** na panelu nabídek nahoře.

## <a name="connector-specific-details"></a>Podrobnosti o konkrétní konektor

Zobrazit všechny aktivační události a akce definované v swagger a také zobrazit žádné limity v [connector – podrobnosti](/connectors/slack/).

## <a name="more-connectors"></a>Více konektorů
Přejděte zpět [rozhraní API seznamu](apis-list.md).

[1]: ./media/connectors-create-api-slack/connectionconfig1.png
[2]: ./media/connectors-create-api-slack/connectionconfig2.png 
[3]: ./media/connectors-create-api-slack/connectionconfig3.png
[4]: ./media/connectors-create-api-slack/connectionconfig4.png
[5]: ./media/connectors-create-api-slack/connectionconfig5.png
[6]: ./media/connectors-create-api-slack/connectionconfig6.png
