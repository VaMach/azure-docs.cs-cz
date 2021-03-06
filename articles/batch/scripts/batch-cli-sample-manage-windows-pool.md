---
title: "Ukázkový skript Azure CLI – Fond s Windows ve službě Batch | Microsoft Docs"
description: "Ukázkový skript Azure CLI – Vytvoření a správa fondu s Windows ve službě Batch"
services: batch
documentationcenter: 
author: dlepow
manager: jeconnoc
editor: 
ms.assetid: 
ms.service: batch
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 01/29/2018
ms.author: danlep
ms.openlocfilehash: b1fe98169e47c5f5386867cf2b6c23f8bc3bfb65
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/01/2018
---
# <a name="cli-example-create-and-manage-a-windows-pool-in-azure-batch"></a>Příklad rozhraní příkazového řádku: Vytvoření a správa fondu s Windows ve službě Azure Batch

Tento skript ukazuje několik příkazů dostupných v Azure CLI pro vytvoření a správu fondu výpočetních uzlů s Windows ve službě Azure Batch. Fond s Windows je možné nakonfigurovat dvěma způsoby, buď s konfigurací služby Cloud Services, nebo s konfigurací virtuálního počítače. Tento příklad ukazuje, jak vytvořit fond s Windows s konfigurací služby Cloud Services.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Pokud se rozhodnete nainstalovat a používat rozhraní příkazového řádku místně, musíte mít verzi Azure CLI 2.0.20 nebo novější. Verzi zjistíte spuštěním příkazu `az --version`. Pokud potřebujete instalaci nebo upgrade, přečtěte si téma [Instalace Azure CLI 2.0](/cli/azure/install-azure-cli). 

## <a name="example-script"></a>Ukázkový skript

[!code-azurecli-interactive[main](../../../cli_scripts/batch/manage-pool/manage-pool-windows.sh "Manage Windows Cloud Services Pool")]

## <a name="clean-up-deployment"></a>Vyčištění nasazení

Spuštěním následujícího příkazu odeberte skupinu prostředků a všechny k ní přidružené prostředky.

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>Vysvětlení skriptu

Tento skript používá následující příkazy. Každý příkaz v tabulce odkazuje na příslušnou část dokumentace.

| Příkaz | Poznámky |
|---|---|
| [az group create](/cli/azure/group#az_group_create) | Vytvoří skupinu prostředků, ve které se ukládají všechny prostředky. |
| [az batch account create](/cli/azure/batch/account#az_batch_account_create) | Vytvoří účet Batch. |
| [az batch account login](https://docs.microsoft.com/cli/azure/batch/account#az_batch_account_login) | Provede ověření v zadaném účtu Batch pro další práci s rozhraním příkazového řádku. |
| [az batch pool create](https://docs.microsoft.com/cli/azure/batch/pool#az_batch_pool_create) | Vytvoří fond výpočetních uzlů.  |
| [az batch pool set](https://docs.microsoft.com/cli/azure/batch/pool#az_batch_pool_set) | Aktualizuje vlastnosti fondu.  |
| [az batch pool autoscale enable](https://docs.microsoft.com/cli/azure/batch/pool/autoscale#az_batch_pool_autoscale_enable) | Povolí automatické škálování fondu a použije vzorec.  |
| [az batch pool show](https://docs.microsoft.com/cli/azure/batch/pool#az_batch_pool_show) | Zobrazí vlastnosti fondu.  |
| [az batch pool autoscale disable](https://docs.microsoft.com/cli/azure/batch/pool/autoscale#az_batch_pool_autoscale_disable) | Zakáže automatické škálování fondu. |
| [az group delete](/cli/azure/group#az_group_delete) | Odstraní skupinu prostředků včetně všech vnořených prostředků. |


## <a name="next-steps"></a>Další kroky

Další informace o Azure CLI najdete v [dokumentaci k Azure CLI](https://docs.microsoft.com/cli/azure/overview).
