---
title: "Začínáme s hybridními připojeními Azure Relay v .NET | Dokumentace Microsoftu"
description: "Postup při psaní konzolové aplikace v jazyce C# pro hybridní připojení"
services: service-bus-relay
documentationcenter: .net
author: sethmanheim
manager: timlt
editor: 
ms.assetid: d1386900-b942-4abf-acfc-38d2ef826253
ms.service: service-bus-relay
ms.devlang: tbd
ms.topic: get-started-article
ms.tgt_pltfrm: dotnet
ms.workload: na
ms.date: 03/27/2017
ms.author: sethm
ms.translationtype: Human Translation
ms.sourcegitcommit: f92909e0098a543f99baf3df3197a799bc9f1edc
ms.openlocfilehash: d27016559ede5d810d7efcec2a3abc78334f0f0a
ms.contentlocale: cs-cz
ms.lasthandoff: 03/01/2017


---

# <a name="get-started-with-relay-hybrid-connections"></a>Začínáme s hybridními připojeními pro přenos
[!INCLUDE [relay-selector-hybrid-connections](../../includes/relay-selector-hybrid-connections.md)]

Tento kurz obsahuje úvod do [hybridních připojení Azure Relay](relay-what-is-it.md#hybrid-connections) a ukazuje, jak vytvořit klientskou aplikaci, která odesílá zprávy do příslušné aplikace naslouchacího procesu. 

## <a name="what-will-be-accomplished"></a>Co všechno zvládneme
Protože hybridní připojení vyžadují komponentu klienta i serveru, tento kurz vytvoří dvě konzolové aplikace. Kroky jsou následující:

1. Pomocí webu Azure Portal vytvoříme obor názvů přenosu.
2. Pomocí webu Azure Portal vytvoříme hybridní připojení.
3. Napíšeme konzolovou aplikaci serveru (naslouchacího procesu) pro příjem zpráv.
4. Napíšeme konzolovou aplikaci klienta (odesílatele) pro odesílání zpráv.

## <a name="prerequisites"></a>Požadavky
1. [Visual Studio 2015 nebo vyšší](http://www.visualstudio.com). V příklady v tomto kurzu se používá Visual Studio 2015.
2. Předplatné Azure.

[!INCLUDE [create-account-note](../../includes/create-account-note.md)]

## <a name="1-create-a-namespace-using-the-azure-portal"></a>1. Vytvoření oboru názvů služby Service Bus pomocí webu Azure Portal
Pokud už máte vytvořený obor názvů pro přenos, přejděte do části [Vytvoření hybridního připojení pomocí webu Azure Portal](#2-create-a-hybrid-connection-using-the-azure-portal).

[!INCLUDE [relay-create-namespace-portal](../../includes/relay-create-namespace-portal.md)]

## <a name="2-create-a-hybrid-connection-using-the-azure-portal"></a>2. Vytvoření hybridního připojení pomocí webu Azure Portal
Pokud už máte vytvořené hybridní připojení, přejděte do části [Vytvoření serverové aplikace](#3-create-a-server-application-listener).

[!INCLUDE [relay-create-hybrid-connection-portal](../../includes/relay-create-hybrid-connection-portal.md)]

## <a name="3-create-a-server-application-listener"></a>3. Vytvoření serverové aplikace (naslouchací proces)
Aby bylo možné prostřednictvím přenosu poslouchat a přijímat zprávy, napíšeme v sadě Visual Studio konzolovou aplikaci v jazyce C#.

[!INCLUDE [relay-hybrid-connections-dotnet-get-started-server](../../includes/relay-hybrid-connections-dotnet-get-started-server.md)]

## <a name="4-create-a-client-application-sender"></a>4. Vytvoření klientské aplikace (odesílatel)
Abychom mohli do fronty odesílat zprávy, napíšeme v sadě Visual Studio konzolovou aplikaci v jazyce C#.

[!INCLUDE [relay-hybrid-connections-dotnet-get-started-client](../../includes/relay-hybrid-connections-dotnet-get-started-client.md)]

## <a name="5-run-the-applications"></a>5. Spuštění aplikací
1. Spusťte serverovou aplikaci.
2. Spusťte klientskou aplikaci a napište nějaký text.
3. Ujistěte se, že výstupem konzoly serverové aplikace je text, který jste zadali v klientské aplikaci.

![running-applications](./media/relay-hybrid-connections-dotnet-get-started/running-applications.png)

Blahopřejeme, vytvořili jste kompletní aplikaci Hybridní připojení.

## <a name="next-steps"></a>Další kroky:
* [Přenos – nejčastější dotazy](relay-faq.md)
* [Vytvoření oboru názvů](relay-create-namespace-portal.md)
* [Začínáme s aplikací Node](relay-hybrid-connections-node-get-started.md)


