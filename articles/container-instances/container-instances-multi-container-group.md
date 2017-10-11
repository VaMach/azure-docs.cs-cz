---
title: "Azure kontejner instancí - skupina více kontejneru | Dokumentace Azure"
description: "Azure kontejner instancí - skupina více kontejneru"
services: container-instances
documentationcenter: 
author: neilpeterson
manager: timlt
editor: 
tags: 
keywords: 
ms.assetid: 
ms.service: container-instances
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/26/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 140f58582645ea32f77e901eb13364ed145bbecf
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="deploy-a-container-group"></a>Nasazení kontejneru skupiny

Azure instancí kontejnerů podporu nasazení několika kontejnerů do jednoho hostitele pomocí *skupina kontejneru*. To je užitečné při sestavování něho aplikace pro protokolování, sledování nebo jakoukoli jinou konfiguraci, které služba potřebuje druhý připojené procesu. 

Tento dokument vás provede spuštění jednoduché něho více kontejneru konfigurace pomocí šablony Azure Resource Manager.

## <a name="configure-the-template"></a>Konfigurace šablony

Vytvořte soubor s názvem `azuredeploy.json` a zkopírujte do ní následující kód json. 

V této ukázce je definována kontejner skupiny s dvěma kontejnery a veřejnou IP adresu. První kontejner skupiny spustí internetovou přístupných aplikaci. Druhý kontejneru něho, zašle požadavek HTTP do hlavní webové aplikace prostřednictvím místní sítě skupiny. 

Tento příklad něho může rozšířit, aby spuštění výstrahy v případě obdržel kódu odpovědi HTTP než 200 OK. 

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
  },
  "variables": {
    "container1name": "aci-tutorial-app",
    "container1image": "microsoft/aci-helloworld:latest",
    "container2name": "aci-tutorial-sidecar",    
    "container2image": "microsoft/aci-tutorial-sidecar"
  },
    "resources": [
      {
        "name": "myContainerGroup",
        "type": "Microsoft.ContainerInstance/containerGroups",
        "apiVersion": "2017-08-01-preview",
        "location": "[resourceGroup().location]",
        "properties": {
          "containers": [
            {
              "name": "[variables('container1name')]",
              "properties": {
                "image": "[variables('container1image')]",
                "resources": {
                  "requests": {
                    "cpu": 1,
                    "memoryInGb": 1.5
                    }
                },
                "ports": [
                  {
                    "port": 80
                  }
                ]
              }
            },
            {
              "name": "[variables('container2name')]",
              "properties": {
                "image": "[variables('container2image')]",
                "resources": {
                  "requests": {
                    "cpu": 1,
                    "memoryInGb": 1.5
                    }
                }
              }
            }
          ],
          "osType": "Linux",
          "ipAddress": {
            "type": "Public",
            "ports": [
              {
                "protocol": "tcp",
                "port": "80"
              }
            ]
          }
        }
      }
    ],
    "outputs": {
      "containerIPv4Address": {
        "type": "string",
        "value": "[reference(resourceId('Microsoft.ContainerInstance/containerGroups/', 'myContainerGroup')).ipAddress.ip]"
      }
    }
  }
```

Použít privátní kontejneru registru bitovou kopii, přidáte objekt v dokumentu json v následujícím formátu.

```json
"imageRegistryCredentials": [
    {
    "server": "[parameters('imageRegistryLoginServer')]",
    "username": "[parameters('imageRegistryUsername')]",
    "password": "[parameters('imageRegistryPassword')]"
    }
]
```

## <a name="deploy-the-template"></a>Nasazení šablony

Vytvořte skupinu prostředků pomocí příkazu [az group create](/cli/azure/group#create).

```azurecli-interactive
az group create --name myResourceGroup --location westus
```

Nasazení šablony s [vytvořit nasazení skupiny az](/cli/azure/group/deployment#create) příkaz.

```azurecli-interactive
az group deployment create --name myContainerGroup --resource-group myResourceGroup --template-file azuredeploy.json
```

Během pár sekund zobrazí se první odpověď z Azure. 

## <a name="view-deployment-state"></a>Zobrazení stavu nasazení

Chcete-li zobrazit stav nasazení, použijte `az container show` příkaz. Tento příkaz vrátí zřízené veřejnou IP adresu přes která je přístupná aplikace.

```azurecli-interactive
az container show --name myContainerGroup --resource-group myResourceGroup -o table
```

Výstup:

```azurecli
Name              ResourceGroup    ProvisioningState    Image                                                             IP:ports           CPU/Memory    OsType    Location
----------------  ---------------  -------------------  ----------------------------------------------------------------  -----------------  ------------  --------  ----------
myContainerGroup  myResourceGrou2  Succeeded            microsoft/aci-tutorial-sidecar,microsoft/aci-tutorial-app:v1      40.118.253.154:80  1.0 core/1.5 gb   Linux     westus
```

## <a name="view-logs"></a>Zobrazit protokoly   

Zobrazit výstup protokolu kontejneru pomocí `az container logs` příkaz. `--container-name` Argument určuje kontejner, ze kterého protokoly pro vyžádání obsahu. V tomto příkladu je zadána první kontejneru. 

```azurecli-interactive
az container logs --name myContainerGroup --container-name aci-tutorial-app --resource-group myResourceGroup
```

Výstup:

```bash
istening on port 80
::1 - - [27/Jul/2017:17:35:29 +0000] "HEAD / HTTP/1.1" 200 1663 "-" "curl/7.54.0"
::1 - - [27/Jul/2017:17:35:32 +0000] "HEAD / HTTP/1.1" 200 1663 "-" "curl/7.54.0"
::1 - - [27/Jul/2017:17:35:35 +0000] "HEAD / HTTP/1.1" 200 1663 "-" "curl/7.54.0"
::1 - - [27/Jul/2017:17:35:38 +0000] "HEAD / HTTP/1.1" 200 1663 "-" "curl/7.54.0"
```

Pokud chcete zobrazit protokoly pro kontejner straně car, spusťte stejný příkaz zadání druhý název kontejneru.

```azurecli-interactive
az container logs --name myContainerGroup --container-name aci-tutorial-sidecar --resource-group myResourceGroup
```

Výstup:

```bash
Every 3.0s: curl -I http://localhost                                                                                                                       Mon Jul 17 11:27:36 2017

  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
  0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--     0  0  1663    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--     0
HTTP/1.1 200 OK
Accept-Ranges: bytes
Content-Length: 1663
Content-Type: text/html; charset=utf-8
Last-Modified: Sun, 16 Jul 2017 02:08:22 GMT
Date: Mon, 17 Jul 2017 18:27:36 GMT
```

Jak můžete vidět, je něho pravidelně zajistit požadavek HTTP je hlavní webové aplikace prostřednictvím místní sítě skupiny zkontrolujte, zda je spuštěn.

## <a name="next-steps"></a>Další kroky

Tento dokument popsané kroky potřebné pro nasazení s více kontejnerů Azure container instance. Koncová instancí kontejnerů Azure prostředí najdete v kurzu instancí kontejnerů Azure.

> [!div class="nextstepaction"]
> [Azure instancí kontejnerů kurzu]:./container-instances-tutorial-prepare-app.md
