---
title: "Začínáme s hybridními připojeními Azure Relay v Node | Dokumentace Microsoftu"
description: "Postup při psaní konzolové aplikace Node v jazyce C# pro hybridní připojení"
services: service-bus-relay
documentationcenter: node
author: sethmanheim
manager: timlt
editor: 
ms.assetid: e44e4867-3cf3-46be-8f8a-7671e2013bc4
ms.service: service-bus-relay
ms.devlang: tbd
ms.topic: get-started-article
ms.tgt_pltfrm: node
ms.workload: na
ms.date: 05/22/2017
ms.author: sethm
ms.translationtype: Human Translation
ms.sourcegitcommit: 532ff423ff53567b6ce40c0ea7ec09a689cee1e7
ms.openlocfilehash: d8f3f6fbe256b34b812369dc1f7492ed4f15d3d3
ms.contentlocale: cs-cz
ms.lasthandoff: 06/05/2017


---
<a id="get-started-with-relay-hybrid-connections" class="xliff"></a>

# Začínáme s hybridními připojeními pro přenos

[!INCLUDE [relay-selector-hybrid-connections](../../includes/relay-selector-hybrid-connections.md)]

<a id="what-will-be-accomplished" class="xliff"></a>

## Co všechno zvládneme

Protože hybridní připojení vyžaduje klienta i server, vytvoříme si v tomto kurzu dvě konzolové aplikace. Postup je následující:

1. Pomocí webu Azure Portal vytvoříme obor názvů přenosu.
2. Pomocí webu Azure Portal vytvoříme hybridní připojení.
3. Napíšeme serverovou aplikaci pro příjem zpráv.
4. Napíšeme aplikaci klientské konzoly pro příjem zpráv.

<a id="prerequisites" class="xliff"></a>

## Požadavky

1. [Node.js](https://nodejs.org/en/) (tento příklad používá Node 7.0).
2. Předplatné Azure.

[!INCLUDE [create-account-note](../../includes/create-account-note.md)]

<a id="1-create-a-namespace-using-the-azure-portal" class="xliff"></a>

## 1. Vytvoření oboru názvů služby Service Bus pomocí webu Azure Portal

Pokud už máte vytvořený obor názvů pro přenos, přejděte do části [Vytvoření hybridního připojení pomocí webu Azure Portal](#2-create-a-hybrid-connection-using-the-azure-portal).

[!INCLUDE [relay-create-namespace-portal](../../includes/relay-create-namespace-portal.md)]

<a id="2-create-a-hybrid-connection-using-the-azure-portal" class="xliff"></a>

## 2. Vytvoření hybridního připojení pomocí webu Azure Portal

Pokud už máte vytvořené hybridní připojení, přejděte do části [Vytvoření serverové aplikace](#3-create-a-server-application-listener).

[!INCLUDE [relay-create-hybrid-connection-portal](../../includes/relay-create-hybrid-connection-portal.md)]

<a id="3-create-a-server-application-listener" class="xliff"></a>

## 3. Vytvoření serverové aplikace (naslouchací proces)

Aby bylo možné prostřednictvím přenosu poslouchat a přijímat zprávy, napíšeme konzolovou aplikaci Node.js.

[!INCLUDE [relay-hybrid-connections-dotnet-get-started-server](../../includes/relay-hybrid-connections-node-get-started-server.md)]

<a id="4-create-a-client-application-sender" class="xliff"></a>

## 4. Vytvoření klientské aplikace (odesílatel)

Aby bylo možné prostřednictvím přenosu odesílat zprávy, napíšeme konzolovou aplikaci Node.js.

[!INCLUDE [relay-hybrid-connections-dotnet-get-started-client](../../includes/relay-hybrid-connections-node-get-started-client.md)]

<a id="5-run-the-applications" class="xliff"></a>

## 5. Spuštění aplikací

1. Spusťte serverovou aplikaci.
2. Spusťte klientskou aplikaci a napište nějaký text.
3. Ujistěte se, že výstupem konzoly serverové aplikace je text, který jste zadali v klientské aplikaci.

![running-applications](./media/relay-hybrid-connections-node-get-started/running-applications.png)

Blahopřejeme, vytvořili jste kompletní aplikaci Hybrid Connections.

<a id="next-steps" class="xliff"></a>

## Další kroky:

* [Přenos – nejčastější dotazy](relay-faq.md)
* [Vytvoření oboru názvů](relay-create-namespace-portal.md)
* [Začínáme s .NET](relay-hybrid-connections-dotnet-get-started.md)
* [Začínáme s aplikací Node](relay-hybrid-connections-node-get-started.md)


