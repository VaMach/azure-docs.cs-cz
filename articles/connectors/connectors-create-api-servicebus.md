---
title: "Naučte se používat konektor Azure Service Bus ve vašich logic apps | Microsoft Docs"
description: "Vytvoření aplikace logiky službou Azure App service. Připojení k Azure Service Bus odesílat a přijímat zprávy. Můžete provést akce, například odeslání do fronty, odesílat do tématu, přijímání z fronty a přijímat z předplatného."
services: logic-apps
documentationcenter: .net,nodejs,java
author: MandiOhlinger
manager: anneta
editor: 
tags: connectors
ms.assetid: d6d14f5f-2126-4e33-808e-41de08e6721f
ms.service: logic-apps
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 08/02/2016
ms.author: mandia; ladocs
ms.openlocfilehash: 89bf0ffec759fca4af5f99af1b6a2dd8d641ff6f
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/19/2018
---
# <a name="get-started-with-the-azure-service-bus-connector"></a>Začínáme s Azure Service Bus konektoru
Připojení k Azure Service Bus odesílat a přijímat zprávy. Můžete provést akce, například odeslání do fronty, odesílat do tématu, přijímání z fronty a přijímat z předplatného.

Chcete-li použít [všechny konektory](apis-list.md), musíte nejprve vytvořit aplikaci logiky. Abyste mohli začít podle [vytvoření aplikace logiky teď](../logic-apps/quickstart-create-first-logic-app-workflow.md).

## <a name="connect-to-service-bus"></a>Připojení k Service Bus
Než se aplikace logiky k jakékoli služby, musíte nejprve vytvořit připojení ke službě. A [připojení](connectors-overview.md) poskytuje připojení mezi aplikace logiky a jiné služby.  

> [!INCLUDE [Steps to create a connection to Azure Service Bus](../../includes/connectors-create-api-servicebus.md)]
> 
> 

## <a name="use-a-service-bus-trigger"></a>Použít aktivační událost Service Bus
Aktivační událost je událost, která můžete použít ke spuštění pracovního postupu definované v aplikaci logiky. [Další informace o aktivační události](../logic-apps/logic-apps-overview.md#logic-app-concepts).  

> [!INCLUDE [Steps to create a Service Bus trigger](../../includes/connectors-create-api-servicebus-trigger.md)]
> 
> 

## <a name="use-a-service-bus-action"></a>Pomocí akce sběrnice
Akce je operace prováděné definované v aplikaci logiky pracovního postupu. [Další informace o akcích](../logic-apps/logic-apps-overview.md#logic-app-concepts).

[!INCLUDE [Steps to create a Service Bus action](../../includes/connectors-create-api-servicebus-action.md)]

## <a name="connector-specific-details"></a>Podrobnosti o konkrétní konektor

Zobrazit všechny aktivační události a akce definované v swagger a také zobrazit žádné limity v [connector – podrobnosti](/connectors/servicebus/). 

## <a name="next-steps"></a>Další postup
[Vytvoření aplikace logiky](../logic-apps/quickstart-create-first-logic-app-workflow.md).

