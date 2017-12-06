---
title: "CI volaných disku CD s Kubernetes v Azure Container Service"
description: "Jak automatizovat proces CI/CD s volaných k nasazení a upgrade kontejnerizované aplikace na Kubernetes v Azure Container Service"
services: container-service
author: chzbrgr71
manager: timlt
ms.service: container-service
ms.topic: article
ms.date: 03/23/2017
ms.author: briar
ms.custom: mvc
ms.openlocfilehash: cd8f7ae584b6b1afd357cc585df28dedd3c1f70e
ms.sourcegitcommit: 5d3e99478a5f26e92d1e7f3cec6b0ff5fbd7cedf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/06/2017
---
# <a name="jenkins-integration-with-azure-container-service-and-kubernetes"></a>Volaných integraci s Azure Container Service a Kubernetes 

[!INCLUDE [aks-preview-redirect.md](../../../includes/aks-preview-redirect.md)]

V tomto kurzu jsme provede proces nastavte průběžnou integraci aplikace s více kontejnerů do Azure kontejneru služby Kubernetes použití volaných platformy. Pracovní postup aktualizací bitové kopie kontejneru v úložiště Docker Hub a upgraduje Kubernetes pracovními stanicemi soustředěnými kolem pomocí zavedení nasazení. 

## <a name="high-level-process"></a>Proces vysoké úrovně
Toto jsou základní kroky popsané v tomto článku: 
- Instalace clusteru s podporou Kubernetes v kontejneru služby
- Nastavit volaných a konfigurovat přístup ke kontejneru služby
- Vytvoření pracovního postupu volaných
- Otestujte proces CI/CD koncová

## <a name="install-a-kubernetes-cluster"></a>Instalace clusteru s podporou Kubernetes
    
Nasazení clusteru Kubernetes v Azure Container Service pomocí následujících kroků. Úplnou dokumentaci se nachází [zde](container-service-kubernetes-walkthrough.md).

### <a name="step-1-create-a-resource-group"></a>Krok 1: Vytvoření skupiny prostředků
```azurecli
RESOURCE_GROUP=my-resource-group
LOCATION=westus

az group create --name=$RESOURCE_GROUP --location=$LOCATION
```

### <a name="step-2-deploy-the-cluster"></a>Krok 2: Nasazení clusteru
> [!NOTE]
> Následující kroky vyžadují místní veřejný klíč SSH uložen ve složce ~/.ssh.
>

```azurecli
RESOURCE_GROUP=my-resource-group
DNS_PREFIX=some-unique-value
CLUSTER_NAME=any-acs-cluster-name

az acs create \
--orchestrator-type=kubernetes \
--resource-group $RESOURCE_GROUP \
--name=$CLUSTER_NAME \
--dns-prefix=$DNS_PREFIX \
--ssh-key-value ~/.ssh/id_rsa.pub \
--admin-username=azureuser \
--master-count=1 \
--agent-count=5 \
--agent-vm-size=Standard_D1_v2
```

## <a name="set-up-jenkins-and-configure-access-to-container-service"></a>Nastavit volaných a konfigurovat přístup ke kontejneru služby

### <a name="step-1-install-jenkins"></a>Krok 1: Instalace volaných
1. Vytvořte virtuální počítač Azure s Ubuntu 16.04 LTS.  Vzhledem k tomu, že později v krocích budete muset připojit k tomuto virtuálnímu počítači pomocí bash na místním počítači, nastavení ověřování typu SSH veřejným klíčem a vložte veřejný klíč SSH, který je místně uložené ve složce ~/.ssh.  Také si poznamenejte uživatelské jméno zadáte vzhledem k tomu, že toto uživatelské jméno bude potřeba k zobrazení řídicího panelu volaných a pro připojení k virtuálnímu počítači volaných v dalších krocích.
2. Nainstalujte volaných prostřednictvím těchto [pokyny](https://wiki.jenkins-ci.org/display/JENKINS/Installing+Jenkins+on+Ubuntu). Podrobnější kurz je určen v [howtoforge.com](https://www.howtoforge.com/tutorial/how-to-install-jenkins-with-apache-on-ubuntu-16-04).
3. Chcete-li zobrazit řídicí panel volaných na místním počítači, aktualizujte na skupinu zabezpečení sítě Azure povolte port 8080 přidáním příchozí pravidlo, které umožňuje přístup k portu 8080.  Alternativně může nastavit přesměrování portu tak, že spustíte tento příkaz:`ssh -i ~/.ssh/id_rsa -L 8080:localhost:8080 <your_jenkins_user>@<your_jenkins_public_ip`
4. Připojení k serveru volaných pomocí prohlížeče přechodem na veřejné IP adresy (http:// < your_jenkins_public_ip >: 8080) a odemknutí řídicím panelu volaných poprvé heslem počáteční správce.  Heslo správce je uložený v /var/lib/jenkins/secrets/initialAdminPassword volaných virtuálního počítače.  Snadný způsob, jak získat toto heslo je SSH do virtuálního počítače volaných: `ssh <your_jenkins_user>@<your_jenkins_public_ip>`.  Potom spustíte: `sudo cat /var/lib/jenkins/secrets/initialAdminPassword`.
5. Nainstalujte do počítače volaných prostřednictvím těchto Docker [pokyny](https://docs.docker.com/cs-engine/1.13/#install-on-ubuntu-1404-lts-or-1604-lts). To umožňuje Docker příkazy ke spuštění v volaných úlohy.
6. Nakonfigurujte oprávnění Docker umožňující volaných pro přístup k Docker koncový bod.

    ```bash
    sudo chmod 777 /run/docker.sock
    ```
8. Nainstalujte `kubectl` rozhraní příkazového řádku na volaných. Další podrobnosti najdete v [instalace a nastavení kubectl](https://kubernetes.io/docs/tasks/kubectl/install/).  Úlohy volaných použije ke správě a nasazení do clusteru Kubernetes 'kubectl'.

    ```bash
    curl -LO https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl

    chmod +x ./kubectl

    sudo mv ./kubectl /usr/local/bin/kubectl
    ```

### <a name="step-2-set-up-access-to-the-kubernetes-cluster"></a>Krok 2: Nastavení přístup ke clusteru Kubernetes

> [!NOTE]
> Existuje několik přístupů k provedení následujících kroků. Použijte postup, který je pro vás nejjednodušší.
>

1. Kopírování `kubectl` konfigurační soubor do počítače volaných tak, aby měli přístup ke clusteru Kubernetes volaných úlohy. Tyto pokyny předpokládají, že používáte bash z jiný počítač než volaných virtuálního počítače a místní veřejný klíč SSH je uložen ve složce ~/.ssh počítače.

```bash
export KUBE_MASTER=<your_cluster_master_fqdn>
export JENKINS_USER=<your_jenkins_user>
export JENKINS_SERVER=<your_jenkins_public_ip>
sudo ssh $JENKINS_USER@$JENKINS_SERVER sudo mkdir -m 777 /home/$JENKINS_USER/.kube/ \
&& sudo ssh $JENKINS_USER@$JENKINS_SERVER sudo mkdir /var/lib/jenkins/.kube/ \
&& sudo scp -3 -i ~/.ssh/id_rsa azureuser@$KUBE_MASTER:.kube/config $JENKINS_USER@$JENKINS_SERVER:~/.kube/config \
&& sudo ssh -i ~/.ssh/id_rsa $JENKINS_USER@$JENKINS_SERVER sudo cp /home/$JENKINS_USER/.kube/config /var/lib/jenkins/.kube/config \
```
        
2. Z volaných ověřte, že Kubernetes cluster je přístupný.  K tomu, SSH do virtuálního počítače volaných: `ssh <your_jenkins_user>@<your_jenkins_public_ip>`.  Dále ověřte volaných můžete úspěšně připojit ke clusteru: `kubectl cluster-info`.
    

## <a name="create-a-jenkins-workflow"></a>Vytvoření pracovního postupu volaných

### <a name="prerequisites"></a>Požadavky

- Účet GitHub pro kód úložišti.
- Docker Hub účet k ukládání a aktualizaci bitové kopie.
- Kontejnerizované aplikace, které je možné znovu sestavit a aktualizovat. Můžete použít této ukázkové kontejneru aplikace napsané v Golang: https://github.com/chzbrgr71/go-web 

> [!NOTE]
> Je třeba provést následující kroky v účtu Githubu. Nebojte se naklonujte výše úložiště, ale svůj vlastní účet musíte použít ke konfiguraci webhooků, které a volaných přístup.
>

### <a name="step-1-deploy-initial-v1-of-application"></a>Krok 1: Nasazení počáteční v1 aplikace
1. Sestavení aplikace z počítače vývojáře pomocí následujících příkazů. Nahraďte `myrepo` vlastními.
    
    ```bash
    git clone https://github.com/chzbrgr71/go-web.git
    cd go-web
    docker build -t myrepo/go-web .
    ```

2. Doručte bitové kopie do úložiště Docker Hub.

    ```bash
    docker login
    docker push myrepo/go-web
    ```

3. Nasaďte do Kubernetes clusteru.
    
    > [!NOTE] 
    > Upravit `go-web.yaml` souboru k aktualizaci kontejneru image a úložišti.
    >
        
    ```bash
    kubectl create -f ./go-web.yaml --record
    ```
### <a name="step-2-configure-jenkins-system"></a>Krok 2: Konfigurace volaných systému
1. Klikněte na tlačítko **spravovat volaných** > **konfiguraci systému**.
2. V části **Githubu**, vyberte **přidat Server Githubu**.
3. Nechte **adresy URL rozhraní API** jako výchozí.
4. V části **pověření**, přidat pomocí přihlašovacích údajů volaných **tajný text**. Doporučujeme používat Githubu osobní přístupové tokeny, které jsou nakonfigurované v nastavení účtu uživatele Githubu. Další informace o to [sem.](https://help.github.com/articles/creating-a-personal-access-token-for-the-command-line/)
5. Klikněte na tlačítko **testovací připojení** Chcete-li to správně nakonfigurovaná.
6. V části **vlastnosti globálních**, přidejte proměnná prostředí `DOCKER_HUB` a zadejte heslo úložiště Docker Hub. (To je užitečné v této ukázce, ale produkční scénář by vyžadovaly bezpečnější přístup.)
7. Uložte.

![Přístup volaných Githubu](./media/container-service-kubernetes-jenkins/jenkins-github-access.png)

### <a name="step-3-create-the-jenkins-workflow"></a>Krok 3: Vytvoření pracovního postupu volaných
1. Vytvořte položku volaných.
2. Zadejte název (například "přejděte – web") a vyberte **volný styl projektu**. 
3. Zkontrolujte **Githubu projektu** a zadejte adresu URL vašeho úložiště GitHub.
4. V **správu zdrojového kódu**, zadejte adresu URL úložiště GitHub a přihlašovací údaje. 
5. Přidat **sestavení krok** typu **spustit prostředí** a použijte následující text:

    ```bash
    WEB_IMAGE_NAME="myrepo/go-web:kube${BUILD_NUMBER}"
    docker build -t $WEB_IMAGE_NAME .
    docker login -u <your-dockerhub-username> -p ${DOCKER_HUB}
    docker push $WEB_IMAGE_NAME
    ```

6. Přidejte další **sestavení krok** typu **spustit prostředí** a použijte následující text:

    ```bash
    WEB_IMAGE_NAME="myrepo/go-web:kube${BUILD_NUMBER}"
    kubectl set image deployment/go-web go-web=$WEB_IMAGE_NAME --kubeconfig /var/lib/jenkins/config
    ```

![Kroky sestavení volaných](./media/container-service-kubernetes-jenkins/jenkins-build-steps.png)
    
7. Uložit položky volaných a testování pomocí **sestavení teď**.

### <a name="step-4-connect-github-webhook"></a>Krok 4: Připojení webhook Githubu
1. V položce volaných jste vytvořili, klikněte na tlačítko **konfigurace**.
2. V části **sestavení aktivační události**, vyberte **Githubu háku aktivační událost pro dotazování GITScm** a **Uložit**. Tím se automaticky nakonfiguruje webhook Githubu.
3. V úložiště GitHub pro přejděte web, klikněte na **Nastavení > Webhooky**.
4. Ověřte, že adresa URL webhooku volaných byl úspěšně přidán. Adresa URL musí končit "webhook githubu".

![Konfigurace webhooku volaných](./media/container-service-kubernetes-jenkins/jenkins-webhook.png)

## <a name="test-the-cicd-process-end-to-end"></a>Otestujte proces CI/CD koncová

1. Aktualizujte kód pro úložiště a nabízených nebo synchronizace s úložištěm GitHub.
2. Z konzoly volaných, zkontrolujte **sestavení historie** a ověřit, jestli se úloha spustila. Výstup konzoly zobrazení zobrazíte podrobnosti.
3. Z Kubernetes zobrazte podrobnosti o upgradované nasazení:

    ```bash
    kubectl rollout history deployment/go-web
    ```

## <a name="next-steps"></a>Další kroky

- Nasaďte registru kontejner Azure a uložení bitové kopie v zabezpečené úložiště. V tématu [dokumentace Azure kontejneru registru](https://docs.microsoft.com/azure/container-registry).
- Vytvořte složitější pracovní postup, který obsahuje vedle sebe nasazení a automatizovaných testů ve volaných.
- Další informace o CI/CD s volaných a Kubernetes najdete v tématu [volaných blog](https://jenkins.io/blog/2015/07/24/integrating-kubernetes-and-jenkins/).
