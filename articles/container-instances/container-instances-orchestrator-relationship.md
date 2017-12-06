---
title: "Instancí Azure kontejnerů a kontejner Orchestration"
description: "Pochopit, jak Azure kontejner instancí interakci s orchestrators kontejneru"
services: container-instances
author: seanmck
manager: timlt
ms.service: container-instances
ms.topic: article
ms.date: 07/24/2017
ms.author: seanmck
ms.custom: mvc
ms.openlocfilehash: a51e746c501cca0521972b09d145439348d1d22d
ms.sourcegitcommit: a48e503fce6d51c7915dd23b4de14a91dd0337d8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/05/2017
---
# <a name="azure-container-instances-and-container-orchestrators"></a>Azure instancí kontejnerů a kontejner orchestrators

Kvůli jejich malá velikost a orientaci aplikace je výhodné pro agilní doručení prostředí a na základě mikroslužbu architektury kontejnery. Úlohu automatizace a správa velký počet kontejnerů a jejich vzájemné interakce se označuje jako *orchestration*. Oblíbených kontejneru orchestrators zahrnují Kubernetes, DC/OS a Docker Swarm, které jsou k dispozici v [Azure Container Service](https://docs.microsoft.com/azure/container-service/).

Azure instancí kontejneru obsahuje některé základní funkce plánování orchestration platforem, ale nezahrnuje služby vyšší hodnota, že tyto platformy poskytují a může být ve skutečnosti doplňkové s nimi. Tento článek popisuje oboru co zpracovává instancí kontejnerů Azure a jak úplné orchestrators kontejner může pracovat s ním.

## <a name="traditional-orchestration"></a>Tradiční orchestration

Standardní definice orchestration zahrnuje následující úkoly:

- **Plánování**: daného kontejneru image a žádost o prostředek, najít vhodný počítač, na který se má spustit kontejneru.
- **Spřažení/proti-affinity**: určení, že by měl sadu kontejnery spuštěny nedaleko druhou (pro výkon) nebo dostatečně daleko od sebe (pro dostupnost).
- **Sledování stavu**: Podívejte se na chyby kontejneru a automaticky znovu je naplánujte.
- **Převzetí služeb při selhání**: sledovat co běží na každém počítači a změnit plán naplánovaných kontejnery z počítačů se nezdařilo pro uzly v pořádku.
- **Škálování**: Přidání nebo odebrání kontejneru instancí tak, aby odpovídaly vyžádání, ručně nebo automaticky.
- **Sítě**: zadejte překryvné sítě pro spolupráci kontejnery pro komunikaci mezi více počítačů hostitele.
- **Zjišťování služby**: Povolit kontejnery navzájem to i v případě jejich přechodu mezi hostitele počítače a změnit IP adres automaticky vyhledat.
- **Koordinované upgradů aplikací**: Správa kontejneru upgrady vyhnout aplikace výpadek a povolte vrácení zpět, pokud dojde k chybě.

## <a name="orchestration-with-azure-container-instances-a-layered-approach"></a>Orchestration s instancemi Azure kontejneru: vrstveného přístupu

Azure instancí kontejnerů umožňuje vrstveného přístupu k orchestration, poskytuje všechny funkce správy a plánování, které jsou nutná k provozování jediný kontejner, současně platformy orchestrator ke správě více kontejneru úloh nad jeho.

Protože všechny základní infrastruktury pro kontejner instancí je spravovaná službou Azure, není potřeba platformě orchestrator zabývat se hledání příslušné hostitelský počítač, na který se má spustit jeden kontejner. Pružnost cloudu zajistí, že než je vždy k dispozici. Místo toho orchestrator můžete soustředit na úlohy, které zjednodušují vývoj architektury více kontejneru, včetně změny a koordinované upgrady.



## <a name="potential-scenarios"></a>Možných scénářů

Sice stále nascent orchestrator integrace s instancemi Azure kontejneru, Očekáváme, že můžou vznikat v několika různých prostředích:

### <a name="orchestration-of-container-instances-exclusively"></a>Orchestration instancí kontejneru výhradně

Protože rychle začít a účtovat pošle druhou, prostředí založené výhradně na kontejner instancí Azure nabízí nejrychlejší způsob začít a jak nakládat s vysoce proměnné úlohy.

### <a name="combination-of-container-instances-and-containers-in-virtual-machines"></a>Kombinace instancí kontejnerů a kontejnery v virtuální počítače

Pro dlouhodobé, stabilní úlohy Orchestrace kontejnery v clusteru s podporou vyhrazených virtuálních počítačů obvykle bude levnějších než spuštěný stejný kontejnery s instancí kontejnerů. Kontejner instancí však nabízí vynikající řešení pro rychlé rozšiřování a smluvní vaší celkovou kapacitu, jak nakládat s neočekávanou nebo krátkodobou špičky využití. Místo škálování počtu virtuálních počítačů v clusteru, pak nasazení další kontejnery na tyto počítače, orchestrator můžete jednoduše naplánovat další kontejnery pomocí kontejner instancí a odstranit, když jsou už potřeba.

## <a name="sample-implementation-azure-container-instances-connector-for-kubernetes"></a>Ukázka implementace: Azure kontejner instancí konektor pro Kubernetes

K předvedení, jak integrovat platformy orchestration kontejner s instancemi Azure kontejneru, jsme spustili sestavování [ukázka konektor pro Kubernetes][aci-connector-k8s].

Konektor pro Kubernetes napodobuje [kubelet] [ kubelet-doc] registrace jako uzel neomezená kapacitu a odeslání vytvoření [pracovními stanicemi soustředěnými kolem] [ pod-doc] kontejneru a skupiny v Azure kontejner instancí.

<!-- ![ACI Connector for Kubernetes][aci-connector-k8s-gif] -->

Konektory pro ostatní orchestrators může být postavená, podobně jako integrovat platformy primitiv kombinovat napájení nástroje orchestrator rozhraní API s rychlostí a zjednodušení správy kontejnerů v Azure kontejner instancí.

> [!WARNING]
> Konektor nástroje ACI pro Kubernetes *experimentální* a neměl by se používat v produkčním prostředí.

## <a name="next-steps"></a>Další kroky

Vytvoření vaší první kontejneru s instancí kontejnerů Azure pomocí [Průvodce rychlým zahájením](container-instances-quickstart.md).

<!-- IMAGES -->
[aci-connector-k8s-gif]: ./media/container-instances-orchestrator-relationship/aci-connector-k8s.gif

<!-- LINKS -->
[aci-connector-k8s]: https://github.com/azure/aci-connector-k8s
[kubelet-doc]: https://kubernetes.io/docs/admin/kubelet/
[pod-doc]: https://kubernetes.io/docs/concepts/workloads/pods/pod/