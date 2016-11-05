---
title: Správa kontejnerů Azure Container Service prostřednictvím webového uživatelského rozhraní | Microsoft Docs
description: Využijte webového uživatelského rozhraní Marathon k nasazení kontejnerů do clusteru Azure Container Service.
services: container-service
documentationcenter: ''
author: neilpeterson
manager: timlt
editor: ''
tags: acs, azure-container-service
keywords: Docker, kontejnery, mikroslužby, Mesos, Azure

ms.service: container-service
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/19/2016
ms.author: nepeters

---
# Správa kontejnerů prostřednictvím webového uživatelského rozhraní
DC/OS poskytuje prostředí pro nasazování a škálování clusterových úloh a zároveň poskytuje abstrakci používaného hardwaru. Nad DC/OS je rozhraní, které spravuje plánování a provádění výpočetních úloh.

Pro mnoho populárních úloh jsou k dispozici rozhraní, tento dokument ale popisuje, jak jde nasazení kontejnerů vytvořit a škálovat pomocí Marathonu. Než si projdete tyto příklady, budete potřebovat cluster DC/OS nakonfigurovaný v Azure Container Service. Kromě toho je nutné mít možnost se k tomuto clusteru připojit vzdáleně. Další informace k těmto záležitostem najdete v těchto článcích:

* [Nasazení clusteru Azure Container Service](container-service-deployment.md)
* [Připojení ke clusteru Azure Container Service](container-service-connect.md)

## Zkoumáme uživatelské rozhraní DC/OS
Po vytvoření tunelu Secure Shell (SSH) přejděte na http://localhost/. Načte se webové uživatelské rozhraní DC/OS a zobrazí se informace o clusteru, například využité prostředky, aktivní agenti a spuštěné služby.

![Uživatelské rozhraní DC/OS](media/dcos/dcos2.png)

## Zkoumáme uživatelské rozhraní Marathon
Pokud si chcete zobrazit uživatelské rozhraní Marathon, přejděte na http://localhost/Marathon. Prostřednictvím této obrazovky můžete spustit nový kontejner nebo jinou aplikaci z clusteru Azure Container Service na bázi DC/OS. Taky uvidíte informace o spuštěných kontejnerech a aplikacích.  

![Uživatelské rozhraní Marathon](media/dcos/dcos3.png)

## Nasazení kontejneru formátovaného Dockerem
Pokud chcete pomocí Marathonu nasadit nový kontejner, klikněte na tlačítko **Create Application** (Vytvořit aplikaci) a zadejte do formuláře následující informace:

| Pole | Hodnota |
| --- | --- |
| ID |nginx |
| Image |nginx |
| Network (Síť) |Bridged (Zapojeno do mostu) |
| Host Port (Port hostitele) |80 |
| Protocol (Protokol) |TCP |

![Nová aplikace uživatelského rozhraní – obecné](media/dcos/dcos4.png)

![Nová aplikace uživatelského rozhraní – kontejner Docker](media/dcos/dcos5.png)

![Nová aplikace uživatelského rozhraní – porty a zjišťování služby](media/dcos/dcos6.png)

Pokud chcete staticky namapovat port kontejneru na port agenta, budete muset použít režim JSON. Provedete to tak, že pomocí přepínače přepnete průvodce novou aplikací na **JSON Mode** (Režim JSON). Potom v části `portMappings` definice aplikace zadejte následující. Tento příklad namapuje port číslo 80 kontejneru na port číslo 80 agenta DC/OS. Po provedení této změny můžete režim JSON v průvodci opět vypnout.

```none
"hostPort": 80,
```

![Nová aplikace uživatelského rozhraní – příklad u portu číslo 80](media/dcos/dcos13.png)

Cluster DC/OS se nasazuje se sadou privátních a veřejných agentů. Pokud chcete, aby měl cluster přístup k aplikacím z internetu, musíte aplikace nasadit s použitím veřejného agenta. Uděláte to následovně: V průvodci novou aplikací vyberete kartu **Optional** (Volitelné) a do **Accepted Resource Roles** (Povolené role prostředků) zadáte **slave_public**.

![Nová aplikace uživatelského rozhraní – nastavení veřejného agenta](media/dcos/dcos14.png)

Když se vrátíte na hlavní stránku Marathonu, uvidíte stav nasazení daného kontejneru.

![Hlavní stránka uživatelského rozhraní Marathon – stav nasazení kontejneru](media/dcos/dcos7.png)

Když přepnete zpět do webového uživatelského rozhraní DC/OS (http://localhost/), uvidíte, že úloha (v tomto případě kontejner formátovaný Dockerem) je spuštěná na clusteru DC/OS.

![Webové uživatelské rozhraní DC/OS – úloha spuštěná na clusteru](media/dcos/dcos8.png)

Taky uvidíte uzel clusteru, na kterém je úloha spuštěná.

![Webové uživatelské rozhraní DC/OS – uzel clusteru úlohy](media/dcos/dcos9.png)

## Škálování kontejnerů
Pomocí uživatelského rozhraní Marathon můžete škálovat počet instancí kontejneru. Jak na to? Přejděte na stránku **Marathon**, vyberte kontejner, který chcete škálovat, a klikněte na tlačítko **Scale** (Škálovat). V dialogu **Scale Application** (Škálování aplikace) zadejte počet instancí kontejneru, který vám vyhovuje, a vyberte **Scale Application** (Škálovat aplikaci).

![Uživatelské rozhraní Marathon – dialog Scale Application (Škálování aplikace)](media/dcos/dcos10.png)

Jakmile bude operace škálování dokončena, zobrazí se více instancí té samé úlohy u různých agentů DC/OS.

![Řídicí panel webového uživatelského rozhraní DC/OS – úloha rozdělená mezi různé agenty](media/dcos/dcos11.png)

![Webové uživatelské rozhraní DC/OS – uzly](media/dcos/dcos12.png)

## Další kroky
* [Práce s DC/OS a rozhraním API Marathonu](container-service-mesos-marathon-rest.md)

Podrobná prohlídka služby Azure Container Service s Mesos

> [AZURE.VIDEO] azurecon-2015-deep-dive-on-the-azure-container-service-with-mesos]
> 
> 

<!--HONumber=Sep16_HO3-->


