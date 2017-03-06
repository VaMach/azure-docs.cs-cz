---
title: "Vytvoření oboru názvů služby Service Bus na webu Azure Portal | Dokumentace Microsoftu"
description: "Postup vytvoření oboru názvů služby Service Bus pomocí webu Azure Portal"
services: service-bus-messaging
documentationcenter: .net
author: jtaubensee
manager: timlt
editor: 
ms.assetid: fbb10e62-b133-4851-9d27-40bd844db3ba
ms.service: service-bus-messaging
ms.devlang: tbd
ms.topic: get-started-article
ms.tgt_pltfrm: dotnet
ms.workload: na
ms.date: 11/30/2016
ms.author: jotaub
translationtype: Human Translation
ms.sourcegitcommit: 4a972b9b8b52a90f27afda98d8bdc661016d1fe1
ms.openlocfilehash: ba7093f8a2c06ab4cecf11207174a4871435ae64
ms.lasthandoff: 02/28/2017


---
# <a name="create-a-service-bus-namespace-using-the-azure-portal"></a>Vytvoření oboru názvů služby Service Bus pomocí webu Azure Portal
Obor názvů je společný kontejner pro všechny součásti zasílání zpráv. Součástí jednoho oboru názvů může být několik front a témat, přičemž obory názvů často slouží jako kontejnery aplikací. Obory názvů služby Service Bus je v současnosti možné vytvořit dvěma způsoby.

1. Portál Azure Portal (tento článek)
2. [Šablony Resource Manageru][create-namespace-using-arm]

## <a name="create-a-namespace-in-the-azure-portal"></a>Vytvoření oboru názvů na webu Azure Portal
[!INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

Blahopřejeme! Právě jste vytvořili obor názvů zasílání zpráv služby Service Bus.

## <a name="next-steps"></a>Další kroky
Podívejte se na naše [ukázky GitHub][github-samples], které představují některé pokročilejší funkce zasílání zpráv služby Azure Service Bus.

[create-namespace-using-arm]: service-bus-resource-manager-overview.md
[github-samples]: https://github.com/Azure-Samples/azure-servicebus-messaging-samples

