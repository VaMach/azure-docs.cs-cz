---
title: "Kurz pro Azure Container Service – spravovat DC/OS"
description: "Kurz pro Azure Container Service – spravovat DC/OS"
services: container-service
author: neilpeterson
manager: timlt
ms.service: container-service
ms.topic: tutorial
ms.date: 07/17/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 0c58bd764cf0fdacd55675f8343c6e7481a11823
ms.sourcegitcommit: 5d3e99478a5f26e92d1e7f3cec6b0ff5fbd7cedf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/06/2017
---
# <a name="azure-container-service-tutorial---manage-dcos"></a>Kurz pro Azure Container Service – spravovat DC/OS

DC/OS poskytuje distribuované platformu pro spuštění moderní a kontejnerizované aplikace. S Azure Container Service zřizování provozní připravený cluster DC/OS je jednoduchý a rychlé. Tento úvodní informace, které základní kroky potřebné k nasazení clusteru DC/OS a spuštění základní úlohy.

> [!div class="checklist"]
> * Vytvoření clusteru služby ACS DC/OS
> * Připojení ke clusteru
> * Instalace rozhraní příkazového řádku DC/OS
> * Nasazení aplikace do clusteru
> * Škálování aplikace v clusteru
> * Škálování uzly clusteru DC/OS
> * Základní správu DC/OS
> * Odstranění clusteru DC/OS

Tento kurz vyžaduje Azure CLI verze 2.0.4 nebo novější. Verzi zjistíte spuštěním příkazu `az --version`. Pokud potřebujete upgrade, přečtěte si téma [Instalace Azure CLI 2.0]( /cli/azure/install-azure-cli). 

## <a name="create-dcos-cluster"></a>Vytvoření clusteru DC/OS

Nejprve vytvořte skupinu prostředků s [vytvořit skupinu az](/cli/azure/group#create) příkaz. Skupina prostředků Azure je logický kontejner, ve kterém se nasazují a spravují prostředky Azure. 

Následující příklad vytvoří skupinu prostředků *myResourceGroup* v umístění *westeurope*.

```azurecli
az group create --name myResourceGroup --location westeurope
```

Dále vytvořte cluster DC/OS s [vytvořit acs az](/cli/azure/acs#create) příkaz.

Následující příklad vytvoří cluster DC/OS s názvem *myDCOSCluster* a vytvoří klíče SSH, pokud dosud neexistují. Chcete-li použít konkrétní sadu klíčů, použijte možnost `--ssh-key-value`.  

```azurecli
az acs create \
  --orchestrator-type dcos \
  --resource-group myResourceGroup \
  --name myDCOSCluster \
  --generate-ssh-keys
```

Po několika minutách se příkaz dokončí a vrátí informace o nasazení.

## <a name="connect-to-dcos-cluster"></a>Připojení ke clusteru DC/OS

Po vytvoření clusteru DC/OS, může být přistupuje prostřednictvím tunelového propojení SSH. Spusťte následující příkaz, který vrátí veřejnou IP adresu hlavního serveru DC/OS. Tato IP adresa je uložené v proměnné a používat v dalším kroku.

```azurecli
ip=$(az network public-ip list --resource-group myResourceGroup --query "[?contains(name,'dcos-master')].[ipAddress]" -o tsv)
```

Vytvoření tunelu SSH, spusťte následující příkaz a postupujte podle na obrazovce pokyny. Pokud port 80 je již používán, příkaz se nezdaří. Aktualizace tunelových port na jedno není ve použití, například `85:localhost:80`. 

```azurecli
sudo ssh -i ~/.ssh/id_rsa -fNL 80:localhost:80 -p 2200 azureuser@$ip
```

## <a name="install-dcos-cli"></a>Instalace rozhraní příkazového řádku DC/OS

Instalace pomocí rozhraní příkazového řádku DC/OS [az acs orchestrátoru DC/OS instalace rozhraní příkazového řádku](/azure/acs/dcos#install-cli) příkaz. Pokud používáte Azure CloudShell, rozhraní příkazového řádku DC/OS je již nainstalován. Pokud používáte Azure CLI v systému macOS nebo Linux, může být potřeba spustit příkaz s sudo.

```azurecli
az acs dcos install-cli
```

Před použitím rozhraní příkazového řádku s clusterem, je nutné nakonfigurovat na použití tunelového propojení SSH. Uděláte to tak, spusťte následující příkaz, nastavení portu, v případě potřeby.

```azurecli
dcos config set core.dcos_url http://localhost
```

## <a name="run-an-application"></a>Spuštění aplikace

Výchozí hodnota plánování mechanismus pro cluster služby ACS DC/OS je Marathon. Marathon slouží ke spuštění aplikace a správu stavu aplikace na clusteru DC/OS. Při plánování aplikace přes Marathon, vytvořte soubor s názvem **marathon app.json**a zkopírujte do něj následující obsah. 

```json
{
  "id": "demo-app-private",
  "cmd": null,
  "cpus": 1,
  "mem": 32,
  "disk": 0,
  "instances": 1,
  "container": {
    "docker": {
      "image": "nginx",
      "network": "BRIDGE",
      "portMappings": [
        {
          "containerPort": 80,
          "protocol": "tcp",
          "name": "80",
          "labels": null
        }
      ]
    },
    "type": "DOCKER"
  }
}
```

Spusťte následující příkaz, který naplánovat spuštění aplikace v clusteru DC/OS.

```azurecli
dcos marathon app add marathon-app.json
```

Pokud chcete zobrazit stav nasazení pro aplikaci, spusťte následující příkaz.

```azurecli
dcos marathon app list
```

Když **úlohy** hodnota sloupce přepíná z *0 nebo 1* k *1 nebo 1*, nasazení aplikace byla dokončena.

```azurecli
ID     MEM  CPUS  TASKS  HEALTH  DEPLOYMENT  WAITING  CONTAINER  CMD   
/test   32   1     0/1    ---       ---      False      DOCKER   None
```

## <a name="scale-marathon-application"></a>Škálování aplikací Marathon

V předchozím příkladu byla vytvořena jedna instance aplikace. K aktualizaci tohoto nasazení tak, aby byly k dispozici tři instance aplikace, otevře **marathon app.json** souboru a aktualizujte vlastnost instance na 3.

```json
{
  "id": "demo-app-private",
  "cmd": null,
  "cpus": 1,
  "mem": 32,
  "disk": 0,
  "instances": 3,
  "container": {
    "docker": {
      "image": "nginx",
      "network": "BRIDGE",
      "portMappings": [
        {
          "containerPort": 80,
          "protocol": "tcp",
          "name": "80",
          "labels": null
        }
      ]
    },
    "type": "DOCKER"
  }
}
```

Aktualizace aplikace pomocí `dcos marathon app update` příkaz.

```azurecli
dcos marathon app update demo-app-private < marathon-app.json
```

Pokud chcete zobrazit stav nasazení pro aplikaci, spusťte následující příkaz.

```azurecli
dcos marathon app list
```

Když **úlohy** hodnota sloupce přepíná z *1/3* k *3/1*, nasazení aplikace byla dokončena.

```azurecli
ID     MEM  CPUS  TASKS  HEALTH  DEPLOYMENT  WAITING  CONTAINER  CMD   
/test   32   1     1/3    ---       ---      False      DOCKER   None
```

## <a name="run-internet-accessible-app"></a>Spuštění aplikace dostupné pro internet

Clusteru ACS DC/OS se skládá z dvě sady uzlu, jeden veřejný, která je přístupná v síti internet a jeden privátní, která není přístupná v síti internet. Výchozí sada je privátní uzly, který byl použit v posledním příkladu.

Chcete-li aplikaci dostupný na Internetu, nasaďte do sady veřejné uzlu. K tomu, dát `acceptedResourceRoles` objektu hodnotu `slave_public`.

Vytvořte soubor s názvem **nginx public.json** a zkopírujte do něj následující obsah.

```json
{
  "id": "demo-app",
  "cmd": null,
  "cpus": 1,
  "mem": 32,
  "disk": 0,
  "instances": 1,
  "container": {
    "docker": {
      "image": "nginx",
      "network": "BRIDGE",
      "portMappings": [
        {
          "containerPort": 80,
          "hostPort": 80,
          "protocol": "tcp",
          "name": "80",
          "labels": null
        }
      ]
    },
    "type": "DOCKER"
  },
  "acceptedResourceRoles": [
    "slave_public"
  ]
}
```

Spusťte následující příkaz, který naplánovat spuštění aplikace v clusteru DC/OS.

```azurecli 
dcos marathon app add nginx-public.json
```

Získáte veřejnou IP adresu clusteru veřejných agentů DC/OS.

```azurecli 
az network public-ip list --resource-group myResourceGroup --query "[?contains(name,'dcos-agent')].[ipAddress]" -o tsv
```

Procházení na tuto adresu vrátí výchozí web NGINX.

![NGINX](./media/container-service-dcos-manage-tutorial/nginx.png)

## <a name="scale-dcos-cluster"></a>Škálování clusteru DC/OS

V předchozích příkladech byla aplikace škálovat na více instancí. DC/OS infrastruktury můžete škálovat také k zajištění vyšší nebo nižší výpočetní kapacity. To lze provést pomocí [škálování služby acs az]() příkaz. 

Pokud chcete zobrazit aktuální počet agentů DC/OS, použijte [zobrazit acs az](/cli/azure/acs#show) příkaz.

```azurecli
az acs show --resource-group myResourceGroup --name myDCOSCluster --query "agentPoolProfiles[0].count"
```

Chcete-li zvýšit počet 5, použijte [škálování služby acs az](/cli/azure/acs#scale) příkaz. 

```azurecli
az acs scale --resource-group myResourceGroup --name myDCOSCluster --new-agent-count 5
```

## <a name="delete-dcos-cluster"></a>Odstranění clusteru DC/OS

Pokud již nepotřebujete, můžete použít [odstranění skupiny az](/cli/azure/group#delete) příkaz, který má-li odebrat skupinu prostředků, clusteru DC/OS a všechny související prostředky.

```azurecli 
az group delete --name myResourceGroup --no-wait
```

## <a name="next-steps"></a>Další kroky

V tomto kurzu jste se naučili o základní úlohy správy DC/OS, včetně následujících. 

> [!div class="checklist"]
> * Vytvoření clusteru služby ACS DC/OS
> * Připojení ke clusteru
> * Instalace rozhraní příkazového řádku DC/OS
> * Nasazení aplikace do clusteru
> * Škálování aplikace v clusteru
> * Škálování uzly clusteru DC/OS
> * Odstranění clusteru DC/OS

Přechodu na v dalším kurzu se dozvíte o aplikaci v DC/OS v Azure Vyrovnávání zatížení. 

> [!div class="nextstepaction"]
> [Vyrovnávání zatížení aplikací](container-service-load-balancing.md)