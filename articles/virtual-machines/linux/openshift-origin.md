---
title: "Nasazení OpenShift původ v Azure | Microsoft Docs"
description: "Nasaďte OpenShift původ v Azure."
services: virtual-machines-linux
documentationcenter: virtual-machines
author: haroldw
manager: najoshi
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 
ms.author: haroldw
ms.openlocfilehash: 1a40c4cc064b32aced7e976f40f6ed6a57e62204
ms.sourcegitcommit: b979d446ccbe0224109f71b3948d6235eb04a967
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/25/2017
---
# <a name="deploy-openshift-origin-in-azure"></a>Nasazení OpenShift původ v Azure

K nasazení OpenShift původ v Azure několika způsoby. Můžete ručně nasadit všechny součásti potřebnou infrastrukturu Azure a pak postupujte podle počátek OpenShift [dokumentaci](https://docs.openshift.org/3.6/welcome/index.html).
Můžete také použít existující šablonu Resource Manageru, která zjednodušuje nasazení clusteru OpenShift původu. Jakmile tyto šablony se nachází [zde](https://github.com/Microsoft/openshift-origin).

## <a name="deploy-using-the-openshift-origin-template"></a>Nasazení pomocí šablony OpenShift původu

Použití `appId` hodnotu z objektu služby, který jste dříve vytvořili pro `aadClientId` parametr.

Následující příklad vytvoří soubor parametrů s názvem **azuredeploy.parameters.json** se všechny požadované vstupy.

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "masterVmSize": {
            "value": "Standard_E2s_v3"
        },
        "infraVmSize": {
            "value": "Standard_E2s_v3"
        },
        "nodeVmSize": {
            "value": "Standard_E2s_v3"
        },
        "openshiftClusterPrefix": {
            "value": "mycluster"
        },
        "masterInstanceCount": {
            "value": 3
        },
        "infraInstanceCount": {
            "value": 2
        },
        "nodeInstanceCount": {
            "value": 2
        },
        "dataDiskSize": {
            "value": 128
        },
        "adminUsername": {
            "value": "clusteradmin"
        },
        "openshiftPassword": {
            "value": "{Strong Password}"
        },
        "sshPublicKey": {
            "value": "{SSH Public Key}"
        },
        "keyVaultResourceGroup": {
            "value": "keyvaultrg"
        },
        "keyVaultName": {
            "value": "keyvault"
        },
        "keyVaultSecret": {
            "value": "keysecret"
        },
        "aadClientId": {
            "value": "11111111-abcd-1234-efgh-111111111111"
        },
        "aadClientSecret": {
            "value": "{Strong Password}"
        },
        "defaultSubDomainType": {
            "value": "nipio"
        }
    }
}
```

### <a name="deploy-using-azure-cli"></a>Nasazení pomocí rozhraní příkazového řádku Azure


> [!NOTE] 
> Tento příkaz vyžaduje rozhraní příkazového řádku Azure 2.0.8 nebo novější. Můžete ověřit az CLI verze se `az --version` příkaz. K aktualizaci verze rozhraní příkazového řádku, najdete v části [nainstalovat Azure CLI 2.0]( /cli/azure/install-azure-cli).

Následující příklad nasadí do skupiny prostředků s názvem myResourceGroup s názvem nasazení myOpenShiftCluster OpenShift clusteru a všechny související prostředky. Šablona se odkazuje přímo z úložiště github a místní parametry soubor s názvem **azuredeploy.parameters.json** soubor je používán.

```azurecli 
az group deployment create -g myResourceGroup --name myOpenShiftCluster \
      --template-uri https://raw.githubusercontent.com/Microsoft/openshift-origin/master/azuredeploy.json \
      --parameters @./azuredeploy.parameters.json
```

Nasazení trvá alespoň 25 minut v závislosti na celkový počet uzlů, které jsou nasazeny. Adresa URL konzoly OpenShift a název DNS je hlavní server OpenShift vytiskne na terminálu po dokončení nasazení.

```json
{
  "OpenShift Console Uri": "http://openshiftlb.cloudapp.azure.com:8443/console",
  "OpenShift Master SSH": "ssh clusteradmin@myopenshiftmaster.cloudapp.azure.com -p 2200"
}
```

## <a name="connect-to-the-openshift-cluster"></a>Připojte se ke clusteru OpenShift

Po dokončení nasazení připojit ke konzole OpenShift pomocí prohlížeče `OpenShift Console Uri`. Alternativně můžete připojit k hlavnímu OpenShift pomocí následujícího příkazu:

```bash
$ ssh -p 2200 clusteradmin@myopenshiftmaster.cloudapp.azure.com
```

## <a name="clean-up-resources"></a>Vyčištění prostředků

Pokud již nepotřebujete, můžete použít [odstranění skupiny az](/cli/azure/group#delete) příkaz, který má-li odebrat skupinu prostředků, OpenShift clusteru a všechny související prostředky.

```azurecli 
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Další kroky

- [Úlohy nasazení POST](./openshift-post-deployment.md)
- [Řešení potíží s nasazením OpenShift](./openshift-troubleshooting.md)
- [Začínáme s OpenShift původu](https://docs.openshift.org/latest/getting_started/index.html)
