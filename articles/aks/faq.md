---
title: "Nejčastější dotazy k Azure Container Service"
description: "Poskytuje odpovědi na některé běžné otázky o Azure Container Service."
services: container-service
author: neilpeterson
manager: timlt
ms.service: container-service
ms.topic: article
ms.date: 2/01/2018
ms.author: nepeters
ms.openlocfilehash: 73c49510512c9148f4fee98423b14770fa8602b9
ms.sourcegitcommit: b32d6948033e7f85e3362e13347a664c0aaa04c1
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/13/2018
---
# <a name="frequently-asked-questions-about-azure-container-service-aks"></a>Nejčastější dotazy o Azure Container Service (AKS)

Tento článek adresy časté otázky o Azure Container Service (AKS).

## <a name="which-azure-regions-will-have-azure-container-service-aks"></a>Které oblasti Azure budou mít Azure Container Service (AKS)? 

- Střední Kanada 
- Východní Kanada 
- Střed USA 
- Východ USA 
- Jihovýchodní Asie 
- Západní Evropa 
- Západní USA 2 

## <a name="when-will-additional-regions-be-added"></a>Pokud bude přidán další oblasti? 

Další oblasti jsou přidány jako rostoucími.

## <a name="are-security-updates-applied-to-aks-nodes"></a>Platí pro uzly AKS aktualizace zabezpečení? 

Opravy zabezpečení operačního systému se použijí pro uzly v clusteru na noční plánu, ale neprovádí se restartovat počítač. V případě potřeby může být uzly restartovat prostřednictvím portálu nebo rozhraní příkazového řádku Azure. Při upgradu clusteru, se používá nejnovější image Ubuntu a aplikují všechny opravy zabezpečení (s restartování).

## <a name="do-you-recommend-customers-use-acs-or-akss"></a>Zákazníkům doporučujeme použití služby ACS nebo AKSs? 

Vzhledem k tomu, že Azure Container Service (AKS) bude GA později, doporučujeme, aby sestavení na PoC, vývoje a testování clusterů v AKS ale produkční clusterů v Kubernetes služby ACS.  

## <a name="when-will-acs-be-deprecated"></a>Pokud bude ACS zastaralá? 

Služba ACS přestanou přibližně v okamžiku, kdy AKS stane všeobecné Budete mít dobu 12 měsíců po tomto datu migrace clusterů do AKS. Během 12 měsíců můžete spustit všechny operace služby ACS.

## <a name="does-aks-support-node-autoscaling"></a>Podporuje AKS automatické škálování uzlu? 

Automatické škálování uzlu není podporován, ale je plánovaná. Můžete chtít podívejte se na to open source [automatické škálování implementace][auto-scaler].

## <a name="why-are-two-resource-groups-created-with-aks"></a>Proč jsou dvě skupiny prostředků vytvořené pomocí AKS? 

Každý cluster Azure Container Service (AKS) je součástí dvě skupiny prostředků. První je vytvořené vámi a obsahuje pouze AKS prostředek. Druhé skupině prostředků se automaticky vytvoří během nasazení a obsahuje všechny infrastruktury prostředků clusteru, jako je například virtuální počítače, sítě a prostředky úložiště. Tato skupina prostředků je vytvořen pro vyčištění snadno prostředků. 

Skupina prostředků vytvořen automaticky, má název, podobně jako:

```
MC_myResourceGRoup_myAKSCluster_eastus
```

Při přidávání prostředků Azure pro použití s Kubernetes clusteru, například účty úložiště nebo vyhrazené veřejné IP adresy, tyto prostředky je nutné vytvořit v automaticky vytvořit skupinu prostředků.   

## <a name="is-azure-key-vault-integrated-with-aks"></a>Azure Key Vault integrovat AKS? 

Ne, není ale je plánovaná této integrace. Mezitím si můžete vyzkoušet následující řešení z [Hexadite][hexadite]. 

<!-- LINKS - external -->
[auto-scaler]: https://github.com/kubernetes/autoscaler
[hexadite]: https://github.com/Hexadite/acs-keyvault-agent  