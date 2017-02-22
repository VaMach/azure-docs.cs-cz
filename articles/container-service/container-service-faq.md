---
title: "Azure Container Service – nejčastější dotazy | Dokumentace Microsoftu"
description: "Odpovědi na nejčastější dotazy týkající se Azure Container Service, služby, která zjednodušuje vytváření, konfiguraci a správu clusterů virtuálních počítačů pro spouštění aplikací typu kontejner Docker."
services: container-service
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: acs, azure-container-service
keywords: "Docker, kontejnery, mikroslužby, Mesos, Azure, Kubernetes"
ms.assetid: 
ms.service: container-service
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/10/2017
ms.author: danlep
translationtype: Human Translation
ms.sourcegitcommit: 3bb83f231d16819e5f5da7edbc9fc3f38baff011
ms.openlocfilehash: b0c5efa595b0377d7ae2d936d0394667356d18c9


---
# <a name="frequently-asked-questions-azure-container-service"></a>Nejčastější dotazy: Azure Container Service


## <a name="orchestrators"></a>Orchestrátory

### <a name="which-container-orchestrators-do-you-support-on-azure-container-service"></a>Jaké orchestrátory kontejneru podporujete ve službě Azure Container Service? 

Podporuje se open source DC/OS, Docker Swarm a Kubernetes. Zatímco podpora DC/OS a Docker Swarm je všeobecně dostupná, podpora pro Kubernetes je aktuálně ve verzi Preview. Další informace najdete v tématu [Přehled](container-service-intro.md).
 
### <a name="do-you-support-swarm-mode"></a>Podporujete režim Swarm? 

Režim Swarm v současné době není podporován, ale do budoucna se plánuje. 

### <a name="does-azure-container-service-support-windows-containers"></a>Podporuje služba Azure Container Service kontejnery Windows?  

Aktuálně jsou podporovány kontejnery Linux. Podpora kontejnerů Windows s orchestrátory DC/OS, Docker Swarm a Kubernetes se plánuje do budoucna. 

### <a name="do-you-recommend-a-specific-orchestrator-in-azure-container-service"></a>Doporučujete ve službě Azure Container Service použití konkrétního orchestrátoru? 
Obecně nedoporučujeme konkrétní orchestrator. Pokud máte zkušenosti s některým z podporovaných orchestrátorů, můžete je využít ve službě Azure Container Service. Trendy v datech nicméně naznačují, že systém DC/OS se v produkčním prostředí osvědčil pro úlohy s velkým objemem dat a úlohy IoT, Kubernetes se skvěle hodí pro úlohy nativní pro cloud a Docker Swarm je známý svou integrací s nástroji Dockeru a jednoduchostí osvojování.

V závislosti na scénáři můžete také vytvořit a spravovat vlastní řešení kontejnerů pomocí dalších služeb Azure. Mezi tyto služby patří [Virtual Machines](../virtual-machines/virtual-machines-linux-azure-overview.md), [Service Fabric](../service-fabric/service-fabric-overview.md), [Web Apps](../app-service-web/app-service-web-overview.md) a [Batch](../batch/batch-technical-overview.md).  

### <a name="what-is-the-difference-between-azure-container-service-and-acs-engine"></a>Jaký je rozdíl mezi službou Azure Container Service a modulem ACS? 
Azure Container Service je služba Azure, na kterou se vztahuje smlouva SLA, s funkcemi na webu Azure Portal, nástroji příkazového řádku Azure a rozhraními API Azure. Služba umožňuje rychlou implementaci a správu clusterů se standardními nástroji pro orchestraci kontejnerů a poskytuje poměrně málo možností konfigurace. 

[Modul ACS](http://github.com/Azure/acs-engine) je open source projekt, který umožňuje zkušeným uživatelům přizpůsobení konfigurace clusteru na všech úrovních. Možnost změny konfigurace infrastruktury i softwaru znamená, že pro modul ACS nenabízíme žádnou smlouvu SLA. Podpora probíhá prostřednictvím open source projektu na GitHubu, nikoli přes oficiální kanály Microsoftu. 

## <a name="cluster-management"></a>Správa clusteru

### <a name="how-do-i-create-ssh-keys-for-my-cluster"></a>Jak pro cluster vytvořím klíče SSH?

Můžete použít standardní nástroje vašeho operačního systému a vytvořit pro cluster pár veřejného a privátního klíče SSH RSA pro ověřování u virtuálních počítačů. Pokyny najdete v doprovodných materiálech k [OS X a Linuxu](../virtual-machines/virtual-machines-linux-mac-create-ssh-keys.md) nebo [Windows](../virtual-machines/virtual-machines-linux-ssh-from-windows.md). 

Pokud k nasazení clusteru služby Container Service použijete [příkazy Azure CLI 2.0 (Preview)](container-service-create-acs-cluster-cli.md), je možné klíče SSH pro cluster vygenerovat automaticky.

### <a name="how-do-i-create-a-service-principal-for-my-kubernetes-cluster"></a>Jak vytvořím instanční objekt pro cluster Kubernetes?

K vytvoření clusteru Kubernetes ve službě Azure Container Service je potřeba také ID a heslo instančního objektu služby Azure Active Directory. Další informace najdete v tématu [O instančním objektu pro cluster Kubernetes](container-service-kubernetes-service-principal.md).


Pokud k nasazení clusteru Kubernetes použijete [příkazy Azure CLI 2.0 (Preview)](container-service-create-acs-cluster-cli.md), je možné přihlašovací údaje instančního objektu pro cluster vygenerovat automaticky.


### <a name="how-do-i-increase-the-number-of-masters-after-a-cluster-is-created"></a>Jak se dá po vytvoření clusteru zvýšit počet hlavních serverů? 
Jakmile je cluster vytvořený, počet hlavních serverů je pevně daný a není možné jej změnit. Při vytváření clusteru byste v ideálním případě měli zvolit tři nebo pět hlavních serverů pro zajištění vysoké dostupnosti.

> [!NOTE]
> Ve verzi Preview může cluster Kubernetes ve službě Azure Container Service obsahovat pouze jeden hlavní server.
>

### <a name="how-do-i-increase-the-number-of-agents-after-a-cluster-is-created"></a>Jak se dá po vytvoření clusteru zvýšit počet agentů? 
Počet agentů v clusteru můžete škálovat pomocí webu Azure Portal nebo nástrojů příkazového řádku. Viz [Škálování clusteru Azure Container Service](container-service-scale.md).

> [!NOTE]
> Ve verzi Preview má cluster Kubernetes ve službě Azure Container Service pevný počet agentů. 
>

### <a name="what-are-the-urls-of-my-masters-and-agents"></a>Jaké jsou adresy URL hlavních serverů a agentů? 
Adresy URL prostředků clusteru ve službě Azure Container Service vycházejí z předpony názvu DNS, kterou jste zadali, a názvu oblasti Azure, kterou jste vybrali pro nasazení. Například plně kvalifikovaný název domény (FQDN) hlavního uzlu je v tomto formátu:

``` 
DNSnamePrefix.AzureRegion.cloudapp.azure.net
```

Běžně používané adresy URL pro váš cluster najdete na webu Azure Portal, v Průzkumníku prostředků Azure a dalších nástrojích Azure.
 
### <a name="where-do-i-find-the-ssh-connection-string-to-my-cluster"></a>Kde najdu připojovací řetězec SSH k mému clusteru?

Připojovací řetězec najdete na webu Azure Portal nebo pomocí nástrojů příkazového řádku Azure. 

1. Na portálu přejděte do skupiny prostředků požadovaného nasazení clusteru.  

2. Klikněte na **Přehled** a potom v části **Základní údaje** klikněte na **Nasazení**. 

3. V okně **Historie nasazení** klikněte na nasazení, jehož název začíná na **microsoft-acs** a následuje datem nasazení. Příklad: microsoft-acs-201701310000.  

4. Na stránce **Souhrn** v části **Výstupy** je několik odkazů na cluster <provided></provided>. **SSHMaster0** představuje připojovací řetězec SSH k prvnímu hlavnímu serveru ve vašem clusteru služby Container Service. 

Jak bylo uvedeno výše, plně kvalifikovaný název domény hlavního serveru můžete najít také pomocí nástrojů Azure. Vytvořte připojení SSH k hlavnímu serveru pomocí plně kvalifikovaného názvu domény hlavního serveru a uživatelského jména, které jste zadali při vytváření clusteru. Například:

```bash
ssh userName@masterFQDN –A –p 22 
```

Další informace najdete v [Připojení ke clusteru služby Azure Container Service](container-service-connect.md).




## <a name="next-steps"></a>Další kroky

* [Další informace](container-service-intro.md) o službě Azure Container Service.
* Nasazení clusteru služby Container Service pomocí [portálu](container-service-deployment.md) nebo [Azure CLI 2.0 (Preview)](container-service-create-acs-cluster-cli.md).


<!--HONumber=Feb17_HO3-->


