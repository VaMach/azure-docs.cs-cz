---
title: "Kontejner Azure rychlý start služba – nasazení clusteru DC/OS | Microsoft Docs"
description: "Kontejner Azure rychlý start služba – nasazení clusteru DC/OS"
services: container-service
documentationcenter: 
author: neilpeterson
manager: timlt
editor: 
tags: acs, azure-container-service
keywords: "Docker, Kontejnery, mikroslužby, Kubernetes, DC/OS, Azure"
ms.assetid: 
ms.service: container-service
ms.devlang: azurecli
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/04/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 8070d224fe6281e61f67483d4f1dd905a2ab99eb
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="deploy-a-dcos-cluster"></a>Nasazení clusteru DC/OS

DC/OS poskytuje distribuované platformu pro spuštění moderní a kontejnerizované aplikace. S Azure Container Service zřizování provozní připravený cluster DC/OS je jednoduchý a rychlé. Podrobnosti o této úvodní základní kroky potřebné k nasazení DC/OS clusteru a spuštění základní úlohy.

Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) před tím, než začnete.

Tento kurz vyžaduje Azure CLI verze 2.0.4 nebo novější. Verzi zjistíte spuštěním příkazu `az --version`. Pokud potřebujete upgrade, přečtěte si téma [Instalace Azure CLI 2.0]( /cli/azure/install-azure-cli). 

## <a name="log-in-to-azure"></a>Přihlaste se k Azure. 

Přihlaste se k předplatnému Azure pomocí příkazu [az login](/cli/azure/#login) a postupujte podle pokynů na obrazovce.

```azurecli
az login
```

## <a name="create-a-resource-group"></a>Vytvoření skupiny prostředků

Vytvořte skupinu prostředků pomocí příkazu [az group create](/cli/azure/group#create). Skupina prostředků Azure je logický kontejner, ve kterém se nasazují a spravují prostředky Azure. 

Následující příklad vytvoří skupinu prostředků *myResourceGroup* v umístění *eastus*.

```azurecli
az group create --name myResourceGroup --location eastus
```

## <a name="create-dcos-cluster"></a>Vytvoření clusteru DC/OS

Vytvořit cluster DC/OS s [vytvořit acs az](/cli/azure/acs#create) příkaz.

Následující příklad vytvoří cluster DC/OS s názvem *myDCOSCluster* a vytvoří klíče SSH, pokud dosud neexistují. Chcete-li použít konkrétní sadu klíčů, použijte možnost `--ssh-key-value`.  

```azurecli
az acs create --orchestrator-type dcos --resource-group myResourceGroup --name myDCOSCluster --generate-ssh-keys
```

V některých případech, například s omezenou zkušební verzí, má předplatné Azure omezený přístup k prostředkům Azure. Pokud se nasazení nezdaří kvůli omezenému počtu dostupných jader, snižte výchozí počet agentů přidáním možnosti `--agent-count 1` do příkazu [az acs create](/cli/azure/acs#create). 

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

Tunelové propojení SSH může být testována procházením `http://localhost`. Pokud port dalších že byl použit 80, nastavit umístění tak, aby odpovídaly. 

Pokud byl úspěšně vytvořen tunel SSH, je vrácena portálu DC/OS.

![ORCHESTRÁTORU DC/OS UŽIVATELSKÉHO ROZHRANÍ](./media/container-service-dcos-quickstart/dcos-ui.png)

## <a name="install-dcos-cli"></a>Instalace rozhraní příkazového řádku DC/OS

Rozhraní příkazového řádku DC/OS se používá ke správě clusteru DC/OS z příkazového řádku. Instalace pomocí rozhraní příkazového řádku DC/OS [az acs orchestrátoru DC/OS instalace rozhraní příkazového řádku](/azure/acs/dcos#install-cli) příkaz. Pokud používáte Azure CloudShell, rozhraní příkazového řádku DC/OS je již nainstalován. 

Pokud používáte Azure CLI v systému macOS nebo Linux, může být potřeba spustit příkaz s sudo.

```azurecli
az acs dcos install-cli
```

Před použitím rozhraní příkazového řádku s clusterem, je nutné nakonfigurovat na použití tunelového propojení SSH. Uděláte to tak, spusťte následující příkaz, nastavení portu, v případě potřeby.

```azurecli
dcos config set core.dcos_url http://localhost
```

## <a name="run-an-application"></a>Spuštění aplikace

Výchozí hodnota plánování mechanismus pro cluster služby ACS DC/OS je Marathon. Marathon slouží ke spuštění aplikace a správu stavu aplikace na clusteru DC/OS. Při plánování aplikace přes Marathon, vytvořte soubor s názvem *marathon app.json*a zkopírujte do něj následující obsah. 

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
dcos marathon app add marathon-app.json
```

Pokud chcete zobrazit stav nasazení pro aplikaci, spusťte následující příkaz.

```azurecli
dcos marathon app list
```

Když **čekání na** hodnota sloupce přepíná z *True* k *False*, nasazení aplikace byla dokončena.

```azurecli
ID     MEM  CPUS  TASKS  HEALTH  DEPLOYMENT  WAITING  CONTAINER  CMD   
/test   32   1     1/1    ---       ---      False      DOCKER   None
```

Získáte veřejnou IP adresu clusteru agentů DC/OS.

```azurecli
az network public-ip list --resource-group myResourceGroup --query "[?contains(name,'dcos-agent')].[ipAddress]" -o tsv
```

Procházení na tuto adresu vrátí výchozí web NGINX.

![NGINX](./media/container-service-dcos-quickstart/nginx.png)

## <a name="delete-dcos-cluster"></a>Odstranění clusteru DC/OS

Pokud již nepotřebujete, můžete použít [odstranění skupiny az](/cli/azure/group#delete) příkaz, který má-li odebrat skupinu prostředků, clusteru DC/OS a všechny související prostředky.

```azurecli
az group delete --name myResourceGroup --no-wait
```

## <a name="next-steps"></a>Další kroky

V této úvodní jste nasadili cluster DC/OS a spustili jednoduché kontejner Docker v clusteru. Další informace o Azure Container Service, i nadále kurzy služby ACS.

> [!div class="nextstepaction"]
> [Správa clusteru služby ACS DC/OS](container-service-dcos-manage-tutorial.md)