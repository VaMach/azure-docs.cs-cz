---
title: "Začínáme s předávání hybridních připojení | Dokumentace Microsoftu"
description: "Postup při psaní konzolové aplikace Node v jazyce C# pro hybridní připojení"
services: service-bus
documentationcenter: node
author: jtaubensee
manager: timlt
editor: 
ms.assetid: e44e4867-3cf3-46be-8f8a-7671e2013bc4
ms.service: service-bus
ms.devlang: tbd
ms.topic: hero-article
ms.tgt_pltfrm: node
ms.workload: na
ms.date: 10/28/2016
ms.author: jotaub
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 0f59faed2edb75e32ea62fa7f1d7013da5f387d8


---
# <a name="get-started-with-relay-hybrid-connections"></a>Začínáme s hybridními připojeními pro přenos
[!INCLUDE [relay-selector-hybrid-connections](../../includes/relay-selector-hybrid-connections.md)]

## <a name="what-will-be-accomplished"></a>Co všechno zvládneme
Protože hybridní připojení vyžaduje klienta i server, vytvoříme si v tomto kurzu dvě konzolové aplikace. Postup je následující:

1. Pomocí webu Azure Portal vytvoříme obor názvů přenosu.
2. Pomocí webu Azure Portal vytvoříme hybridní připojení.
3. Napíšeme serverovou aplikaci pro příjem zpráv.
4. Napíšeme aplikaci klientské konzoly pro příjem zpráv.

## <a name="prerequisites"></a>Požadavky
1. [Node.js](https://nodejs.org/en/) (tento příklad používá Node 7.0).
2. Předplatné Azure.

[!INCLUDE [create-account-note](../../includes/create-account-note.md)]

## <a name="1-create-a-namespace-using-the-azure-portal"></a>1. Vytvoření oboru názvů služby Service Bus pomocí webu Azure Portal
Pokud už máte vytvořený obor názvů pro přenos, přejděte do části [Vytvoření hybridního připojení pomocí webu Azure Portal](#2-create-a-hybrid-connection-using-the-azure-portal).

[!INCLUDE [relay-create-namespace-portal](../../includes/relay-create-namespace-portal.md)]

## <a name="2-create-a-hybrid-connection-using-the-azure-portal"></a>2. Vytvoření hybridního připojení pomocí webu Azure Portal
Pokud už máte vytvořené hybridní připojení, přejděte do části [Vytvoření serverové aplikace](#3-create-a-server-application-listener).

[!INCLUDE [relay-create-hybrid-connection-portal](../../includes/relay-create-hybrid-connection-portal.md)]

## <a name="3-create-a-server-application-listener"></a>3. Vytvoření serverové aplikace (naslouchací proces)
Aby bylo možné prostřednictvím přenosu poslouchat a přijímat zprávy, napíšeme konzolovou aplikaci Node.js.

[!INCLUDE [relay-hybrid-connections-dotnet-get-started-server](../../includes/relay-hybrid-connections-node-get-started-server.md)]

## <a name="4-create-a-client-application-sender"></a>4. Vytvoření klientské aplikace (odesílatel)
Aby bylo možné prostřednictvím přenosu odesílat zprávy, napíšeme konzolovou aplikaci Node.js.

[!INCLUDE [relay-hybrid-connections-dotnet-get-started-client](../../includes/relay-hybrid-connections-node-get-started-client.md)]

## <a name="5-run-the-applications"></a>5. Spuštění aplikací
1. Spusťte serverovou aplikaci.
2. Spusťte klientskou aplikaci a napište nějaký text.
3. Ujistěte se, že výstupem konzoly serverové aplikace je text, který jste zadali v klientské aplikaci.

![running-applications](./media/relay-hybrid-connections-node-get-started/running-applications.png)

Blahopřejeme, vytvořili jste kompletní aplikaci Hybridní připojení.

## <a name="next-steps"></a>Další kroky:
* [Přenos – nejčastější dotazy](relay-faq.md)
* [Vytvoření oboru názvů](relay-create-namespace-portal.md)
* [Začínáme s .NET](relay-hybrid-connections-dotnet-get-started.md)
* [Začínáme s aplikací Node](relay-hybrid-connections-node-get-started.md)




<!--HONumber=Nov16_HO2-->


