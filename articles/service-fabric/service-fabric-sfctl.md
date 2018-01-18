---
title: "Azure Service Fabric rozhraní příkazového řádku - sfctl | Microsoft Docs"
description: "Popisuje sfctl příkazy Service Fabric rozhraní příkazového řádku."
services: service-fabric
documentationcenter: na
author: rwike77
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: cli
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 12/22/2017
ms.author: ryanwi
ms.openlocfilehash: 23adc7fa6ee53d1c5edd10b2772f8825dbcef2a8
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/18/2018
---
# <a name="sfctl"></a>Sfctl 
Příkazy pro správu clusterů Service Fabric a entity. Tato verze není kompatibilní s modulu runtime Service Fabric 6.0. Příkazy postupujte podle vzoru sloveso-podstatné jméno, viz následující podskupiny Další informace.

## <a name="subgroups"></a>Podskupiny
|Podskupinu|Popis|
| --- | --- |
| [aplikace](service-fabric-sfctl-application.md)| Vytvoření, odstranění a spravovali aplikace a typy aplikací.|
| [chaos](service-fabric-sfctl-chaos.md)   | Spuštění, zastavení a sestav ve službě chaos testu.|
| [cluster](service-fabric-sfctl-cluster.md) | Vyberte, Správa a provoz clusterů Service Fabric.|
| [compose](service-fabric-sfctl-compose.md) | Vytvoření, odstranění a spravovat Docker Compose aplikace.|
| [je](service-fabric-sfctl-is.md)      | Dotaz a odeslat příkazy ke službě infrastruktury.|
| [uzel](service-fabric-sfctl-node.md)    | Spravujte uzly, které vytvoří cluster.|
| [oddíl](service-fabric-sfctl-partition.md)  | Dotazování a správu oddílů pro libovolnou službu.|
| [rpm](service-fabric-sfctl-rpm.md)        | Dotaz a odesílat příkazy na service manager opravit.|
| [repliky](service-fabric-sfctl-replica.md) | Spravujte repliky, které patří do oddílů služby.|
| [service](service-fabric-sfctl-service.md) | Vytvořit, odstranit a spravovat službu, typů služeb a balíčky služeb.|
| [úložiště](service-fabric-sfctl-store.md)   | Proveďte základní souborové úrovni operace na clusteru úložiště bitových kopií.|

## <a name="next-steps"></a>Další postup
- [Nastavit](service-fabric-cli.md) rozhraní příkazového řádku služby prostředků infrastruktury.
- Další informace o použití pomocí Service Fabric rozhraní příkazového řádku [ukázkové skripty](/azure/service-fabric/scripts/sfctl-upgrade-application).