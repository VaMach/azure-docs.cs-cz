---
title: "Monitorování Azure Kubernetes cluster s Datadog"
description: "Monitorování Kubernetes clusteru v Azure Container Service pomocí Datadog"
services: container-service
author: bburns
manager: timlt
ms.service: container-service
ms.topic: article
ms.date: 12/09/2016
ms.author: bburns
ms.custom: mvc
ms.openlocfilehash: 96bbd88f163939b58263f2f3a94b7b4d90f85736
ms.sourcegitcommit: 5d3e99478a5f26e92d1e7f3cec6b0ff5fbd7cedf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/06/2017
---
# <a name="monitor-an-azure-container-service-cluster-with-datadog"></a>Monitorování clusteru Azure Container Service s DataDog

[!INCLUDE [aks-preview-redirect.md](../../../includes/aks-preview-redirect.md)]

## <a name="prerequisites"></a>Požadavky
Tento návod předpokládá, že máte [vytvořit Kubernetes clusteru Azure Container Service pomocí](container-service-kubernetes-walkthrough.md).

Předpokládá také, abyste měli `az` rozhraní příkazového řádku Azure a `kubectl` nástroje nainstalované.

Můžete otestovat, pokud máte `az` nainstalovaná, spuštěním nástroje:

```console
$ az --version
```

Pokud nemáte `az` nástroj nainstalovali, jsou k dispozici pokyny [zde](https://github.com/azure/azure-cli#installation).

Můžete otestovat, pokud máte `kubectl` nainstalovaná, spuštěním nástroje:

```console
$ kubectl version
```

Pokud nemáte `kubectl` nainstalován, můžete spustit:

```console
$ az acs kubernetes install-cli
```

## <a name="datadog"></a>DataDog
Datadog je monitorování služba, která shromažďuje data monitorování z kontejnerů v rámci clusteru Azure Container Service. Datadog má řídicí panel integrace Dockeru kde můžete zobrazit konkrétní metriky v rámci kontejnerů. Metriky shromážděná z kontejnerů jsou uspořádané podle využití procesoru, paměti, síťové a vstupně-výstupní operace. Datadog rozdělí metriky na kontejnery a obrázků.

Je nutné nejprve [vytvoření účtu](https://www.datadoghq.com/lpg/)

## <a name="installing-the-datadog-agent-with-a-daemonset"></a>Instalace agenta Datadog s DaemonSet
DaemonSets Kubernetes používá ke spuštění jednu instanci kontejner na jednotlivých hostitelích v clusteru.
Jsou ideální pro spuštění monitorovací agenty.

Po přihlášení do Datadog můžete provést [Datadog pokyny](https://app.datadoghq.com/account/settings#agent/kubernetes) k instalaci agentů Datadog v clusteru pomocí DaemonSet.

## <a name="conclusion"></a>Závěr
A to je vše! Jakmile agenti jsou spuštěná byste měli vidět data v konzole za pár minut. Můžete navštívit integrované [řídicí panel kubernetes](https://app.datadoghq.com/screen/integration/kubernetes) se zobrazí souhrn clusteru.
