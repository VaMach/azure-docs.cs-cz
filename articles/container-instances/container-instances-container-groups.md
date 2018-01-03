---
title: "Kontejner Azure instance skupiny kontejnerů"
description: "Pochopit, jak fungují skupiny kontejnerů v Azure kontejner instancí"
services: container-instances
author: seanmck
manager: timlt
ms.service: container-instances
ms.topic: article
ms.date: 12/19/2017
ms.author: seanmck
ms.custom: mvc
ms.openlocfilehash: b4a0af8fffd3ce012bf9addeec7029884d4ccf25
ms.sourcegitcommit: 85012dbead7879f1f6c2965daa61302eb78bd366
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/02/2018
---
# <a name="container-groups-in-azure-container-instances"></a>Skupiny kontejnerů v Azure kontejner instancí

Je nejvyšší úrovně prostředků v Azure kontejner instancí *skupina kontejneru*. Tento článek popisuje, jaké jsou skupiny kontejnerů a typy scénáře, které umožňují.

## <a name="how-a-container-group-works"></a>Jak funguje skupina kontejneru

Skupina kontejneru je kolekce kontejnery, které získat naplánovat na stejném hostitelském počítači. Kontejnery v kontejneru skupiny sdílet životního cyklu, místní sítí a svazky úložiště. Je podobný koncept *pod* v [Kubernetes] [ kubernetes-pod] a [DC/OS][dcos-pod].

Následující diagram ukazuje příklad skupinou kontejneru, která obsahuje více kontejnerů.

![Diagram skupiny kontejneru][container-groups-example]

Tato skupina kontejneru příklad:

* Je naplánován na jeden hostitelský počítač.
* Zpřístupní jednu veřejnou IP adresu, s zveřejněné jeden port.
* Se skládá ze dvou kontejnery. Jeden kontejner naslouchá na portu 80, při dalších sleduje na port 5000.
* Zahrnuje dvě Azure sdílené složky jako svazek připojení zařízení a každý kontejner připojí některé sdílené složky místně.

### <a name="networking"></a>Sítě

Skupiny kontejnerů sdílet IP adresy a portu obor názvů na tuto IP adresu. Pokud chcete povolit externí klienti dosáhli na kontejner v rámci skupiny, musí vystavit port na IP adrese a z kontejneru. Vzhledem k tomu, že kontejnery v rámci skupiny sdílet port obor názvů, mapování portů se nepodporuje. Kontejnery v rámci skupiny dosáhnout vzájemně prostřednictvím místního hostitele na porty, které se mají vystavený, i když tyto porty se nezobrazí externě na IP adrese skupiny.

### <a name="storage"></a>Úložiště

Můžete zadat externí svazky připojit v rámci skupiny kontejneru. Můžete namapovat tyto svazky do konkrétních cest v rámci jednotlivých kontejnerů ve skupině.

## <a name="common-scenarios"></a>Obvyklé scénáře

Více kontejner skupiny jsou užitečné v případech, ve které chcete rozdělit jeden úkol funkční do malý počet kontejneru Image, které mohou být dodány pomocí různými týmy a obsahují požadavky samostatné prostředků.

Příklad použití můžou zahrnovat:

* Kontejner aplikace a kontejner protokolování. Kontejner protokolování shromažďuje protokoly a metriky výstup v hlavní aplikaci a zapíše do dlouhodobého úložiště.
* Aplikace a kontejner monitorování. Monitorování kontejneru pravidelně vytváří požadavek na aplikaci zkontrolujte, zda je spuštěná a správně reagovat a vyvolá výstrahu, pokud není.
* Kontejner obsluhující webové aplikace a kontejner stahování nejnovější obsah od správy zdrojového kódu.

## <a name="next-steps"></a>Další postup

Zjistěte, jak [nasazení s více kontejner skupiny](container-instances-multi-container-group.md) pomocí šablony Azure Resource Manager.

<!-- IMAGES -->
[container-groups-example]: ./media/container-instances-container-groups/container-groups-example.png

<!-- LINKS - External -->
[dcos-pod]: https://dcos.io/docs/1.10/deploying-services/pods/
[kubernetes-pod]: https://kubernetes.io/docs/concepts/workloads/pods/pod/
