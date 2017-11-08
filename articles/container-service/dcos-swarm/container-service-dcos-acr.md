---
title: "ACR pomocí clusteru služby Azure DC/OS | Microsoft Docs"
description: "Cluster DC/OS v Azure Container Service pomocí registru kontejneru služby Azure"
services: container-service
documentationcenter: 
author: julienstroheker
manager: dcaro
editor: 
tags: acs, azure-container-service, acr, azure-container-registry
keywords: "Docker, kontejnery, Micro-services, Mesos, Azure, sdílení souborů, cifs"
ms.assetid: 
ms.service: container-service
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/23/2017
ms.author: juliens
ms.custom: mvc
ms.openlocfilehash: 36e57bb6ebf9f55d42c526a361fed33b4238b313
ms.sourcegitcommit: 6a6e14fdd9388333d3ededc02b1fb2fb3f8d56e5
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/07/2017
---
# <a name="use-acr-with-a-dcos-cluster-to-deploy-your-application"></a>Použití ACR s cluster DC/OS pro nasazení aplikace

V tomto článku jsme prozkoumejte použití registru kontejner Azure s cluster DC/OS. Pomocí ACR umožňuje soukromě ukládání a správě bitových kopií kontejneru. Tento kurz obsahuje následující úlohy:

> [!div class="checklist"]
> * Nasazení registru kontejner Azure (v případě potřeby)
> * Konfigurace ověřování ACR na cluster DC/OS
> * Odeslat bitovou kopii do registru kontejner Azure
> * Spusťte kontejner image z registru kontejner Azure

Je třeba cluster služby ACS DC/OS, pokud chcete provést kroky v tomto kurzu. V případě potřeby [tento ukázkový skript](./../kubernetes/scripts/container-service-cli-deploy-dcos.md) můžete vytvořit za vás.

Tento kurz vyžaduje Azure CLI verze 2.0.4 nebo novější. Verzi zjistíte spuštěním příkazu `az --version`. Pokud potřebujete upgrade, přečtěte si téma [Instalace Azure CLI 2.0]( /cli/azure/install-azure-cli). 

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="deploy-azure-container-registry"></a>Nasadit kontejner Azure registru

V případě potřeby vytvořte registru kontejneru Azure s [vytvořit az acr](/cli/azure/acr#create) příkaz. 

Následující příklad vytvoří registr s využitím náhodnému generování název. Registr je také nakonfigurovaný se k pomocí účtu správce `--admin-enabled` argument.

```azurecli-interactive
az acr create --resource-group myResourceGroup --name myContainerRegistry$RANDOM --sku Basic
```

Po vytvoření registru rozhraní příkazového řádku Azure výstupy data podobný následujícímu. Poznamenejte si `name` a `loginServer`, ty se používají v dalších krocích.

```azurecli
{
  "adminUserEnabled": false,
  "creationDate": "2017-06-06T03:40:56.511597+00:00",
  "id": "/subscriptions/f2799821-a08a-434e-9128-454ec4348b10/resourcegroups/myResourceGroup/providers/Microsoft.ContainerRegistry/registries/myContainerRegistry23489",
  "location": "eastus",
  "loginServer": "mycontainerregistry23489.azurecr.io",
  "name": "myContainerRegistry23489",
  "provisioningState": "Succeeded",
  "sku": {
    "name": "Basic",
    "tier": "Basic"
  },
  "storageAccount": {
    "name": "mycontainerregistr034017"
  },
  "tags": {},
  "type": "Microsoft.ContainerRegistry/registries"
}
```

Získání kontejneru přihlašovacích údajů registru pomocí [az acr pověření zobrazit](/cli/azure/acr/credential) příkaz. Nahraďte `--name` se si poznamenali v předchozím kroku. Všimněte si jeden hesla, je potřeba v pozdější fázi.

```azurecli-interactive
az acr credential show --name myContainerRegistry23489
```

Další informace o registru kontejner Azure najdete v tématu [Úvod do privátní registrech kontejner Docker](../../container-registry/container-registry-intro.md). 

## <a name="manage-acr-authentication"></a>Spravovat ACR ověřování

Konvenční způsob, jak nabízení a vyžadování image privátní registr, je nejprve ověřit pomocí registru. Chcete-li to provést, použijte `docker login` na libovolného klienta, který potřebuje přístup k privátním registru. Vzhledem k tomu, že cluster DC/OS může obsahovat mnoho uzlů, které potřebujete se ACR ověřit, je užitečné pro automatizaci tohoto procesu mezi všechny uzly. 

### <a name="create-shared-storage"></a>Vytvoření sdílené úložiště

Tento proces používá sdílenou složku Azure, namontování na každém uzlu v clusteru. Pokud již jste nenastavili sdílené úložiště, najdete v části [nastavit sdílenou složku v clusteru DC/OS](container-service-dcos-fileshare.md).

### <a name="configure-acr-authentication"></a>Konfigurace ověřování ACR

Nejdřív získat plně kvalifikovaný název domény hlavního serveru DC/OS a uložte ho do proměnné.

```azurecli-interactive
FQDN=$(az acs list --resource-group myResourceGroup --query "[0].masterProfile.fqdn" --output tsv)
```

Vytvoření připojení SSH s hlavním serverem (nebo prvního hlavního serveru) ve vašem clusteru založenými na DC/OS. Aktualizujte uživatelské jméno, pokud při vytvoření clusteru se použila jiná než výchozí hodnotu.

```azurecli-interactive
ssh azureuser@$FQDN
```

Spusťte následující příkaz k přihlášení do registru kontejner Azure. Nahraďte `--username` s názvem kontejneru registru a `--password` s jedním z zadaná hesla. Nahraďte poslední argument *mycontainerregistry.azurecr.io* v příkladu s názvem loginServer registru kontejneru. 

Tento příkaz uloží hodnoty ověřování místně v části `~/.docker` cesta.

```azurecli-interactive
docker -H tcp://localhost:2375 login --username=myContainerRegistry23489 --password=//=ls++q/m+w+pQDb/xCi0OhD=2c/hST mycontainerregistry.azurecr.io
```

Vytvořte komprimovaný soubor, který obsahuje hodnoty registru ověřování kontejneru.

```azurecli-interactive
tar czf docker.tar.gz .docker
```

Zkopírujte tento soubor do clusteru sdíleného úložiště. Tento krok umožňuje soubor k dispozici na všech uzlech clusteru DC/OS.

```azurecli-interactive
cp docker.tar.gz /mnt/share/dcosshare
```

## <a name="upload-image-to-acr"></a>Odeslat bitovou kopii na ACR

Nyní z vývojovém počítači, nebo všechny ostatní systémy s Docker nainstalovaná, vytvoření bitové kopie a nahrajte ho do registru kontejner Azure.

Vytvořte kontejner z Ubuntu image.

```azurecli-interactive
docker run ubuntu --name base-image
```

Nyní zachytit kontejneru do novou bitovou kopii. Musí obsahovat název bitové kopie `loginServer` název kontejneru registrywith a formát `loginServer/imageName`.

```azurecli-interactive
docker -H tcp://localhost:2375 commit base-image mycontainerregistry30678.azurecr.io/dcos-demo
````

Přihlaste se do registru kontejner Azure. Nahraďte název názvu loginServer – uživatelské jméno s názvem kontejneru registru a--heslo s jedním z zadaná hesla.

```azurecli-interactive
docker login --username=myContainerRegistry23489 --password=//=ls++q/m+w+pQDb/xCi0OhD=2c/hST mycontainerregistry2675.azurecr.io
```

Nakonec nahrajte image na registru ACR. Tento příklad odešle obrázek s názvem demo orchestrátoru DC/OS.

```azurecli-interactive
docker push mycontainerregistry30678.azurecr.io/dcos-demo
```

## <a name="run-an-image-from-acr"></a>Spusťte bitovou kopii z ACR

Chcete-li použít bitovou kopii z registru ACR, vytvořte soubor názvy *acrDemo.json* a zkopírujte následující text do ní. Nahraďte název bitové kopie s názvem loginServer registru kontejneru a název bitové kopie, například `loginServer/imageName`. Poznamenejte si `uris` vlastnost. Tato vlastnost obsahuje umístění kontejneru ověřování souboru registru, který v tomto případě je Azure sdílené složky, která je připojena v každém uzlu v clusteru DC/OS.

```json
{
  "id": "myapp",
  "container": {
    "type": "DOCKER",
    "docker": {
      "image": "mycontainerregistry30678.azurecr.io/dcos-demo",
      "network": "BRIDGE",
      "portMappings": [
        {
          "containerPort": 80,
          "hostPort": 80,
          "protocol": "tcp",
          "name": "80",
          "labels": null
        }
      ],
      "forcePullImage":true
    }
  },
  "instances": 3,
  "cpus": 0.1,
  "mem": 65,
  "healthChecks": [{
      "protocol": "HTTP",
      "path": "/",
      "portIndex": 0,
      "timeoutSeconds": 10,
      "gracePeriodSeconds": 10,
      "intervalSeconds": 2,
      "maxConsecutiveFailures": 10
  }],
  "uris":  [
       "file:///mnt/share/dcosshare/docker.tar.gz"
   ]
}
```

Nasazení aplikace pomocí rozhraní příkazového řádku DC / ° c.

```azurecli-interactive
dcos marathon app add acrDemo.json
```

## <a name="next-steps"></a>Další kroky

V tomto kurzu máte nakonfigurovat DC/OS použijte registru kontejner Azure, včetně těchto úloh:

> [!div class="checklist"]
> * Nasazení registru kontejner Azure (v případě potřeby)
> * Konfigurace ověřování ACR na cluster DC/OS
> * Odeslat bitovou kopii do registru kontejner Azure
> * Spusťte kontejner image z registru kontejner Azure
