---
title: "Začínáme s předávání hybridních připojení | Dokumentace Microsoftu"
description: "Postup při psaní konzolové aplikace v jazyce C# pro hybridní připojení"
services: service-bus-relay
documentationcenter: .net
author: jtaubensee
manager: timlt
editor: 
ms.assetid: d1386900-b942-4abf-acfc-38d2ef826253
ms.service: service-bus-relay
ms.devlang: tbd
ms.topic: get-started-article
ms.tgt_pltfrm: dotnet
ms.workload: na
ms.date: 10/28/2016
ms.author: jotaub,sethm
translationtype: Human Translation
ms.sourcegitcommit: 849d7995e9c74bc929c4f791ae9155ca18ddb77b
ms.openlocfilehash: 3ab92ba4e5c19d4b713ceb2da30835542f343d68


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
1. [Visual Studio 2013 nebo Visual Studio 2015](http://www.visualstudio.com). V příklady v tomto kurzu se používá Visual Studio 2015.
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




<!--HONumber=Nov16_HO3-->


