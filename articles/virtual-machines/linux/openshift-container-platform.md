---
title: "Nasazení OpenShift kontejneru platformy v Azure | Microsoft Docs"
description: "Nasaďte OpenShift kontejneru platformy v Azure."
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
ms.openlocfilehash: c91b7232b2f87e0b4b5e659126b96a6ef8b4202c
ms.sourcegitcommit: b979d446ccbe0224109f71b3948d6235eb04a967
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/25/2017
---
# <a name="deploy-openshift-container-platform-in-azure"></a>Nasazení OpenShift kontejneru platformy v Azure

K nasazení kontejneru platformy OpenShift v Azure několika způsoby. Můžete ručně nasadit všechny součásti potřebnou infrastrukturu Azure a pak postupujte podle platformy kontejneru OpenShift [dokumentaci](https://docs.openshift.com/container-platform/3.6/welcome/index.html).
Můžete také použít existující šablonu Resource Manageru, která zjednodušuje nasazení clusteru OpenShift kontejneru platformy. Jakmile tyto šablony se nachází [zde](https://github.com/Microsoft/openshift-container-platform/).

Další možností je použít [Azure Marketplace nabízí](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/redhat.openshift-container-platform?tab=Overview).

Pro obě možnosti vyžaduje Red Hat se předplatné. Během nasazení je RHEL instance zaregistrované předplatné Red Hat a připojené k ID fondu, který obsahuje oprávnění pro platformu OpenShift kontejneru.
Zkontrolujte, zda že máte platný Red Hat odběr Manager uživatelské jméno, heslo a ID fondu (RHSM uživatelské jméno, heslo RHSM a ID fondu). Po přihlášení do https://access.redhat.com můžete ověřit informace.

## <a name="deploy-using-the-openshift-container-platform-resource-manager-template"></a>Nasazení pomocí šablony OpenShift kontejneru platformy Resource Manageru

Pokud chcete nasadit, pomocí šablony Resource Manageru, soubor parametrů slouží k zadání všech vstupních parametrů. Pokud chcete přizpůsobit kterékoli z položky nasazení, které nejsou zahrnuty pomocí vstupních parametrů rozvětvení úložiště github a změnit odpovídající položky.

Některé běžné možnosti přizpůsobení (ale bez omezení):

- Virtuální síť CIDR [proměnné v azuredeploy.json]
- Velikost virtuálního počítače bastionu [proměnné v azuredeploy.json]
- Zásady vytváření názvů [proměnné v azuredeploy.json]
- Specifika clusteru OpenShift - mění prostřednictvím souboru hostitelů [deployOpenShift.sh]

### <a name="configure-parameters-file"></a>Konfigurace souboru parametrů

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
        "enableMetrics": {
            "value": "true"
        },
        "enableLogging": {
            "value": "true"
        },
        "enableCockpit": {
            "value": "false"
        },
        "rhsmUsernamePasswordOrActivationKey": {
            "value": "usernamepassword"
        },
        "rhsmUsernameOrOrgId": {
            "value": "{RHSM Username}"
        },
        "rhsmPasswordOrActivationKey": {
            "value": "{RHSM Password}"
        },
        "rhsmPoolId": {
            "value": "{Pool ID}"
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
        "enableAzure": {
            "value": "true"
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

Nahraďte položky v {} s důležité informace.

### <a name="deploy-using-azure-cli"></a>Nasazení pomocí rozhraní příkazového řádku Azure

> [!NOTE] 
> Tento příkaz vyžaduje rozhraní příkazového řádku Azure 2.0.8 nebo novější. Můžete ověřit az CLI verze se `az --version` příkaz. K aktualizaci verze rozhraní příkazového řádku, najdete v části [nainstalovat Azure CLI 2.0]( /cli/azure/install-azure-cli).

Následující příklad nasadí do skupiny prostředků s názvem myResourceGroup s názvem nasazení myOpenShiftCluster OpenShift clusteru a všechny související prostředky. Šablona se odkazuje přímo z úložiště github a místní parametry soubor s názvem **azuredeploy.parameters.json** soubor je používán.

```azurecli 
az group deployment create -g myResourceGroup --name myOpenShiftCluster \
      --template-uri https://raw.githubusercontent.com/Microsoft/openshift-container-platform/master/azuredeploy.json \
      --parameters @./azuredeploy.parameters.json
```

Nasazení trvá nejméně 30 minut v závislosti na celkový počet uzlů, které jsou nasazeny. Adresa URL konzoly OpenShift a název DNS je hlavní server OpenShift vytiskne na terminálu po dokončení nasazení.

```json
{
  "OpenShift Console Uri": "http://openshiftlb.cloudapp.azure.com:8443/console",
  "OpenShift Master SSH": "ssh clusteradmin@myopenshiftmaster.cloudapp.azure.com -p 2200"
}
```

## <a name="deploy-using-openshift-container-platform-marketplace-offer"></a>Nasazení pomocí nabídky marketplace OpenShift kontejneru platformy

Nejjednodušší způsob, jak nasadit OpenShift kontejneru platformy do Azure se má používat [Azure Marketplace nabízí](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/redhat.openshift-container-platform?tab=Overview).

Tato možnost je nejjednodušší, ale také má omezené možnosti přizpůsobení. Nabídka zahrnuje tři možnosti konfigurace:

- Malá: Nasadí bez-HA cluster s jeden hlavní uzel, jeden uzel infrastruktury, dva uzly aplikace a jeden uzel Bastionu. Všechny uzly jsou velikosti standardní DS2v2 virtuálních počítačů. Tento cluster vyžaduje 10 Celkový počet jader a je ideální pro testování v menším měřítku.
- Střední: Nasadí cluster služby HA tři hlavní uzly, dva uzly infrastruktury, čtyři uzly aplikace a jeden uzel Bastionu. Všechny uzly s výjimkou Bastionu jsou velikosti standardní DS3v2 virtuálních počítačů. Uzel Bastionu je standardní DS2v2. Tento cluster vyžaduje 38 jader.
- Velké: Nasadí HA clusteru služby s tři hlavní uzly, dva uzly infrastruktury, šesti uzlů aplikace a jeden uzel Bastionu. Hlavní a uzly infrastruktury jsou velikosti virtuálních počítačů standardní DS3v2, aplikace uzly jsou velikosti standardní DS4v2 virtuálních počítačů a uzel Bastionu je standardní DS2v2. Tento cluster vyžaduje 70 jader.

Konfigurace zprostředkovatele cloudu Azure je volitelné pro střední a velké velikosti clusteru. Malá velikost není poskytnuta možnost pro konfiguraci poskytovatele cloudových služeb Azure.

## <a name="connect-to-the-openshift-cluster"></a>Připojte se ke clusteru OpenShift

Po dokončení nasazení připojit ke konzole OpenShift pomocí prohlížeče `OpenShift Console Uri`. Alternativně můžete připojit k hlavnímu OpenShift pomocí následujícího příkazu:

```bash
$ ssh clusteradmin@myopenshiftmaster.cloudapp.azure.com -p 2200
```

## <a name="clean-up-resources"></a>Vyčištění prostředků

Pokud již nepotřebujete, můžete použít [odstranění skupiny az](/cli/azure/group#delete) příkaz, který má-li odebrat skupinu prostředků, OpenShift clusteru a všechny související prostředky.

```azurecli 
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Další kroky

- [Úlohy nasazení POST](./openshift-post-deployment.md)
- [Řešení potíží s nasazením OpenShift](./openshift-troubleshooting.md)
- [Začínáme s platformou OpenShift kontejneru](https://docs.openshift.com/container-platform/3.6/getting_started/index.html)
