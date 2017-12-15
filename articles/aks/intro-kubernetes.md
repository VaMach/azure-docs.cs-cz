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
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/14/2017
---
# <a name="introduction-to-azure-container-service-aks"></a>Seznámení se službou Azure Container Service (AKS)

Služba Azure Container Service (AKS) zjednodušuje vytváření, konfiguraci a správu clusteru virtuálních počítačů, ve kterých je předem nakonfigurované spouštění kontejnerizovaných aplikací. Díky tomu můžete při nasazování a správě kontejnerových aplikací v Microsoft Azure využívat své stávající dovednosti nebo stavět na rozsáhlých a stále se rozšiřujících odborných znalostech komunity.

Díky používání AKS můžete využívat výhody funkcí Azure na podnikové úrovni, a přitom zachovávat přenositelnost aplikací prostřednictvím Kubernetes a formátu image Dockeru.

## <a name="managed-kubernetes-in-azure"></a>Managed Kubernetes v Azure

AKS zjednodušuje správu clusteru Kubernetes a snižuje provozní režii s tím spojenou díky přenášení většiny zodpovědnosti na Azure. Jako hostovaná služba Kubernetes se za vás Azure stará o důležité úlohy, jako je monitorování stavu a údržba. Navíc platíte pouze za agentské uzly v rámci vašich clusterů, a ne za hlavní uzly. AKS jako spravovaná služba Kubernetes poskytuje:

> [!div class="checklist"]
> * Automatické upgrady verzí Kubernetes a opravy chyb
> * Snadné škálování clusterů
> * Hostovanou rovinu řízení s automatickými opravami (hlavní uzly)
> * Úspory nákladů – platíte pouze za spuštěné uzly fondu agentů

Díky tomu, že se Azure stará o správu uzlů ve vašem clusteru AKS, už řadu úloh, například upgrady clusteru, nemusíte provádět ručně. Vzhledem k tomu, že se Azure stará o tyto důležité úlohy údržby za vás, neposkytuje AKS přímý přístup (například pomocí SSH) ke clusteru.

## <a name="using-azure-container-service-aks"></a>Použití služby Azure Container Service (AKS)
Cílem AKS je poskytnout hostitelské prostředí pro kontejnery díky používání nástrojů a technologií open source, které jsou dnes mezi zákazníky oblíbené. Za tímto účelem zveřejňujeme standardní koncové body rozhraní Kubernetes API. S použitím těchto standardních koncových bodů můžete využívat veškerý software, který dokáže komunikovat s clusterem Kubernetes. Můžete například zvolit [kubectl](https://kubernetes.io/docs/user-guide/kubectl-overview/), [helm](https://helm.sh/) nebo [draft](https://github.com/Azure/draft).

## <a name="creating-a-kubernetes-cluster-using-azure-container-service-aks"></a>Vytvoření clusteru Kubernetes pomocí služby Azure Container Service (AKS)
Pokud chcete začít používat AKS, nasaďte cluster AKS pomocí [Azure CLI](./kubernetes-walkthrough.md) nebo přes portál (na Marketplace vyhledejte **Azure Container Service**). Pokud jste pokročilý uživatel a potřebujete větší kontrolu nad šablonami Azure Resource Manageru, můžete pomocí open source projektu [acs-engine](https://github.com/Azure/acs-engine) sestavit vlastní cluster Kubernetes a nasadit ho přes rozhraní příkazového řádku `az`.

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

Azure Container Service (AKS) – Azure Friday, říjen 2017:

> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Container-Orchestration-Simplified-with-Managed-Kubernetes-in-Azure-Container-Service-AKS/player]
>
>

Nástroje pro vývoj a nasazování aplikací v Kubernetes – Azure OpenDev, červen 2017:

> [!VIDEO https://channel9.msdn.com/Events/AzureOpenDev/June2017/Tools-for-Developing-and-Deploying-Applications-on-Kubernetes/player]
>
>

Další informace o nasazení a správě AKS najdete v rychlých startech pro AKS.

> [!div class="nextstepaction"]
> [Kurz AKS](./kubernetes-walkthrough.md)