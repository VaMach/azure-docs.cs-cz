---
title: "Připojení ke clusteru Azure Container Service | Dokumentace Microsoftu"
description: "Připojení ke clusterům Kubernetes, DC/OS nebo Docker Swarm ve službě Azure Container Service ze vzdáleného počítače"
services: container-service
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: acs, azure-container-service
keywords: "Docker, Kontejnery, mikroslužby, Kubernetes, DC/OS, Azure"
ms.assetid: ff8d9e32-20d2-4658-829f-590dec89603d
ms.service: container-service
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/12/2017
ms.author: rogardle
translationtype: Human Translation
ms.sourcegitcommit: 2549ca9cd05f44f644687bbdf588f7af01bae3f4
ms.openlocfilehash: 79162e5d31346370e596f39fa4827d49625897b3


---
# <a name="connect-to-an-azure-container-service-cluster"></a>Připojení ke clusteru Azure Container Service
Po vytvoření clusteru Azure Container Service je nutné se ke clusteru připojit kvůli nasazení a správě úloh. Tento článek popisuje, jak se připojit k hlavnímu virtuálnímu počítači clusteru ze vzdáleného počítače. Clustery Kubernetes, DC/OS a Docker Swarm zpřístupňují koncové body REST. V případě Kubernetes je tento koncový bod bezpečně zpřístupněn na internetu a můžete se k němu přímo připojit z libovolného počítače připojeného k internetu spuštěním pomocí nástroje `kubectl` z příkazového řádku. V případě DC/OS a Docker Swarm je nutné pro bezpečné připojení ke koncovému bodu REST vytvořit tunel Secure Shell (SSH). 

> [!NOTE]
> Podpora pro Kubernetes je v Azure Container Service momentálně ve verzi preview.
>

## <a name="prerequisites"></a>Požadavky

* Cluster Kubernetes, DC/OS nebo Swarm [nasazený ve službě v Azure Container Service](container-service-deployment.md)
* Privátní klíč SSH odpovídající veřejnému klíči SSH přidaného do clusteru během nasazení Tyto příkazy předpokládají, že privátní klíč SSH je ve vašem počítači ve složce `$HOME/.ssh/id_rsa`. Tyto pokyny existují také pro [OS X a Linux](../virtual-machines/virtual-machines-linux-mac-create-ssh-keys.md) nebo pro [Windows](../virtual-machines/virtual-machines-linux-ssh-from-windows.md). Pokud připojení SSH nefunguje, možná budete muset [resetovat vaše klíče SSH](../virtual-machines/virtual-machines-linux-troubleshoot-ssh-connection.md).

## <a name="connect-to-a-kubernetes-cluster"></a>Připojení ke clusteru Kubernetes

Podle následujícího postupu nainstalujete a nakonfigurujete `kubectl` ve vašem počítači.

> [!NOTE] 
> V Linuxu nebo v OS X může být nutné spouštět uvedené příkazy pomocí `sudo`.
> 

### <a name="install-kubectl"></a>Instalace kubectl
Jedním ze způsobů, jak tento nástroj nainstalovat, je použít nástroj příkazového řádku Azure 2.0 (Preview) `az acs kubernetes install-cli`. Pokud chcete spustit tento příkaz, ujistěte se, že jste [nainstalovali](/cli/azure/install-az-cli2) nejnovější příkazový řádek Azure CLI 2.0 (Preview) a jste přihlášení k účtu Azure (`az login`).

```azurecli
# Linux or OS X
az acs kubernetes install-cli [--install-location=/some/directory/kubectl]

# Windows
az acs kubernetes install-cli [--install-location=C:\some\directory\kubectl.exe]
```

Případně si můžete klienta stáhnout přímo ze [stránky vydaných verzí](https://github.com/kubernetes/kubernetes/blob/master/CHANGELOG.md#downloads-for-v146).

### <a name="download-cluster-credentials"></a>Stažení přihlašovacích údajů clusteru
Jakmile budete mít `kubectl` nainstalovaný, je třeba, abyste na svůj počítač zkopírovali přihlašovací údaje clusteru. Přihlašovací údaje můžete získat například příkazem `az acs kubernetes get-credentials`. Předejte název skupiny prostředků a název prostředku kontejnerové služby:


```azurecli
az acs kubernetes get-credentials --resource-group=<cluster-resource-group> --name=<cluster-name>
```

Tento příkaz stáhne přihlašovací údaje clusteru do složky `$HOME/.kube/config`, kde `kubectl` očekává, že je najde.

Případně můžete použít `scp` a bezpečně zkopírovat soubor ze složky `$HOME/.kube/config` na hlavním virtuálním počítači do svého místního počítače. Například:

```console
mkdir $HOME/.kube/config
scp azureuser@<master-dns-name>:.kube/config $HOME/.kube/config
```

V systému Windows je třeba použít Bash on Ubuntu on Windows, klienta pro bezpečné kopírování souborů PuTTY nebo něco podobného.



### <a name="use-kubectl"></a>Použití kubectl

Jakmile bude `kubectl` nakonfigurovaný, můžete otestovat připojení výpisem uzlů ve vašem clusteru:

```console
kubectl get nodes
```

Můžete vyzkoušet i další příkazy nástroje `kubectl`. Můžete například zobrazit řídicí panel Kubernetes. Nejprve spusťte proxy na serveru Kubernetes API:

```console
kubectl proxy
```

Uživatelské rozhraní Kubernetes je nyní k dispozici na adrese: `http://localhost:8001/ui`.

Další informace najdete v tématu [Rychlé představení Kubernetes](http://kubernetes.io/docs/user-guide/quick-start/).

## <a name="connect-to-a-dcos-or-swarm-cluster"></a>Připojení ke clusteru DC/OS nebo Swarm

Clustery DC/OS a Docker Swarm nasazené v Azure Container Service zpřístupňují koncové body REST. Tyto koncové body ale nejsou k dispozici pro vnější svět. Pokud chcete tyto koncové body spravovat, je nutné vytvořit tunel Secure Shell (SSH). Po vytvoření tunelu SSH můžete proti koncovým bodům clusteru spouštět příkazy a na svém vlastním systému si můžete přes prohlížeč zobrazit uživatelské rozhraní clusteru. Následující část vás provede vytvořením tunelu SSH z počítače s Linuxem, OS X nebo Windows.

> [!NOTE]
> Se systémem pro správu clusteru je možné vytvořit relace SSH. To ale nedoporučujeme. Práce přímo v systému pro správu představuje riziko neúmyslných změn konfigurace.
> 

### <a name="create-an-ssh-tunnel-on-linux-or-os-x"></a>Vytvoření tunelu SSH v Linuxu a OS X
První věc, kterou je nutné udělat, když vytváříte tunel SSH v Linuxu nebo OS X, je nalezení veřejného názvu DNS hlavních serverů s vyrovnáváním zatížení. Postupujte následovně:


1. Na webu [Azure Portal](https://portal.azure.com) přejděte do skupiny prostředků obsahující váš cluster kontejnerové služby. Rozbalte skupinu prostředků, aby se zobrazily všechny prostředky. 

2. Najděte a vyberte virtuální počítač hlavního uzlu. V clusteru DC/OS začíná název tohoto prostředku na **dcos-master-**. 

    Okno **Virtuální počítač** obsahuje informace o veřejné IP adrese, včetně názvu DNS. Uložte si tento název pro pozdější použití. 

    ![Veřejný název DNS](media/pubdns.png)

3. Nyní otevřete prostředí shell a spusťte příkaz `ssh` s následujícími parametry: 

    **PORT** je port koncového bodu, který chcete zveřejnit. Pro Swarm použijte 2375. Pro DC/OS použijte port 80.  
    **USERNAME** je uživatelské jméno zadané ve chvíli, kdy jste nasadili cluster.  
    **DNSPREFIX** je předpona DNS zadaná ve chvíli, kdy jste nasadili cluster.  
    **REGION** je oblast, ve které je umístěna skupina prostředků.  
    **PATH_TO_PRIVATE_KEY** [NEPOVINNÉ] je cesta k privátnímu klíči, který odpovídá veřejnému klíči zadanému při vytváření clusteru. Tuto možnost použijte spolu s příznakem `-i`.

    ```bash
    ssh -L PORT:localhost:PORT -f -N [USERNAME]@[DNSPREFIX]mgmt.[REGION].cloudapp.azure.com -p 2200
    ```
    > [!NOTE]
    > Port pro připojení SSH je 2200, nikoli standardní port 22. V clusteru s více hlavními virtuálními počítači je to port pro připojení k prvnímu hlavnímu virtuálnímu počítači.
    > 

Podívejte se na příklady pro DC/OS a Swarm v následujících částech.    

### <a name="dcos-tunnel"></a>Tunel DC/OS
Pokud chcete otevřít tunel ke koncovým bodům souvisejícím s DC/OS, spusťte příkaz podobný tomuto:

```bash
sudo ssh -L 80:localhost:80 -f -N azureuser@acsexamplemgmt.japaneast.cloudapp.azure.com -p 2200
```

Nyní můžete ke koncovým bodům souvisejícím s DC/OS přistupovat přes tyto adresy:

* DC/OS: `http://localhost/`
* Marathon: `http://localhost/marathon`
* Mesos: `http://localhost/mesos`

Obdobně můžete přes tento tunel kontaktovat rozhraní REST API pro každou z aplikací.

### <a name="swarm-tunnel"></a>Tunel Swarm
Pokud chcete otevřít tunel ke koncovým bodům Swarmu, spusťte příkaz podobný tomuto:

```bash
ssh -L 2375:localhost:2375 -f -N azureuser@acsexamplemgmt.japaneast.cloudapp.azure.com -p 2200
```

Nyní můžete nastavit proměnnou prostředí vašeho DOCKER_HOST následujícím způsobem. Můžete nadále používat rozhraní příkazového řádku Docker (CLI) jako za normálních okolností.

```bash
export DOCKER_HOST=:2375
```

### <a name="create-an-ssh-tunnel-on-windows"></a>Vytvoření tunelu SSH ve Windows
Tunely SSH je ve Windows možné vytvořit několika způsoby. Tato část popisuje, jak pomocí PuTTY vytvořit tunel.

1. [Stáhněte si PuTTY](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html) do svého počítače s Windows.

2. Spusťte aplikaci.

3. Zadejte název hostitele, který se skládá z uživatelského jména správce clusteru a veřejného názvu DNS prvního hlavního serveru v clusteru. **Název hostitele** vypadá podobně jako `adminuser@PublicDNSName`. Jako **port** zadejte 2200.

    ![Konfigurace PuTTY 1](media/putty1.png)

4. Vyberte **SSH > Ověřování**. Pro ověření přidejte cestu ke svému souboru privátního klíče (formát .ppk). Můžete použít nástroj typu [PuTTYgen](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html) k vygenerování tohoto souboru z klíč SSH použitého při vytváření clusteru.

    ![Konfigurace PuTTY 2](media/putty2.png)

5. Vyberte **SSH > Tunely** a nakonfigurujte následující přesměrované porty:

    * **Zdrojový port:** Použijte 80 pro DC/OS nebo 2375 pro Swarm.
    * **Cíl:** Použijte localhost:80 pro DC/OS nebo localhost:2375 pro Swarm.

    Následující příklad je nakonfigurován pro DC/OS, ale pro Docker Swarm bude vypadat obdobně.

    > [!NOTE]
    > Při vytváření tohoto tunelu se port 80 nesmí používat.
    > 

    ![Konfigurace PuTTY 3](media/putty3.png)

6. Po dokončení uložte konfiguraci připojení kliknutím na **Relace > Uložit**.

7. K relaci PuTTY se připojíte kliknutím na **Otevřít**. Po připojení je možné v protokolu událostí PuTTY vidět konfiguraci portů.

    ![Protokol událostí PuTTY](media/putty4.png)

Až bude tunel pro DC/OS nakonfigurovaný, budete mít k příslušnému koncovému bodu přístup přes tyto adresy:

* DC/OS: `http://localhost/`
* Marathon: `http://localhost/marathon`
* Mesos: `http://localhost/mesos`

Po dokončení konfigurace tunelu pro Docker Swarm otevřete nastavení systému Windows a nastavte proměnnou prostředí `DOCKER_HOST` na hodnotu `:2375`. Pak budete mít přístup ke clusteru Swarm přes příkazový řádek Dockeru.

## <a name="next-steps"></a>Další kroky
Nasazení a správa kontejnerů ve vašem clusteru:

* [Práce s Azure Container Service a Kubernetes](container-service-kubernetes-ui.md)
* [Práce se službou Azure Container Service a DC/OS](container-service-mesos-marathon-rest.md)
* [Práce s Azure Container Service a Docker Swarm](container-service-docker-swarm.md)




<!--HONumber=Jan17_HO4-->


