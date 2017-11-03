---
title: Konektor Githubu v Azure Logic Apps | Microsoft Docs
description: "Vytvoření aplikace logiky službou Azure App service. GitHub je hostitelem služby založené na webu úložiště Git. Nabízí všechny distribuované revize řízení a zdrojový kód (SCM) funkce správy Git, jakož i přidání vlastní funkce."
services: logic-apps
documentationcenter: .net,nodejs,java
author: MandiOhlinger
manager: anneta
editor: 
tags: connectors
ms.assetid: 8f873e6c-f4c0-4c2e-a5bd-2e953efe5e2b
ms.service: logic-apps
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 08/18/2016
ms.author: mandia; ladocs
ms.openlocfilehash: d4614b0ceff0ec0d36dbb1a136551f985f2fc1a1
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="get-started-with-the-github-connector"></a>Začínáme s konektorem Githubu
GitHub je hostitelem služby založené na webu úložiště Git. Nabízí všechny distribuované revize řízení a zdrojový kód (SCM) funkce správy Git, jakož i přidání vlastní funkce.

Můžete začít s vytvářením aplikace logiky teď najdete v tématu [vytvoření aplikace logiky](../logic-apps/logic-apps-create-a-logic-app.md).

## <a name="create-a-connection-to-github"></a>Umožňuje vytvořit připojení ke Githubu
Postup vytvoření aplikace logiky se Githubu, musíte nejdřív vytvořit **připojení** pak zadejte podrobnosti pro následující vlastnosti: 

| Vlastnost | Požaduje se | Popis |
| --- | --- | --- |
| Token |Ano |Zadejte přihlašovací údaje Githubu |

Po vytvoření připojení, můžete ke spouštění akcí a naslouchat aktivační události popsané v tomto článku. 

> [!INCLUDE [Steps to create a connection to GitHub](../../includes/connectors-create-api-github.md)]
> 

## <a name="connector-specific-details"></a>Podrobnosti o konkrétní konektor

Zobrazit všechny aktivační události a akce definované v swagger a také zobrazit žádné limity v [connector – podrobnosti](/connectors/github/).

## <a name="more-connectors"></a>Více konektorů
Přejděte zpět [rozhraní API seznamu](apis-list.md).