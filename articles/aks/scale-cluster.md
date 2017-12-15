---
title: "Škálování clusteru Azure Container Service (AKS) | Dokumentace Microsoftu"
description: "Škálujte cluster Azure Container Service (AKS)."
services: container-service
documentationcenter: 
author: gabrtv
manager: timlt
editor: 
tags: aks, azure-container-service
keywords: Kubernetes, Docker, Containers, Microservices, Azure
ms.assetid: 
ms.service: container-service
ms.devlang: na
ms.topic: overview
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/15/2017
ms.author: gamonroy
ms.custom: mvc
ms.openlocfilehash: b2fa3ebb7a22b9d19678d45cc50806627ab80e90
ms.sourcegitcommit: c25cf136aab5f082caaf93d598df78dc23e327b9
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/15/2017
---
# <a name="scale-an-azure-container-service-aks-cluster"></a>Škálování clusteru Azure Container Service (AKS)

Cluster AKS je možné snadno škálovat na různý počet uzlů.  Vyberte požadovaný počet uzlů a spusťte příkaz `az aks scale`.  Během vertikálního snižování kapacity jsou uzly pečlivě [uzavřené a vyprázdněné](https://kubernetes.io/docs/tasks/administer-cluster/safely-drain-node/), aby se minimalizovalo přerušení spuštěných aplikací.  Během vertikálního navyšování kapacity `az`příkaz čeká na označení uzlů`Ready` clusterem Kubernetes.

## <a name="scale-the-cluster-nodes"></a>Škálování uzlů clusteru

Uzly clusteru můžete škálovat pomocí příkazu `az aks scale`. Následující příklad škáluje cluster *myK8SCluster* na jediný uzel.

```azurecli-interactive
az aks scale --name myK8sCluster --resource-group myResourceGroup --node-count 1
```

Výstup:

```json
{
  "id": "/subscriptions/4f48eeae-9347-40c5-897b-46af1b8811ec/resourcegroups/myResourceGroup/providers/Microsoft.ContainerService/managedClusters/myK8sCluster",
  "location": "eastus",
  "name": "myK8sCluster",
  "properties": {
    "accessProfiles": {
      "clusterAdmin": {
        "kubeConfig": "..."
      },
      "clusterUser": {
        "kubeConfig": "..."
      }
    },
    "agentPoolProfiles": [
      {
        "count": 1,
        "dnsPrefix": null,
        "fqdn": null,
        "name": "myK8sCluster",
        "osDiskSizeGb": null,
        "osType": "Linux",
        "ports": null,
        "storageProfile": "ManagedDisks",
        "vmSize": "Standard_D2_v2",
        "vnetSubnetId": null
      }
    ],
    "dnsPrefix": "myK8sClust-myResourceGroup-4f48ee",
    "fqdn": "myk8sclust-myresourcegroup-4f48ee-406cc140.hcp.eastus.azmk8s.io",
    "kubernetesVersion": "1.7.7",
    "linuxProfile": {
      "adminUsername": "azureuser",
      "ssh": {
        "publicKeys": [
          {
            "keyData": "..."
          }
        ]
      }
    },
    "provisioningState": "Succeeded",
    "servicePrincipalProfile": {
      "clientId": "e70c1c1c-0ca4-4e0a-be5e-aea5225af017",
      "keyVaultSecretRef": null,
      "secret": null
    }
  },
  "resourceGroup": "myResourceGroup",
  "tags": null,
  "type": "Microsoft.ContainerService/ManagedClusters"
}
```

## <a name="next-steps"></a>Další kroky

Další informace o nasazení a správě AKS najdete v kurzech AKS.

> [!div class="nextstepaction"]
> [Kurz AKS](./tutorial-kubernetes-prepare-app.md)
