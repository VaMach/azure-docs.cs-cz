---
title: Připojení ke clusteru Azure Container Service | Microsoft Docs
description: Ke clusteru Azure Container Service se můžete připojit pomocí tunelu SSH.
services: container-service
documentationcenter: ''
author: rgardler
manager: timlt
editor: ''
tags: acs, azure-container-service
keywords: Docker, Kontejnery, mikroslužby, DC/OS, Azure

ms.service: container-service
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/13/2016
ms.author: rogardle

---
# Připojení ke clusteru Azure Container Service
Clustery DC/OS a Docker Swarm nasazené v Azure Container Service zpřístupňují koncové body REST. Tyto koncové body ale nejsou k dispozici pro vnější svět. Pokud chcete tyto koncové body spravovat, je nutné vytvořit tunel Secure Shell (SSH). Po vytvoření tunelu SSH můžete proti koncovým bodům clusteru spouštět příkazy a na svém vlastním systému si můžete přes prohlížeč zobrazit uživatelské rozhraní clusteru. Tento dokument vás provede vytvořením tunelu SSH z Linuxu, OS X a Windows.

> [!NOTE]
> Se systémem pro správu clusteru je možné vytvořit relace SSH. To ale nedoporučujeme. Práce přímo v systému pro správu představuje riziko neúmyslných změn konfigurace.   
> 
> 

## Vytvoření tunelu SSH v Linuxu a OS X
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

## Tunel DC/OS
Pokud chcete otevřít tunel ke koncovým bodům souvisejícím s DC/OS, spusťte příkaz podobný tomuto:

```bash
sudo ssh -L 80:localhost:80 -f -N azureuser@acsexamplemgmt.japaneast.cloudapp.azure.com -p 2200
```

Nyní můžete ke koncovým bodům souvisejícím s DC/OS přistupovat přes tyto adresy:

* DC/OS: `http://localhost/`
* Marathon: `http://localhost/marathon`
* Mesos: `http://localhost/mesos`

Obdobně můžete přes tento tunel kontaktovat rozhraní REST API pro každou z aplikací.

## Tunel Swarm
Pokud chcete otevřít tunel ke koncovému bodu Swarm, spusťte příkaz podobný tomuto:

```bash
ssh -L 2375:localhost:2375 -f -N azureuser@acsexamplemgmt.japaneast.cloudapp.azure.com -p 2200
```

Nyní můžete nastavit proměnnou prostředí vašeho DOCKER_HOST následujícím způsobem. Můžete nadále používat rozhraní příkazového řádku Docker (CLI) jako za normálních okolností.

```bash
export DOCKER_HOST=:2375
```

## Vytvoření tunelu SSH ve Windows
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

## Další kroky
Nasazení a správa kontejnerů pomocí DC/OS nebo Swarmu:

* [Práce s Azure Container Service a DC/OS](container-service-mesos-marathon-rest.md)
* [Práce s Azure Container Service a Docker Swarm](container-service-docker-swarm.md)

<!--HONumber=Sep16_HO3-->


