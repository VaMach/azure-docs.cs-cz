---
title: "Azure CLI skriptu ukázkové – nasadit spravované aplikace | Microsoft Docs"
description: "Azure CLI skriptu ukázkové – nasazení definici spravované aplikace"
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
ms.openlocfilehash: 62d0247df3b3d9f242877e4ea27ccc871cf797c0
ms.sourcegitcommit: 804db51744e24dca10f06a89fe950ddad8b6a22d
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/30/2017
---
# <a name="deploy-a-managed-application-for-service-catalog-with-azure-cli"></a>Nasadit spravované aplikace pro katalog služby pomocí rozhraní příkazového řádku Azure

Tento skript nasadí definice spravované aplikace z katalogu služeb. 


[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Ukázkový skript

[!code-azurecli[main](../../../cli_scripts/managed-applications/create-application/create-application.sh "Create application")]


## <a name="script-explanation"></a>Vysvětlení skriptu

Tento skript používá následující příkaz k nasazení spravované aplikace. Každý příkaz v tabulce odkazy na dokumentaci specifické pro příkaz.

| Příkaz | Poznámky |
|---|---|
| [Vytvoření az managedapp](https://docs.microsoft.com/cli/azure/managedapp#az_managedapp_create) | Vytvoření spravované aplikace. Zadejte ID definice a parametry šablony. |


## <a name="next-steps"></a>Další kroky

* Úvod do spravovaných aplikací, najdete v části [Azure spravovaných aplikací – přehled](../overview.md).
* Další informace o rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).
