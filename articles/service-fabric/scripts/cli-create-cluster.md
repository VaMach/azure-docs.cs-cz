---
title: "Ukázka nasazení skriptu rozhraní příkazového řádku Azure Service Fabric"
description: "Vytvořte cluster Service Fabric Linux zabezpečení v Azure pomocí rozhraní příkazového řádku Azure Service Fabric."
services: service-fabric
documentationcenter: 
author: rwike77
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 
ms.service: service-fabric
ms.workload: multiple
ms.devlang: na
ms.topic: sample
ms.date: 09/18/2017
ms.author: ryanwi
ms.custom: mvc
ms.openlocfilehash: 1fb5828ed65ec4268af5285f23a8fb9f46a96b03
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/08/2017
---
# <a name="create-a-secure-service-fabric-linux-cluster-in-azure"></a>Vytvořit cluster Service Fabric Linux zabezpečení v Azure

Tento příkaz vytvoří certifikát podepsaný svým držitelem, přidá do trezoru klíčů a soubory ke stažení certifikátu místně.  Nový certifikát se používá k zabezpečení clusteru při nasazování.  Můžete také použít existující certifikát místo vytvoření nové.  V obou případech název subjektu certifikátu musí odpovídat domény, který používáte pro přístup ke clusteru Service Fabric. Toto porovnání je potřeba zadat protokolem SSL pro koncové body správy protokolu HTTPS a Service Fabric Explorer clusteru. Nelze získat od certifikační Autority pro certifikát SSL `.cloudapp.azure.com` domény. Je nutné získat vlastní název domény pro váš cluster. Pokud budete požadovat certifikát od certifikační Autority, název subjektu certifikátu musí odpovídat názvu vlastní domény, který používáte pro váš cluster.

V případě potřeby nainstalujte [Azure CLI 2.0](/en-us/cli/azure/install-azure-cli?view=azure-cli-latest).

## <a name="sample-script"></a>Ukázkový skript

[!code-sh[main](../../../cli_scripts/service-fabric/create-cluster/create-cluster.sh "Deploy an application to a cluster")]

## <a name="clean-up-deployment"></a>Vyčištění nasazení

Po spuštění ukázka skriptu, následující příkaz lze použít k odebrání skupiny prostředků clusteru a všechny související prostředky.

```azurecli
ResourceGroupName = "aztestclustergroup"
az group delete --name $ResourceGroupName
```

## <a name="script-explanation"></a>Vysvětlení skriptu

Tento skript používá následující příkazy. Každý příkaz v tabulce odkazy na dokumentaci konkrétní příkaz.

| Příkaz | Poznámky |
|---|---|
| [Vytvoření clusteru sf az](https://docs.microsoft.com/cli/azure/sf/cluster?view=azure-cli-latest#az_sf_cluster_create) | Vytvoří nový cluster Service Fabric.  |

## <a name="next-steps"></a>Další kroky

Další ukázky pro Service Fabric rozhraní příkazového řádku pro Azure Service Fabric najdete v [Service Fabric rozhraní příkazového řádku ukázky](../samples-cli.md).
