---
title: Cluster Azure Kubernetes pro Windows | Dokumentace Microsoftu
description: "Nasazení clusteru Kubernetes pro kontejnery Windows ve službě Azure Container Service a zahájení práce"
services: container-service
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: acs, azure-container-service, kubernetes
keywords: 
ms.assetid: 
ms.service: container-service
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/04/2017
ms.author: danlep
ms.custom: H1Hack27Feb2017
ms.translationtype: Human Translation
ms.sourcegitcommit: 18d4994f303a11e9ce2d07bc1124aaedf570fc82
ms.openlocfilehash: 4e730b65a98af05ea00c5f8ebd9914e3367b66a7
ms.contentlocale: cs-cz
ms.lasthandoff: 05/09/2017


---

# <a name="get-started-with-kubernetes-and-windows-containers-in-container-service"></a>Začínáme s kontejnery Windows a Kubernetes v Container Service


Tento článek ukazuje, jak vytvořit cluster Kubernetes v Azure Container Service, který obsahuje uzly Windows pro spouštění kontejnerů Windows. Začnete použitím příkazu Azure CLI 2.0 `az acs` k vytvoření clusteru Kubernetes v Azure Container Service. Potom začnete pomocí nástroje příkazového řádku Kubernetes `kubectl` pracovat s kontejnery Windows sestavenými z imagí Dockeru. 

> [!NOTE]
> Podpora pro kontejnery Windows s Kubernetes ve službě Azure Container Service je ve verzi Preview. 
>



Následující obrázek ukazuje architekturu clusteru Kubernetes ve službě Azure Container Service s jedním hlavním linuxovým uzlem a dvěma agentskými uzly Windows. 

![Obrázek clusteru Kubernetes v Azure](media/container-service-kubernetes-windows-walkthrough/kubernetes-windows.png)

* Hlavní linuxový uzel obsluhuje pro Kubernetes rozhraní REST API a je přístupný prostřednictvím SSH na portu 22 nebo `kubectl` na portu 443. 
* Agentské uzly Windows jsou seskupené ve skupině dostupnosti Azure a spouští vaše kontejnery. Uzly Windows jsou přístupné prostřednictvím tunelu RDP SSH přes hlavní uzel. Pravidla nástroje pro vyrovnávání zatížení Azure se dynamicky přidávají do clusteru v závislosti na vystavených službách.



Všechny virtuální počítače jsou ve stejné privátní virtuální síti a jsou vzájemně plně přístupné. Na všech virtuálních počítačích běží kubelet, Docker a proxy server.

Další související informace najdete v tématu [Úvod do Azure Container Service](container-service-intro.md) a v [dokumentaci ke Kubernetes](https://kubernetes.io/docs/home/).

## <a name="prerequisites"></a>Požadavky
K vytvoření clusteru Azure Container Service pomocí Azure CLI 2.0 musíte mít:
* účet Azure ([získejte bezplatnou zkušební verzi](https://azure.microsoft.com/pricing/free-trial/)),
* nainstalované [Azure CLI 2.0](/cli/azure/install-az-cli2), k němuž jste přihlášeni.

Pro cluster Kubernetes potřebujete také následující položky. Můžete si je připravit předem nebo je pomocí možností příkazu `az acs create` vygenerovat automaticky při nasazování clusteru. 

* **Veřejný klíč SSH RSA:** Pokud chcete vytvořit klíče SSH (Secure Shell) RSA, postupujte podle pokynů pro systémy [macOS a Linux](../virtual-machines/linux/mac-create-ssh-keys.md) nebo [Windows](../virtual-machines/linux/ssh-from-windows.md). 

* **ID a tajný klíč klienta instančního objektu:** Postup vytvoření instančního objektu služby Azure Active Directory a další informace najdete v tématu [O instančním objektu pro cluster Kubernetes](container-service-kubernetes-service-principal.md).

Příklad příkazu v tomto článku automaticky vygeneruje klíče SSH a instanční objekt.
  
## <a name="create-your-kubernetes-cluster"></a>Vytvoření clusteru Kubernetes

Tady najdete příkazy Azure CLI 2.0 pro vytvoření clusteru. 

### <a name="create-a-resource-group"></a>Vytvoření skupiny prostředků
Vytvořte skupinu prostředků v umístění, kde je Azure Container Service [dostupná](https://azure.microsoft.com/regions/services/). Následující příkaz vytvoří skupinu prostředků *myKubernetesResourceGroup* v umístění *westus*:

```azurecli
az group create --name=myKubernetesResourceGroup --location=westus
```

### <a name="create-a-kubernetes-cluster-with-windows-agent-nodes"></a>Vytvoření clusteru Kubernetes s uzly agentů Windows

Vytvořte ve skupině prostředků cluster Kubernetes pomocí příkazu `az acs create` s použitím `--orchestrator-type=kubernetes` a možností agenta `--windows`. Syntaxi příkazu najdete v [nápovědě](/cli/azure/acs#create) k příkazu `az acs create`.

Následující příkaz vytvoří cluster Container Service *myKubernetesClusterName* s předponou DNS *myPrefix* pro uzel správy a zadanými přihlašovacími údaji pro přístup k uzlům Windows. Tato verze příkazu pro cluster Kubernetes automaticky vygeneruje klíče SSH RSA a instanční objekt.


```azurecli
az acs create --orchestrator-type=kubernetes \
    --resource-group myKubernetesResourceGroup \
    --name=myKubernetesClusterName \
    --dns-prefix=myPrefix \
    --agent-count=2 \
    --generate-ssh-keys \
    --windows --admin-username myWindowsAdminName \
    --admin-password myWindowsAdminPassword
```

Po několika minutách se příkaz dokončí a měli byste mít funkční cluster Kubernetes.

> [!IMPORTANT]
> Pokud váš účet nemá oprávnění k vytvoření instančního objektu služby Azure AD, příkaz vygeneruje chybu podobnou této: `Insufficient privileges to complete the operation.`. Další informace najdete v tématu [O instančním objektu pro cluster Kubernetes](container-service-kubernetes-service-principal.md). 
> 

## <a name="connect-to-the-cluster-with-kubectl"></a>Připojení ke clusteru pomocí kubectl

Pokud se chcete připojit ke clusteru Kubernetes z klientského počítače, použijte klienta příkazového řádku Kubernetes [`kubectl`](https://kubernetes.io/docs/user-guide/kubectl/). 

Pokud `kubectl` nemáte místně nainstalovaný, můžete k instalaci použít příkaz `az acs kubernetes install-cli`. (Můžete si ho také stáhnout z [webu Kubernetes](https://kubernetes.io/docs/tasks/kubectl/install/).)

**Linux nebo macOS**

```azurecli
sudo az acs kubernetes install-cli
```

**Windows**
```azurecli
az acs kubernetes install-cli
```

> [!TIP]
> Ve výchozím nastavení tento příkaz nainstaluje binární soubor `kubectl` do adresáře `/usr/local/bin/kubectl` v systému Linux nebo macOS, nebo do adresáře `C:\Program Files (x86)\kubectl.exe` ve Windows. Pokud chcete zadat jinou instalační cestu, použijte parametr `--install-location`.
>
> Po instalaci `kubectl` se ujistěte, že jeho adresář je v systémové cestě, nebo ho tam přidejte. 


Pak spuštěním následujícího příkazu stáhněte hlavní konfiguraci clusteru Kubernetes do místního souboru `~/.kube/config`:

```azurecli
az acs kubernetes get-credentials --resource-group=myKubernetesResourceGroup --name=myKubernetesClusterName
```

V tomto okamžiku jste být připraveni přistoupit ke clusteru ze svého počítače. Zkuste spustit:

```bash
kubectl get nodes
```

Ověřte, že vidíte seznam počítačů v clusteru.

![Uzly spuštěné v clusteru Kubernetes](media/container-service-kubernetes-windows-walkthrough/kubectl-get-nodes.png)

## <a name="create-your-first-kubernetes-service"></a>Vytvoření první služby Kubernetes

Po vytvoření clusteru a připojení pomocí `kubectl` zkuste z kontejneru Dockeru spustit aplikaci Windows a zpřístupnit ji na internetu. Tento základní příklad používá soubor JSON k určení kontejneru se serverem služby Microsoft IIS a pak ho vytvoří pomocí příkazu `kubctl apply`. 

1. Vytvořte místní soubor `iis.json` a zkopírujte do něj následující. Tento soubor říká Kubernetes, aby spustil službu IIS v systému Windows Server 2016 Server Core s použitím veřejné image z [Docker Hubu](https://hub.docker.com/r/microsoft/iis/). Kontejner používá port 80, ale zpočátku je přístupný pouze v rámci sítě s clustery.

  ```JSON
  {
    "apiVersion": "v1",
    "kind": "Pod",
    "metadata": {
      "name": "iis",
      "labels": {
        "name": "iis"
      }
    },
    "spec": {
      "containers": [
        {
          "name": "iis",
          "image": "microsoft/iis",
          "ports": [
            {
            "containerPort": 80
            }
          ]
        }
      ],
      "nodeSelector": {
        "beta.kubernetes.io/os": "windows"
      }
    }
  }
  ```
2. Chcete-li spustit aplikaci, zadejte:  
  
  ```bash
  kubectl apply -f iis.json
  ```  
3. Pokud chcete sledovat nasazení kontejneru, zadejte:  
  ```bash
  kubectl get pods
  ```
  Zatímco se kontejner nasazuje, je jeho stav `ContainerCreating` (Vytváření kontejneru). 

  ![Kontejner služby IIS ve stavu ContainerCreating](media/container-service-kubernetes-windows-walkthrough/iis-pod-creating.png)   

  Kvůli velikosti image služby IIS může přechod kontejneru do stavu `Running` (Spuštěný) trvat několik minut.

  ![Kontejner služby IIS ve stavu Running](media/container-service-kubernetes-windows-walkthrough/iis-pod-running.png)

4. Pokud chcete cluster zpřístupnit celému světu, zadejte následující příkaz:

  ```bash
  kubectl expose pods iis --port=80 --type=LoadBalancer
  ```

  Tento příkaz způsobí, že Kubernetes vytvoří pravidlo nástroje pro vyrovnávání zatížení Azure s veřejnou IP adresou. Rozšíření této změny do nástroje pro vyrovnávání zatížení trvá několik minut. Další informace najdete v tématu [Kontejnery pro vyrovnávání zatížení v clusteru Kubernetes v Azure Container Service](container-service-kubernetes-load-balancing.md).

5. Spuštěním následujícího příkazu zobrazte stav služby.

  ```bash
  kubectl get svc
  ```

  Zpočátku se IP adresa bude zobrazovat jako `pending` (čekající):

  ![Externí IP adresa ve stavu Pending](media/container-service-kubernetes-windows-walkthrough/iis-svc-expose.png)

  Po několika minutách je IP adresa nastavena:
  
  ![Externí IP adresa pro službu IIS](media/container-service-kubernetes-windows-walkthrough/iis-svc-expose-public.png)


6. Jakmile je externí IP adresa k dispozici, můžete na ni přejít pomocí prohlížeče:

  ![Obrázek přechodu na službu IIS](media/container-service-kubernetes-windows-walkthrough/kubernetes-iis.png)  

7. Pokud chcete odstranit pod služby IIS, zadejte:

  ```bash
  kubectl delete pods iis
  ```

## <a name="next-steps"></a>Další kroky

* Pokud chcete použít uživatelské rozhraní Kubernetes, spusťte příkaz `kubectl proxy`. Pak přejděte na adresu http://localhost:8001/ui.

* Postup sestavení webu IIS a jeho spuštění v kontejneru Windows najdete v doprovodných materiálech na [Docker Hubu](https://hub.docker.com/r/microsoft/iis/).

* Pokud chcete přistupovat k uzlům Windows prostřednictvím tunelu RDP SSH k hlavnímu serveru pomocí nástroje PuTTy, přečtěte si [dokumentaci k ACS-Engine](https://github.com/Azure/acs-engine/blob/master/docs/ssh.md#create-port-80-tunnel-to-the-master). 

