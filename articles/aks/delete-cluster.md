---
title: "Odstranění clusteru služby Azure Container Service (AKS)"
description: "Odstranění a AKS clusteru pomocí rozhraní CLI nebo Azure portálu."
services: container-service
author: neilpeterson
manager: timlt
ms.service: container-service
ms.topic: article
ms.date: 2/05/2018
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: fd5d3bd7cb2ce5d8f0999710bfeb6a9514231d8d
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/09/2018
---
# <a name="delete-an-azure-container-service-aks-cluster"></a>Odstranění clusteru služby Azure Container Service (AKS)

Při odstraňování clusteru Azure Container Service, skupinu prostředků, ve kterém byl nasazen cluster zůstane, ale jsou odstraněny všechny AKS související prostředky. Tento dokument ukazuje postup odstranění clusteru služby AKS pomocí rozhraní příkazového řádku Azure a Azure portal. 

Kromě odstranění clusteru, skupinu prostředků, ve kterém byl nasazen se dá odstranit, která také odstraňuje AKS clusteru.

## <a name="azure-cli"></a>Azure CLI

Použití [odstranit az aks] [ az-aks-delete] příkaz k odstranění AKS clusteru.

```azurecli-interactive
az aks delete --resource-group myResourceGroup --name myAKSCluster
```

Jsou k dispozici následující možnosti `az aks delete` příkaz.

| Argument | Popis | Požaduje se |
|---|---|:---:|
| `--name` `-n` | Název prostředku pro spravovaný cluster. | ano |
| `--resource-group` `-g` | Název skupiny prostředků Azure Container Service. | ano |
| `--no-wait` | Není počkejte na dokončení operace časově náročné. | ne |
| `--yes` `-y` | Nezobrazovat výzvu k potvrzení. | ne |

## <a name="azure-portal"></a>Azure Portal

V portálu Azure přejděte do skupiny prostředků obsahující prostředek Azure Container Service (AKS), vyberte prostředek a klikněte na tlačítko **odstranit**. Zobrazí se výzva k potvrzení operace odstranění.

![Odstranit AKS clusteru portálu](media/container-service-delete-cluster/delete-aks-portal.png)

<!-- LINKS - internal -->
[az-aks-delete]: azure-files-dynamic-pv.md