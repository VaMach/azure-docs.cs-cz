---
title: "Kontejner Azure instance skupiny kontejnerů"
description: "Pochopit, jak fungují skupiny kontejnerů v Azure kontejner instancí"
services: container-instances
documentationcenter: 
author: seanmck
manager: timlt
editor: 
tags: 
keywords: 
ms.assetid: 
ms.service: container-instances
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/08/2017
ms.author: seanmck
ms.custom: mvc
ms.openlocfilehash: 25eab41c3f0c986bcce33123f86f4c9638b77191
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="container-groups-in-azure-container-instances"></a>Skupiny kontejnerů v Azure kontejner instancí

Nejvyšší úrovně prostředků v Azure kontejner instancí je skupina kontejneru. Tento článek popisuje, jaké jsou skupiny kontejnerů a jaké druhy scénářů umožňují.

## <a name="how-a-container-group-works"></a>Jak funguje skupina kontejneru

Skupina kontejneru je kolekce kontejnery, které získat naplánovat na stejném hostitelském počítači a sdílet životního cyklu, místní sítě a svazky úložiště. Je podobný koncept *pod* v [Kubernetes](https://kubernetes.io/docs/concepts/workloads/pods/pod/) a [DC/OS](https://dcos.io/docs/1.9/deploying-services/pods/).

Následující diagram ukazuje příklad skupinou kontejneru, která obsahuje více kontejnerů.

![Příklad skupiny kontejneru][container-groups-example]

Poznámky:

- Skupiny je naplánováno na jeden hostitelský počítač.
- Skupiny zpřístupní jednu veřejnou IP adresu, s zveřejněné jeden port.
- Skupiny je tvořen dvěma kontejnery. Jeden kontejner naslouchá na portu 80, při dalších sleduje na port 5000.
- Skupiny obsahuje dvě sdílené složky Azure jako svazek připojení zařízení a každý kontejner připojí některé sdílené složky místně.

### <a name="networking"></a>Sítě

Skupiny kontejnerů sdílet IP adresy a portu obor názvů na tuto IP adresu. Pokud chcete povolit externí klienti dosáhli na kontejner v rámci skupiny, musí vystavit port na IP adrese a z kontejneru. Vzhledem k tomu, že kontejnery v rámci skupiny sdílet port obor názvů, mapování portů se nepodporuje. Kontejnery v rámci skupiny dosáhnout vzájemně prostřednictvím místního hostitele na porty, které se mají vystavený, i když tyto porty se nezobrazí externě na IP adrese skupiny.

### <a name="storage"></a>Úložiště

Můžete zadat externí svazky připojit v rámci skupiny kontejneru. Můžete namapovat tyto svazky do konkrétních cest v rámci jednotlivých kontejnerů ve skupině.

## <a name="common-scenarios"></a>Obvyklé scénáře

Více kontejner skupiny jsou užitečné v případech, ve které chcete rozdělit jeden úkol funkční do malý počet kontejneru Image, které mohou být dodány pomocí různými týmy a obsahují požadavky samostatné prostředků. Příklad použití můžou zahrnovat:

- Kontejner aplikace a kontejner protokolování. Kontejner protokolování shromažďuje protokoly a metriky výstup v hlavní aplikaci a zapíše do dlouhodobého úložiště.
- Aplikace a kontejner monitorování. Monitorování kontejneru pravidelně odešle požadavek na aplikaci zkontrolujte, zda je spuštěná a správně reagovat a vyvolá výstrahu, pokud není.
- Kontejner obsluhující webové aplikace a kontejner stahování nejnovější obsah od správy zdrojového kódu.

## <a name="next-steps"></a>Další kroky

Zjistěte, jak [nasazení s více kontejner skupiny](container-instances-multi-container-group.md) pomocí šablony Azure Resource Manager.

<!-- IMAGES -->

[container-groups-example]: ./media/container-instances-container-groups/container-groups-example.png