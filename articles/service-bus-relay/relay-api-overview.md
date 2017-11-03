---
title: "Přehled služby Azure API předávání | Microsoft Docs"
description: "Přehled dostupných rozhraní API předávání přes Azure"
services: event-hubs
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: fdaa1d2b-bd80-4e75-abb9-0c3d0773af2d
ms.service: service-bus-relay
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/05/2017
ms.author: sethm
ms.openlocfilehash: 60413bd110fd4021b070f94d6338a50e1b321dea
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="available-relay-apis"></a>K dispozici předávání přes rozhraní API

## <a name="runtime-apis"></a>Modul runtime rozhraní API

Následující tabulka uvádí všechny klienty aktuálně k dispozici modul runtime předávání.

[Další informace o](#additional-information) část obsahuje informace o stavu jednotlivých modulu runtime knihoven.

| Jazyk nebo platformu | Dostupné funkce | Balíček klienta | Úložiště |
| --- | --- | --- | --- |
| Standardní rozhraní .NET | Hybridní připojení | [Microsoft.Azure.Relay](https://www.nuget.org/packages/Microsoft.Azure.Relay/) | [GitHub](https://github.com/azure/azure-relay-dotnet) |
| Rozhraní .NET framework | WCF Relay | [WindowsAzure.ServiceBus](https://www.nuget.org/packages/WindowsAzure.ServiceBus/) | Není k dispozici |
| Node | Hybridní připojení | [`hyco-ws`](https://www.npmjs.com/package/hyco-ws)<br/>[`hyco-websocket`](https://www.npmjs.com/package/hyco-websocket) | [GitHub](https://github.com/Azure/azure-relay-node) |

### <a name="additional-information"></a>Další informace

#### <a name="net"></a>.NET
Ekosystému .NET má více moduly runtime a proto nejsou více knihovny .NET pro centra událostí. Knihovně .NET Standard můžete spustit pomocí .NET Core nebo rozhraní .NET Framework, když rozhraní .NET Framework – knihovna může spustit pouze v prostředí .NET Framework. Další informace o rozhraní .NET Framework naleznete v tématu [framework verze](/dotnet/articles/standard/frameworks#framework-versions).

## <a name="next-steps"></a>Další kroky
Další informace o předávání přes Azure, najdete pomocí těchto odkazů:
* [Co je Azure Relay?](relay-what-is-it.md)
* [Přenos – nejčastější dotazy](relay-faq.md)