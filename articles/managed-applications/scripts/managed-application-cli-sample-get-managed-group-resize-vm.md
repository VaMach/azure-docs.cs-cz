---
title: "Rozhraní příkazového řádku Azure ukázkový skript – získat skupinu spravovaných prostředků a změně velikosti virtuálních počítačů | Microsoft Docs"
description: "Ukázka skriptu Azure CLI - získat skupinu spravovaných prostředků a změně velikosti virtuálních počítačů"
services: managed-applications
documentationcenter: na
author: tfitzmac
manager: timlt
ms.service: managed-applications
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/25/2017
ms.author: tomfitz
ms.openlocfilehash: c78d2646471e40d60972cf91cb5bbd351f71a66c
ms.sourcegitcommit: 804db51744e24dca10f06a89fe950ddad8b6a22d
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/30/2017
---
# <a name="get-resources-in-a-managed-resource-group-and-resize-vms-with-azure-cli"></a>Získat prostředky ve skupině spravovaných prostředků a změna velikosti virtuálních počítačů pomocí rozhraní příkazového řádku Azure

Tento skript načte zdroje ze skupiny spravovaných prostředků a změní velikost virtuální počítače v příslušné skupině prostředků.


[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Ukázkový skript

[!code-azurecli[main](../../../cli_scripts/managed-applications/get-application/get-application.sh "Get application")]


## <a name="script-explanation"></a>Vysvětlení skriptu

Tento skript používá následující příkazy k nasazení spravované aplikace. Každý příkaz v tabulce odkazy na dokumentaci specifické pro příkaz.

| Příkaz | Poznámky |
|---|---|
| [AZ managedapp seznamu](https://docs.microsoft.com/cli/azure/managedapp#az_managedapp_list) | Seznam spravovaných aplikací. Zadejte hodnoty dotazu a zaměřit se výsledky. |
| [Seznam zdrojů az](https://docs.microsoft.com/cli/azure/resource#az_resource_list) | Zobrazí seznam prostředků. Zadejte prostředek skupiny a dotaz hodnoty a zaměřit se výsledek. |
| [Změna velikosti virtuálních počítačů az](https://docs.microsoft.com/cli/azure/vm#az_vm_resize) | Aktualizujte velikost virtuálního počítače. |


## <a name="next-steps"></a>Další kroky

* Úvod do spravovaných aplikací, najdete v části [Azure spravovaných aplikací – přehled](../overview.md).
* Další informace o rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).
