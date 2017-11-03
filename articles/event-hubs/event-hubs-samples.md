---
title: "Ukázek Azure Event Hubs | Microsoft Docs"
description: "Ukázek Azure Event Hubs"
services: event-hubs
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/15/2017
ms.author: sethm
ms.openlocfilehash: ae9fbd97a1747d8f14c561f247a0973bb11fd039
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="event-hubs-samples"></a>Ukázky centra událostí 

Sada ukázek Azure Event Hubs ukazuje klíčové funkce v [Azure Event Hubs](/azure/event-hubs/). Tento článek rozděluje a popisuje, k dispozici, s odkazy na všechny ukázky.

V době psaní tohoto textu Event Hubs ukázky umístěny v několika různých místech:

- [Ukázky kódu vývojáře MSDN](https://code.msdn.microsoft.com/site/search?query=event%20hubs&f%5B0%5D.Value=event%20hubs&f%5B0%5D.Type=SearchText&ac=5)
- [GitHub](https://github.com/Azure/azure-event-hubs/tree/master/samples)

Další informace o různých verzích rozhraní .NET Framework najdete v tématu [architektury a cíle](/dotnet/articles/standard/frameworks).

Další ukázky bude přidán v čase, tak zkontrolujte, vraťte se sem často aktualizací.

## <a name="net-standard"></a>Standardní rozhraní .NET

Následující ukázky ukazují, jak odesílat a přijímat události pomocí [klienta služby Event Hubs](https://github.com/Azure/azure-event-hubs-dotnet/blob/master/readme.md) pro [.NET standardní knihovna](/dotnet/articles/standard/library).

### <a name="send-events"></a>Odesílání událostí 

[Začínáme odesílání](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/SampleSender) příklad ukazuje, jak psát aplikace konzoly .NET Core, která zasílá události do centra událostí.

### <a name="receive-events"></a>Příjem událostí 

[Začít pracovat s Event Processor Host přijetí](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/SampleEphReceiver) ukázka je aplikace konzoly .NET Core, která přijímá zprávy z centra událostí pomocí Event Processor Host.

## <a name="net-framework"></a>Rozhraní .NET framework   

Tyto ukázky demonstrují různým funkcím služby Azure Event Hubs, cílení [rozhraní .NET Framework – knihovna](/dotnet/framework/index).
 
### <a name="notify-users-of-events-received"></a>Upozorněte uživatele událostí přijatých

[AppToNotifyUsers](https://github.com/Azure-Samples/event-hubs-dotnet-user-notifications) ukázka upozorní uživatele z dat přijatých ze senzorů nebo jinými systémy.

### <a name="get-started-with-event-hubs"></a>Začínáme se službou Event Hubs 

[Event Hubs Začínáme](https://code.msdn.microsoft.com/Service-Bus-Event-Hub-286fd097) příklad ukazuje základní možnosti služby Event Hubs, například vytvoření centra událostí, odesílání událostí do centra událostí a využívat událostí pomocí [Event Processor Host](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost/) .

### <a name="scale-out-event-processing"></a>Horizontální navýšení kapacity zpracování událostí 

[Horizontální navýšení kapacity zpracování událostí](https://code.msdn.microsoft.com/Service-Bus-Event-Hub-45f43fc3) příklad ukazuje způsob použití [Event Processor Host](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost/) distribuovat zatížení spotřeby Event Hubs datového proudu. Ukazuje, jak implementovat **EventProcessor** a **EventProcessorFactory** objektů ke správě datového proudu událostí. 

###  <a name="pull-data-from-sql-into-an-event-hub"></a>Získání dat z SQL do centra událostí

[Dat stahování SQL](https://github.com/Azure-Samples/event-hubs-dotnet-import-from-sql) příklad ukazuje postup vyžádá data z tabulky SQL a poslat ho do centra událostí, použít jako vstup v příjem dat analytických aplikací.

### <a name="pull-web-data-into-an-event-hub"></a>Získání dat webové do centra událostí 

[Importovat data z webu](https://github.com/Azure-Samples/event-hubs-dotnet-importfromweb) příklad ukazuje, jak načítat data z veřejné informační kanály (například ministerstva dopravy na provoz informace informační kanál) a poslat ho do centra událostí.

## <a name="next-steps"></a>Další kroky

Další informace o rozhraní .NET Framework verze když přejdete na následujících odkazech:

- [Rozhraní a cíle](/dotnet/articles/standard/frameworks)
- [Rozhraní .NET framework 4.6 a 4.5](/dotnet/framework/index)

Se více o službě Event Hubs v těchto článcích:

- [Přehled služby Event Hubs](event-hubs-what-is-event-hubs.md)
- [Vytvoření centra událostí](event-hubs-create.md)
- [Nejčastější dotazy k Event Hubs](event-hubs-faq.md)