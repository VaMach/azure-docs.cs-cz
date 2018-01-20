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
ms.openlocfilehash: c9120babaa5f6da4f33bd60ba27434e24cb2f45e
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/19/2018
---
# <a name="get-started-with-the-github-connector"></a>Začínáme s konektorem Githubu
GitHub je hostitelem služby založené na webu úložiště Git. Nabízí všechny distribuované revize řízení a zdrojový kód (SCM) funkce správy Git, jakož i přidání vlastní funkce.

Můžete začít s vytvářením aplikace logiky teď najdete v tématu [vytvoření aplikace logiky](../logic-apps/quickstart-create-first-logic-app-workflow.md).

## <a name="create-a-connection-to-github"></a>Umožňuje vytvořit připojení ke Githubu
Postup vytvoření aplikace logiky se Githubu, musíte nejdřív vytvořit **připojení** pak zadejte podrobnosti pro následující vlastnosti: 

| Vlastnost | Požaduje se | Popis |
| --- | --- | --- |
| Token |Ano |Zadejte přihlašovací údaje pro GitHub. |

Po vytvoření připojení, můžete ke spouštění akcí a naslouchat aktivační události popsané v tomto článku. 

> [!INCLUDE [Steps to create a connection to GitHub](../../includes/connectors-create-api-github.md)]
> 

## <a name="connector-specific-details"></a>Podrobnosti o konkrétní konektor

Zobrazit všechny aktivační události a akce definované v swagger a také zobrazit žádné limity v [connector – podrobnosti](/connectors/github/).

## <a name="more-connectors"></a>Více konektorů
Přejděte zpět [rozhraní API seznamu](apis-list.md).