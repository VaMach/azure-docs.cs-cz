---
title: "Pomocí konektoru serveru SharePoint Server ve vašich Logic Apps | Microsoft Docs"
description: "Začněte používat konektor Server služby SharePoint ve vašich Logic apps"
services: logic-apps
documentationcenter: 
author: MandiOhlinger
manager: anneta
editor: 
tags: connectors
ms.assetid: 0238a060-d592-4719-b7a2-26064c437a1a
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/18/2016
ms.author: mandia; ladocs
ms.openlocfilehash: 0f3274816e279a1aa57febaa2f8294914900799a
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="get-started-with-the-sharepoint-connector"></a>Začínáme s konektorem služby SharePoint
Konektor služby SharePoint poskytuje způsob práce se seznamy na webu služby SharePoint.

Začněte vytvořením aplikace logiky; v tématu [vytvoření aplikace logiky](../logic-apps/logic-apps-create-a-logic-app.md).

## <a name="create-a-connection-to-sharepoint"></a>Umožňuje vytvořit připojení do služby SharePoint
K používání konektoru služby SharePoint, je třeba nejprve vytvořit **připojení** pak zadejte podrobnosti pro tyto vlastnosti: 

| Vlastnost | Požaduje se | Popis |
| --- | --- | --- |
| Token |Ano |Zadejte pověření serveru SharePoint |

Pro připojení k **SharePoint**, zadejte svoji identitu (uživatelského jména a hesla, čipové karty přihlašovací údaje, atd.) do služby SharePoint. Jakmile jste jste ověřena, můžete přejít k používání konektoru služby SharePoint v aplikaci logiky. 

Při na návrháře aplikace logiky, postupujte podle těchto kroků pro přihlášení do služby SharePoint k vytvoření připojení **připojení** pro použití v aplikaci logiky:

1. Zadejte do pole vyhledávání služby SharePoint a počkat na výsledky vyhledávání na vrátí všechny položky se službou SharePoint v názvu:   
   ![Konfigurace služby SharePoint][1]  
2. Vyberte **SharePoint – když dojde k vytvoření souboru**   
3. Vyberte **přihlásit do služby SharePoint**:   
   ![Konfigurace služby SharePoint][2]    
4. Zadejte svoje přihlašovací údaje služby SharePoint pro přihlášení k ověření pomocí služby SharePoint   
   ![Konfigurace služby SharePoint][3]     
5. Po dokončení ověření budete přesměrováni na svou aplikaci logiky dokončit konfigurací služby SharePoint na **vytvoření souboru** dialogové okno.          
   ![Konfigurace služby SharePoint][4]  
6. Poté můžete přidat další aktivační události a akce, které potřebujete k dokončení svou aplikaci logiky.   
7. Uložte si práci, výběrem **Uložit** na panelu nabídek nahoře.  

## <a name="connector-specific-details"></a>Podrobnosti o konkrétní konektor

Zobrazit všechny aktivační události a akce definované v swagger a také zobrazit žádné limity v [connector – podrobnosti](/connectors/sharepoint/).

## <a name="more-connectors"></a>Více konektorů
Přejděte zpět [rozhraní API seznamu](apis-list.md).

[1]: ../../includes/media/connectors-create-api-sharepointonline/connectionconfig1.png  
[2]: ../../includes/media/connectors-create-api-sharepointonline/connectionconfig2.png 
[3]: ../../includes/media/connectors-create-api-sharepointonline/connectionconfig3.png
[4]: ../../includes/media/connectors-create-api-sharepointonline/connectionconfig4.png
[5]: ../../includes/media/connectors-create-api-sharepointonline/connectionconfig5.png
