---
title: "Načíst kontejnery vyrovnávání v clusteru Azure DC/OS | Microsoft Docs"
description: "Vyrovnávání zatížení v rámci několika kontejnerů na clusteru Azure Container Service DC/OS."
services: container-service
documentationcenter: 
author: rgardler
manager: timlt
editor: 
tags: acs, azure-container-service
keywords: "Kontejnery, mikroslužby, DC/OS, Azure"
ms.service: container-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/02/2017
ms.author: rogardle
ms.custom: mvc
ms.openlocfilehash: 78725c9d23e13d307821a188028ef573d1def038
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="load-balance-containers-in-an-azure-container-service-dcos-cluster"></a>Kontejnery Vyrovnávání zatížení v clusteru služby Azure Container Service DC/OS
V tomto článku jsme zjistit, jak vytvořit interní nástroj v DC/OS spravované Azure Container Service pomocí Marathon-LB. Tato konfigurace umožňuje škálování aplikací vodorovně. Také umožňuje využít výhod agenta veřejné a privátní clustery umístěním nástroje pro vyrovnávání zatížení na vaše kontejnery aplikací v clusteru, privátní a veřejné clusteru. V tomto kurzu jste:

> [!div class="checklist"]
> * Konfigurace služby Vyrovnávání zatížení Marathon
> * Nasazení aplikace pomocí nástroje pro vyrovnávání zatížení
> * Konfigurace a nástroj pro vyrovnávání zatížení Azure

Je třeba cluster služby ACS DC/OS, pokud chcete provést kroky v tomto kurzu. V případě potřeby [tento ukázkový skript](./../kubernetes/scripts/container-service-cli-deploy-dcos.md) můžete vytvořit za vás.

Tento kurz vyžaduje Azure CLI verze 2.0.4 nebo novější. Verzi zjistíte spuštěním příkazu `az --version`. Pokud potřebujete upgrade, přečtěte si téma [Instalace Azure CLI 2.0]( /cli/azure/install-azure-cli). 

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="load-balancing-overview"></a>Přehled Vyrovnávání zatížení

Existují dvě vrstvy Vyrovnávání zatížení v clusteru služby Azure Container Service DC/OS: 

**Azure nástroj pro vyrovnávání zatížení** poskytuje veřejné vstupní body (ty, které koncovým uživatelům přístup k). Azure LB automaticky poskytuje Azure Container Service a je ve výchozím nastavení nakonfigurován tak, aby vystavit port 80 a 443 8080.

**Vyrovnávání zatížení (marathon-lb) Marathon** trasy příchozí požadavky na kontejner instancí, které tyto žádosti o služby. Škály kontejnerů, které poskytují naše webové služby, marathon-lb dynamicky přizpůsobení. Tento nástroj pro vyrovnávání zatížení není dostupné ve výchozím nastavení v kontejneru služby, ale lze snadno nainstalovat.

## <a name="configure-marathon-load-balancer"></a>Konfigurace pro vyrovnávání zatížení Marathon

Marathon Load Balancer se sám dynamicky rekonfiguruje na základě kontejnerů, které jste nasadili. Je také odolný vůči ztrátě kontejneru nebo agenta – Pokud k tomu dojde, restartuje kontejner na jiném místě Apache Mesos a marathon-lb přizpůsobení.

Spusťte následující příkaz pro instalaci nástroje pro vyrovnávání zatížení marathon v clusteru veřejného agenta.

```azurecli-interactive
dcos package install marathon-lb
```

## <a name="deploy-load-balanced-application"></a>Nasazení aplikace skupinu s vyrovnáváním zatížení

Jakmile máme balíček marathon-lb, můžeme nasadit kontejner aplikace, u kterého chceme vyrovnávat zatížení. 

Nejdřív získáte plně kvalifikovaný název domény veřejně vystavené agentů.

```azurecli-interactive
az acs list --resource-group myResourceGroup --query "[0].agentPoolProfiles[0].fqdn" --output tsv
```

Dále vytvořte soubor s názvem *hello web.json* a zkopírujte následující obsah. `HAPROXY_0_VHOST` Popisek je třeba aktualizovat pomocí plně kvalifikovaný název domény agentů DC/OS. 

```json
{
  "id": "web",
  "container": {
    "type": "DOCKER",
    "docker": {
      "image": "yeasy/simple-web",
      "network": "BRIDGE",
      "portMappings": [
        { "hostPort": 0, "containerPort": 80, "servicePort": 10000 }
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
  "labels":{
    "HAPROXY_GROUP":"external",
    "HAPROXY_0_VHOST":"YOUR FQDN",
    "HAPROXY_0_MODE":"http"
  }
}
```

Pomocí rozhraní příkazového řádku DC/OS a spusťte aplikaci. Ve výchozím nastavení nasadí Marathonu aplikace do privátní clusteru. To znamená, že výše nasazení je pouze přístupné přes nástroj pro vyrovnávání zatížení, který je obvykle toto chování žádoucí.

```azurecli-interactive
dcos marathon app add hello-web.json
```

Jakmile je aplikace nasazená, přejděte na plně kvalifikovaný název clusteru agenta zobrazíte skupinu s vyrovnáváním zatížení aplikace.

![Bitové kopie aplikace skupinu s vyrovnáváním zatížení](./media/container-service-load-balancing/lb-app.png)

## <a name="configure-azure-load-balancer"></a>Konfigurace pro vyrovnávání zatížení Azure

Služba Azure Load Balancer ve výchozím nastavení zpřístupňuje porty 80, 8080 a 443. Pokud používáte některý z těchto tří portů (jako my v příkladu výše), není třeba provádět žádnou další akci. Nyní byste měli mít dosáhl agenta nástroj pro vyrovnávání zatížení je plně kvalifikovaný název domény a pokaždé, když aktualizujete, můžete budete použít jeden ze tří webových serverů v kruhového dotazování. 

Pokud používáte jiný port, budete muset přidat pravidlo kruhového dotazování a najít na Vyrovnávání zatížení pro port, který jste použili. To lze udělat z [rozhraní příkazového řádku Azure](../../azure-resource-manager/xplat-cli-azure-resource-manager.md) pomocí příkazů `azure network lb rule create` a `azure network lb probe create`.

## <a name="next-steps"></a>Další kroky

V tomto kurzu jste se dozvěděli o vyrovnávání zatížení v rámci služby ACS se Marathon i Azure Vyrovnávání zatížení, včetně následujících akcí:

> [!div class="checklist"]
> * Konfigurace služby Vyrovnávání zatížení Marathon
> * Nasazení aplikace pomocí nástroje pro vyrovnávání zatížení
> * Konfigurace a nástroj pro vyrovnávání zatížení Azure

Přechodu na v dalším kurzu se dozvíte o integraci Azure storage s DC/OS v Azure.

> [!div class="nextstepaction"]
> [Azure připojit sdílenou složku v clusteru DC/OS](container-service-dcos-fileshare.md)