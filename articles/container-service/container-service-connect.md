---
title: "Připojení ke clusteru Azure Container Service | Dokumentace Microsoftu"
description: "Ke clusteru Azure Container Service se můžete připojit pomocí tunelu SSH."
services: container-service
documentationcenter: 
author: rgardler
manager: timlt
editor: 
tags: acs, azure-container-service
keywords: "Docker, Kontejnery, mikroslužby, DC/OS, Azure"
ms.assetid: ff8d9e32-20d2-4658-829f-590dec89603d
ms.service: container-service
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/13/2016
ms.author: rogardle
translationtype: Human Translation
ms.sourcegitcommit: bcc2d3468c8a560105aa2c2feb0d969ec3cccdcb
ms.openlocfilehash: 5296586b9266f432042f847f4dff9e6ff62ebc8b


---
# <a name="connect-to-an-azure-container-service-cluster"></a>Připojení ke clusteru Azure Container Service
Clustery DC/OS, Kubernetes a Docker Swarm nasazené v Azure Container Service zpřístupňují koncové body REST.  V případě Kubernetes je tento koncový bod bezpečně zpřístupněn na internetu a můžete k němu přímo přistupovat z libovolného počítače připojeného k internetu. V případě DC/OS a Docker Swarm je nutné pro možnost bezpečného připojení ke koncovému bodu REST vytvořit tunel SSH. Jednotlivá připojení jsou popsána níže.

> [!NOTE]
> Podpora pro Kubernetes je v Azure Container Service momentálně ve verzi preview.
>

## <a name="connecting-to-a-kubernetes-cluster"></a>Připojení ke clusteru Kubernetes
Pokud se chcete připojit ke clusteru Kubernetes, musíte mít nainstalovaný nástroj příkazového řádku `kubectl`.  Nejjednodušším způsobem, jak tento nástroj nainstalovat, je použít nástroj příkazového řádku Azure 2.0 `az`.

```console
az acs kubernetes install cli [--install-location=/some/directory]
```

Případně si můžete klienta stáhnout přímo ze [stránky vydaných verzí](https://github.com/kubernetes/kubernetes/blob/master/CHANGELOG.md#downloads-for-v146).

Jakmile budete mít `kubectl` nainstalovaný, je třeba, abyste na svůj počítač zkopírovali přihlašovací údaje clusteru.  Nejjednodušší způsob je opět pomocí nástroje příkazového řádku `az`:

```console
az acs kubernetes get-credentials --dns-prefix=<some-prefix> --location=<some-location>
```

Tento příkaz stáhne přihlašovací údaje clusteru do složky `$HOME/.kube/config`, kde `kubectl` očekává, že budou umístěné.

Případně můžete použít `scp` a bezpečně zkopírovat soubor ze složky `$HOME/.kube/config` na hlavním virtuálním počítači do svého místního počítače.

```console
mkdir $HOME/.kube/config
scp azureuser@<master-dns-name>:.kube/config $HOME/.kube/config
```

Pokud jste v systému Windows, budete muset použít Bash na Ubuntu ve Windows nebo nástroj PuTTY „pscp“.

Jakmile bude `kubectl` nakonfigurovaný, můžete ho otestovat uvedením uzlů ve vašem clusteru:

```console
kubectl get nodes
```

Nakonec můžete zobrazit řídicí panel Kubernetes. Nejprve je potřeba provést:

```console
kubectl proxy
```

Uživatelské rozhraní Kubernetes je nyní k dispozici na adrese: http://localhost:8001/ui

Další pokyny najdete v tématu [Rychlé představení Kubernetes](http://kubernetes.io/docs/user-guide/quick-start/).

## <a name="connecting-to-a-dcos-or-swarm-cluster"></a>Připojení ke clusteru DC/OS nebo Swarm

Clustery DC/OS a Docker Swarm nasazené v Azure Container Service zpřístupňují koncové body REST. Tyto koncové body ale nejsou k dispozici pro vnější svět. Pokud chcete tyto koncové body spravovat, je nutné vytvořit tunel Secure Shell (SSH). Po vytvoření tunelu SSH můžete proti koncovým bodům clusteru spouštět příkazy a na svém vlastním systému si můžete přes prohlížeč zobrazit uživatelské rozhraní clusteru. Tento dokument vás provede vytvořením tunelu SSH z Linuxu, OS X a Windows.

> [!NOTE]
> Se systémem pro správu clusteru je možné vytvořit relace SSH. To ale nedoporučujeme. Práce přímo v systému pro správu představuje riziko neúmyslných změn konfigurace.   
> 
> 

## <a name="create-an-ssh-tunnel-on-linux-or-os-x"></a>Vytvoření tunelu SSH v Linuxu a OS X
První věc, kterou je nutné udělat, když vytváříte tunel SSH v Linuxu nebo OS X, je nalezení veřejného názvu DNS hlavních serverů s vyrovnáváním zatížení. To provedete tak, že rozbalíte skupinu prostředků, aby se zobrazily všechny prostředky. Najděte a vyberte veřejnou IP adresu hlavního serveru. Tím se otevře okno, které obsahuje informace o veřejné IP adrese, včetně názvu DNS. Uložte si tento název pro pozdější použití. <br />

![Veřejný název DNS](media/pubdns.png)

Nyní otevřete prostředí a spusťte následující příkaz, kde:

**PORT** je port koncového bodu, který chcete zveřejnit. Pro Swarm je to 2375. Pro DC/OS použijte port 80.  
**USERNAME** je uživatelské jméno zadané ve chvíli, kdy jste nasadili cluster.  
**DNSPREFIX** je předpona DNS zadaná ve chvíli, kdy jste nasadili cluster.  
**REGION** je oblast, ve které je umístěna skupina prostředků.  
**PATH_TO_PRIVATE_KEY** [NEPOVINNÉ] je cesta k privátnímu klíči, který odpovídá veřejnému klíči zadanému při vytváření clusteru kontejnerové služby. Tuto možnost použijte spolu s příznakem -i.

```bash
ssh -L PORT:localhost:PORT -f -N [USERNAME]@[DNSPREFIX]mgmt.[REGION].cloudapp.azure.com -p 2200
```
> Port pro připojení SSH je 2200, nikoli standardní port 22.
> 
> 

## <a name="dcos-tunnel"></a>Tunel DC/OS
Pokud chcete otevřít tunel ke koncovým bodům souvisejícím s DC/OS, spusťte příkaz podobný tomuto:

```bash
sudo ssh -L 80:localhost:80 -f -N azureuser@acsexamplemgmt.japaneast.cloudapp.azure.com -p 2200
```

Nyní můžete ke koncovým bodům souvisejícím s DC/OS přistupovat přes tyto adresy:

* DC/OS: `http://localhost/`
* Marathon: `http://localhost/marathon`
* Mesos: `http://localhost/mesos`

Obdobně můžete přes tento tunel kontaktovat rozhraní REST API pro každou z aplikací.

## <a name="swarm-tunnel"></a>Tunel Swarm
Pokud chcete otevřít tunel ke koncovému bodu Swarm, spusťte příkaz podobný tomuto:

```bash
ssh -L 2375:localhost:2375 -f -N azureuser@acsexamplemgmt.japaneast.cloudapp.azure.com -p 2200
```

Nyní můžete nastavit proměnnou prostředí vašeho DOCKER_HOST následujícím způsobem. Můžete nadále používat rozhraní příkazového řádku Docker (CLI) jako za normálních okolností.

```bash
export DOCKER_HOST=:2375
```

## <a name="create-an-ssh-tunnel-on-windows"></a>Vytvoření tunelu SSH ve Windows
Tunely SSH je ve Windows možné vytvořit několika způsoby. Tento dokument popisuje, jak k tomu použít PuTTY.

Stáhněte si PuTTY do Windows a aplikaci spusťte.

Zadejte název hostitele, který se skládá z uživatelského jména správce clusteru a veřejného názvu DNS prvního hlavního serveru v clusteru. **Název hostitele** bude vypadat takto: `adminuser@PublicDNS`. Jako **port** zadejte 2200.

![Konfigurace PuTTY 1](media/putty1.png)

Vyberte **SSH** a **ověřování**. Pro ověření přidejte svůj soubor privátního klíče.

![Konfigurace PuTTY 2](media/putty2.png)

Vyberte **tunely** a nakonfigurujte následující přesměrované porty:

* **Zdrojový port:** Vaše předvolba – použijte 80 pro DC/OS nebo 2375 pro Swarm.
* **Cíl:** Použijte localhost:80 pro DC/OS nebo localhost:2375 pro Swarm.

Následující příklad je nakonfigurován pro DC/OS, ale pro Docker Swarm bude vypadat obdobně.

> [!NOTE]
> Při vytváření tohoto tunelu se port 80 nesmí používat.
> 
> 

![Konfigurace PuTTY 3](media/putty3.png)

Po dokončení uložte konfiguraci připojení a připojte relaci PuTTY. Po připojení je možné v protokolu událostí PuTTY vidět konfiguraci portů.

![Protokol událostí PuTTY](media/putty4.png)

Až bude tunel pro DC/OS nakonfigurován, budete mít k souvisejícímu koncovému bodu přístup přes tyto adresy:

* DC/OS: `http://localhost/`
* Marathon: `http://localhost/marathon`
* Mesos: `http://localhost/mesos`

Když nakonfigurujete tunel pro Docker Swarm, budete mít ke clusteru Swarm přístup přes rozhraní příkazového řádku Dockeru. Nejdřív bude nutné nastavit proměnnou prostředí Windows s názvem `DOCKER_HOST` na hodnotu ` :2375`.

## <a name="next-steps"></a>Další kroky
Nasazení a správa kontejnerů pomocí DC/OS nebo Swarmu:

* [Práce se službou Azure Container Service a DC/OS](container-service-mesos-marathon-rest.md)
* [Práce s Azure Container Service a Docker Swarm](container-service-docker-swarm.md)




<!--HONumber=Dec16_HO3-->


