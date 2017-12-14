---
title: "Ukázky Azure PowerShell - Service Fabric | Microsoft Docs"
description: "Ukázky Azure PowerShell - Service Fabric"
services: service-fabric
documentationcenter: service-fabric
author: rwike77
manager: timlt
editor: 
tags: 
ms.assetid: b48d1137-8c04-46e0-b430-101e07d7e470
ms.service: service-fabric
ms.devlang: na
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: service-fabric
ms.date: 12/13/2017
ms.author: ryanwi
ms.custom: mvc
ms.openlocfilehash: 1825b2a58e1022f22c71395477a5fca54c715455
ms.sourcegitcommit: fa28ca091317eba4e55cef17766e72475bdd4c96
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/14/2017
---
# <a name="azure-powershell-samples"></a>Ukázky Azure PowerShellu

Následující tabulka obsahuje odkazy na ukázky skripty prostředí PowerShell, které vytvoření a správě clusterů Service Fabric, aplikace a služby.

[!INCLUDE [links to azure cli and service fabric cli](../../includes/service-fabric-powershell.md)]

| | |
|-|-|
| **Vytvoření clusteru** ||
| [Vytvoření clusteru s podporou (Azure)](./scripts/service-fabric-powershell-create-secure-cluster-cert.md)| Vytvoří cluster služby Azure Service Fabric. |
|[Vytvoření clusteru s podporou test (Azure)](./scripts/service-fabric-powershell-create-test-cluster.md)| Vytvoří testovací tři uzly clusteru Service Fabric v Azure.|
| **Spravovat cluster, uzly a infrastruktury** ||
| [Přidat certifikátu aplikace](./scripts/service-fabric-powershell-add-application-certificate.md)| Přidá certifikát X.509 aplikace na všech uzlech v clusteru. |
| [Aktualizovat rozsah portů RDP v clusteru virtuálních počítačů](./scripts/service-fabric-powershell-change-rdp-port-range.md)|Změní rozsahu portu RDP na uzlu clusteru virtuálních počítačů v clusteru s podporou nasazené.|
| [Aktualizace uživatel s oprávněními správce a heslo pro uzel clusteru virtuálních počítačů](./scripts/service-fabric-powershell-change-rdp-user-and-pw.md) | Aktualizuje uživatelské jméno správce a heslo pro uzel clusteru virtuálních počítačů. |
| [Otevření portu v nástroji pro vyrovnávání zatížení](./scripts/service-fabric-powershell-open-port-in-load-balancer.md) | Otevřete port aplikace nástroji pro vyrovnávání zatížení Azure chcete povolit příchozí přenosy na určitém portu. |
| [Vytvoření skupiny pravidla zabezpečení příchozích síťových](./scripts/service-fabric-powershell-add-nsg-rule.md) | Vytvoření skupiny pravidla zabezpečení příchozích síťových chcete povolit příchozí přenosy do clusteru na určitém portu. |
| **Správa aplikací** ||
| [Nasazení aplikace](./scripts/service-fabric-powershell-deploy-application.md)| Nasazení aplikace do clusteru.|
| [Upgrade aplikace](./scripts/service-fabric-powershell-upgrade-application.md)| Upgrade aplikace.|
| [Odebrání aplikace](./scripts/service-fabric-powershell-remove-application.md)| Odebrání aplikace z clusteru.|
