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
ms.date: 11/28/2017
ms.author: ryanwi
ms.custom: mvc
ms.openlocfilehash: 17b10011af915a06e7c65f8d321a1e452956c28a
ms.sourcegitcommit: 4ac89872f4c86c612a71eb7ec30b755e7df89722
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/07/2017
---
# <a name="azure-powershell-samples"></a>Ukázky Azure PowerShellu

Následující tabulka obsahuje odkazy na ukázky skripty prostředí PowerShell, které vytvoření a správě clusterů Service Fabric, aplikace a služby.

[!INCLUDE [links to azure cli and service fabric cli](../../includes/service-fabric-powershell.md)]

| | |
|-|-|
| **Vytvoření clusteru** ||
| [Vytvoření clusteru s podporou (Azure)](./scripts/service-fabric-powershell-create-secure-cluster-cert.md)| Vytvoří cluster služby Azure Service Fabric. |
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
