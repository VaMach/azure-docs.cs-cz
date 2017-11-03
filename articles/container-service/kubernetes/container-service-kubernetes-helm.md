---
title: "Nasazení kontejnerů s Helm v Azure Kubernetes | Microsoft Docs"
description: "Pomocí nástroje balení Helm nasazení kontejnerů v clusteru s podporou Kubernetes v Azure Container Service"
services: container-service
documentationcenter: 
author: sauryadas
manager: madhana
editor: 
tags: acs, azure-container-service
keywords: 
ms.service: container-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/10/2017
ms.author: saudas
ms.custom: mvc
ms.openlocfilehash: 9dd137de99864fbac2c73400a88956f6afa324db
ms.sourcegitcommit: c5eeb0c950a0ba35d0b0953f5d88d3be57960180
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/24/2017
---
# <a name="use-helm-to-deploy-containers-on-a-kubernetes-cluster"></a>Použít k nasazení kontejnerů v clusteru s podporou Kubernetes Helm

[!INCLUDE [aks-preview-redirect.md](../../../includes/aks-preview-redirect.md)]

[Helm](https://github.com/kubernetes/helm/) je nástroj balení open source, který vám pomůže nainstalovat a spravovat životní cyklus aplikace Kubernetes. Podobně jako u správce balíčku Linux například byt č get a Yum, Helm slouží ke správě Kubernetes grafy, které jsou balíčky předkonfigurované Kubernetes prostředků. Tento článek ukazuje, jak pracovat s Helm na clusteru s podporou Kubernetes nasazené v Azure Container Service.

Helm má dvě součásti: 
* **Helm CLI** je klient, který běží na vašem počítači místně nebo v cloudu  

* **Výšce kormidelní páky** je server, který běží na clusteru Kubernetes a spravuje životní cyklus aplikace Kubernetes 
 
## <a name="prerequisites"></a>Požadavky

* [Vytvoření clusteru s podporou Kubernetes](container-service-kubernetes-walkthrough.md) v Azure Container Service

* [Instalace a konfigurace `kubectl` ](../container-service-connect.md) v místním počítači.

* [Nainstalujte Helm](https://github.com/kubernetes/helm/blob/master/docs/install.md) v místním počítači.

## <a name="helm-basics"></a>Základy Helm 

Chcete-li zobrazit informace o clusteru Kubernetes, že instalujete výšce kormidelní páky a nasazení vašich aplikací, zadejte následující příkaz:

```bash
kubectl cluster-info 
```
![kubectl cluster-info](./media/container-service-kubernetes-helm/clusterinfo.png)
 
Po instalaci Helm nainstalujte výšce kormidelní páky Kubernetes cluster tak, že zadáte následující příkaz:

```bash
helm init --upgrade
```
Po úspěšném dokončení, zobrazí se výstup takto:

![Instalace výšce kormidelní páky](./media/container-service-kubernetes-helm/tiller-install.png)
 
 
 
 
Chcete-li zobrazit dostupné Helm grafy v úložišti, zadejte následující příkaz:

```bash 
helm search 
```

Zobrazí výstup takto:

![Helm vyhledávání](./media/container-service-kubernetes-helm/helm-search.png)
 
Chcete-li aktualizovat grafy získat nejnovější verze, zadejte:

```bash 
helm repo update 
```
## <a name="deploy-an-nginx-ingress-controller-chart"></a>Nasazení grafu řadič aplikace Nginx příjem příchozích dat 
 
Chcete-li nasadit grafu řadič aplikace Nginx příjem příchozích dat, zadejte jeden příkaz:

```bash
helm install stable/nginx-ingress 
```
![Nasazení řadiče příjem příchozích dat](./media/container-service-kubernetes-helm/nginx-ingress.png)

Pokud zadáte `kubectl get svc` Pokud chcete zobrazit všechny služby, které běží na clusteru, uvidíte, jestli řadič příjem příchozích dat je přiřazené IP adresy. (Když probíhá přiřazení, uvidíte `<pending>`. Trvá několik minut na dokončení.) 

Po IP je přiřazená adresa, přejděte na hodnotu externí IP adresu, kterou najdete v části s back-end Nginx. 
 
![Příjem příchozích dat IP adres](./media/container-service-kubernetes-helm/ingress-ip-address.png)


Chcete-li zobrazit seznam grafy nainstalován v clusteru, zadejte:

```bash
helm list 
```

Příkaz k můžete zkrátit `helm ls`.
 
 
 
 
## <a name="deploy-a-mariadb-chart-and-client"></a>Nasazení klienta a graf MariaDB

Teď nasaďte MariaDB graf a MariaDB klienta se můžete připojit k databázi.

Pokud chcete nasadit MariaDB grafu, zadejte následující příkaz:

```bash
helm install --name v1 stable/mariadb
```

kde `--name` je značku používá pro verze.

> [!TIP]
> Pokud se nasazení nezdaří, spusťte `helm repo update` a zkuste to znovu.
>
 
 
Chcete-li zobrazit všechny grafy nasazené v clusteru, zadejte:

```bash 
helm list
```
 
Pokud chcete zobrazit všechna nasazení, které jsou spuštěné v clusteru, zadejte:

```bash
kubectl get deployments 
``` 
 
 
Nakonec spustit pod pro přístup k klienta, zadejte:

```bash
kubectl run v1-mariadb-client --rm --tty -i --image bitnami/mariadb --command -- bash  
``` 
 
 
Pokud chcete připojit ke klientovi, zadejte následující příkaz, nahraďte `v1-mariadb` s názvem vašeho nasazení:

```bash
sudo mysql –h v1-mariadb
```
 
 
Nyní můžete standardní příkazy SQL k vytvoření databáze, tabulek atd. Například `Create DATABASE testdb1;` vytvoří prázdnou databázi. 
 
 
 
## <a name="next-steps"></a>Další kroky

* Další informace o správě Kubernetes grafy, najdete v článku [Helm dokumentaci](https://github.com/kubernetes/helm/blob/master/docs/index.md). 

