---
title: "Seznámení se službou Azure Container Service pro Kubernetes | Dokumentace Microsoftu"
description: "Azure Container Service pro Kubernetes zjednodušuje nasazování a správu aplikací založených na kontejnerech v Azure."
services: container-service
documentationcenter: 
author: gabrtv
manager: timlt
editor: 
tags: aks, azure-container-service
keywords: Kubernetes, Docker, Containers, Microservices, Azure
ms.assetid: 
ms.service: container-service
ms.devlang: na
ms.topic: overview
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/13/2017
ms.author: gamonroy
ms.custom: mvc
ms.openlocfilehash: 9fba9fdda3503ec80fede845466858825e3677a5
ms.sourcegitcommit: 732e5df390dea94c363fc99b9d781e64cb75e220
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/14/2017
---
# <a name="introduction-to-azure-container-service-aks"></a>Úvod do Azure Container Service (AKS)

Azure Container Service (AKS) můžete snadno vytvářet, konfigurovat a spravovat cluster virtuálních počítačů, které jsou předem nakonfigurován ke spuštění kontejnerizované aplikací. Díky tomu můžete při nasazování a správě kontejnerových aplikací v Microsoft Azure využívat své stávající dovednosti nebo stavět na rozsáhlých a stále se rozšiřujících odborných znalostech komunity.

Pomocí AKS můžete využít výhod podnikové úrovni funkce Azure, a současně se zachovává přenositelnost aplikace prostřednictvím Kubernetes a formát obrázku Docker.

## <a name="managed-kubernetes-in-azure"></a>Spravované Kubernetes v Azure

AKS snižuje složitost a provozní režie správy clusteru s podporou Kubernetes přesměrováním většinu tuto odpovědnost do Azure. Jako v hostované službě Kubernetes, Azure zpracovává kritické úlohy, jako je stav monitorování a údržby pro vás. Kromě toho platí pouze pro uzly agenta v rámci clusterů, není pro předlohy. Služba se spravuje Kubernetes AKS poskytuje:

> [!div class="checklist"]
> * Automatické upgrady verze Kubernetes a opravy chyb
> * Škálování snadno clusterů
> * Samoopravitelné roviny hostovaného ovládacího prvku (hlavní servery)
> * Finanční úspoře – platit pouze pro spuštění agenta uzly fondu

S Azure zpracování správu uzlů v clusteru AKS už nemusí ručně, provádět mnoho úloh, jako je upgrade clusteru. Protože Azure zpracovává tyto úlohy údržby kritické pro vás, AKS neposkytuje přímý přístup (například pomocí protokolu SSH) do clusteru.

## <a name="using-azure-container-service-aks"></a>Pomocí Azure Container Service (AKS)
Cílem AKS je poskytnout kontejner hostování prostředí pomocí nástroje open source a technologie, které jsou dnes oblíbených mezi zákazníky. Za tímto účelem zveřejňujeme standardní koncové body rozhraní Kubernetes API. S použitím těchto standardních koncových bodů můžete využívat veškerý software, který dokáže komunikovat s clusterem Kubernetes. Můžete například zvolit [kubectl](https://kubernetes.io/docs/user-guide/kubectl-overview/), [helm](https://helm.sh/) nebo [draft](https://github.com/Azure/draft).

## <a name="creating-a-kubernetes-cluster-using-azure-container-service-aks"></a>Vytváření clusteru Kubernetes pomocí Azure Container Service (AKS)
Pokud chcete začít používat AKS, nasazení clusteru AKS s [rozhraní příkazového řádku Azure](./kubernetes-walkthrough.md) nebo prostřednictvím portálu (vyhledávání na webu Marketplace pro **Azure Container Service**). Pokud jste pokročilý uživatel a potřebujete větší kontrolu nad šablonami Azure Resource Manageru, můžete pomocí open source projektu [acs-engine](https://github.com/Azure/acs-engine) sestavit vlastní cluster Kubernetes a nasadit ho přes rozhraní příkazového řádku `az`.

### <a name="using-kubernetes"></a>S použitím Kubernetes
Kubernetes automatizuje nasazení, škálování a správu kontejnerizovaných aplikací. Má bohatou výbavu funkcí, například:
* Automatické balení do kontejnerů
* Samoopravení
* Horizontální škálování
* Zjišťování služeb a vyrovnávání zatížení
* Automatická uvádění a vracení zpět
* Správa tajných kódů a konfigurací
* Orchestrace úložiště
* Spouštění dávek

## <a name="videos"></a>Videa

Azure Container Service (AKS) - Azure pátek, říjen 2017:

> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Container-Orchestration-Simplified-with-Managed-Kubernetes-in-Azure-Container-Service-AKS/player]
>
>

Nástroje pro vývoj a nasazení aplikací na Kubernetes - Azure OpenDev června 2017:

> [!VIDEO https://channel9.msdn.com/Events/AzureOpenDev/June2017/Tools-for-Developing-and-Deploying-Applications-on-Kubernetes/player]
>
>

Další informace o nasazení a správě AKS s AKS rychlý start.

> [!div class="nextstepaction"]
> [Kurz AKS](./kubernetes-walkthrough.md)