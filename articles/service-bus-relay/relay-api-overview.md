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
ms.date: 01/23/2018
ms.author: sethm
ms.openlocfilehash: fc6db8aba887b186961da9b12e7c5f32afa4355b
ms.sourcegitcommit: 9890483687a2b28860ec179f5fd0a292cdf11d22
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
---
# <a name="available-relay-apis"></a>K dispozici předávání přes rozhraní API

## <a name="runtime-apis"></a>Modul runtime rozhraní API

Následující tabulka uvádí všechny klienty aktuálně k dispozici modul runtime předávání.

[Další informace o](#additional-information) část obsahuje informace o stavu jednotlivých modulu runtime knihoven.

| Jazyk nebo platformu | Dostupné funkce | Balíček klienta | Úložiště |
| --- | --- | --- | --- |
| Standardní rozhraní .NET | Hybridní připojení | [Microsoft.Azure.Relay](https://www.nuget.org/packages/Microsoft.Azure.Relay/) | [GitHub](https://github.com/azure/azure-relay-dotnet) |
| .NET Framework | WCF Relay | [WindowsAzure.ServiceBus](https://www.nuget.org/packages/WindowsAzure.ServiceBus/) | neuvedeno |
| Node | Hybridní připojení | [`hyco-ws`](https://www.npmjs.com/package/hyco-ws)<br/>[`hyco-websocket`](https://www.npmjs.com/package/hyco-websocket) | [GitHub](https://github.com/Azure/azure-relay-node) |

### <a name="additional-information"></a>Další informace

#### <a name="net"></a>.NET

Ekosystému .NET má více moduly runtime a proto nejsou více knihovny .NET pro centra událostí. Knihovně .NET Standard můžete spustit pomocí .NET Core nebo rozhraní .NET Framework, když rozhraní .NET Framework – knihovna může spustit pouze v prostředí .NET Framework. Další informace o rozhraní .NET Framework naleznete v tématu [framework verze](/dotnet/articles/standard/frameworks#framework-versions).

## <a name="next-steps"></a>Další postup

Další informace o předávání přes Azure, najdete pomocí těchto odkazů:
* [Co je Azure Relay?](relay-what-is-it.md)
* [Přenos – nejčastější dotazy](relay-faq.md)