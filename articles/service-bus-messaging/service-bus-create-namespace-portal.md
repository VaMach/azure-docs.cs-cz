---
title: "Vytvoření oboru názvů služby Service Bus na webu Azure Portal | Dokumentace Microsoftu"
description: "Postup vytvoření oboru názvů služby Service Bus pomocí webu Azure Portal"
services: service-bus-messaging
documentationcenter: .net
author: sethmanheim
manager: timlt
editor: 
ms.assetid: fbb10e62-b133-4851-9d27-40bd844db3ba
ms.service: service-bus-messaging
ms.devlang: tbd
ms.topic: get-started-article
ms.tgt_pltfrm: dotnet
ms.workload: na
ms.date: 03/23/2017
ms.author: sethm
ms.translationtype: Human Translation
ms.sourcegitcommit: 0bec803e4b49f3ae53f2cc3be6b9cb2d256fe5ea
ms.openlocfilehash: 5de92033eb7eb4fef8d27a215b3284ab80594065
ms.contentlocale: cs-cz
ms.lasthandoff: 03/24/2017


---
# <a name="create-a-service-bus-namespace-using-the-azure-portal"></a>Vytvoření oboru názvů služby Service Bus pomocí webu Azure Portal
Obor názvů je společný kontejner pro všechny součásti zasílání zpráv. Součástí jednoho oboru názvů může být několik front a témat, přičemž obory názvů často slouží jako kontejnery aplikací. Obory názvů služby Service Bus je možné vytvořit dvěma způsoby:

1. Portál Azure Portal (tento článek)
2. [Šablony Resource Manageru][create-namespace-using-arm]

## <a name="create-a-namespace-in-the-azure-portal"></a>Vytvoření oboru názvů na webu Azure Portal
[!INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

Blahopřejeme! Právě jste vytvořili obor názvů zasílání zpráv služby Service Bus.

## <a name="next-steps"></a>Další kroky
Podívejte se na naše [ukázky GitHub][github-samples], které představují některé pokročilejší funkce zasílání zpráv služby Azure Service Bus.

[create-namespace-using-arm]: service-bus-resource-manager-overview.md
[github-samples]: https://github.com/Azure-Samples/azure-servicebus-messaging-samples

