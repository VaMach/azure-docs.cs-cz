---
title: "Ukázka skriptu Azure CLI - odstranění kontejnerů podle předpony | Microsoft Docs"
description: "Odstranění kontejnerů objektů blob Azure Storage na základě předpony názvu kontejneru."
services: storage
documentationcenter: na
author: tamram
manager: timlt
editor: tysonn
ms.assetid: 
ms.custom: mvc
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: azurecli
ms.topic: sample
ms.date: 06/22/2017
ms.author: tamram
ms.openlocfilehash: d14195abf1c17d11e259ed9edb5112626b063112
ms.sourcegitcommit: 5a6e943718a8d2bc5babea3cd624c0557ab67bd5
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/01/2017
---
# <a name="delete-containers-based-on-container-name-prefix"></a>Odstranění kontejnerů na základě předpony názvu kontejneru

Tento skript nejprve vytvoří několik ukázkových kontejnerů v úložišti objektů Blob v Azure, a poté odstraní některé z kontejnerů na základě předpony v názvu kontejneru.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Ukázkový skript

[!code-azurecli-interactive[main](../../../cli_scripts/storage/delete-containers-by-prefix/delete-containers-by-prefix.sh?highlight=2-3 "Delete containers by prefix")]

## <a name="clean-up-deployment"></a>Vyčištění nasazení 

Pomocí následujícího příkazu odeberte skupinu zdrojů, zbývající kontejnery, a všechny související prostředky.

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>Vysvětlení skriptu

Tento skript používá následující příkazy k odstranění kontejnerů na základě předpony názvu kontejneru. Každou položku v tabulce odkazy na dokumentaci specifické pro příkaz.

| Příkaz | Poznámky |
|---|---|
| [Vytvoření skupiny az](/cli/azure/group#create) | Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky. |
| [Vytvořit účet úložiště az](/cli/azure/storage/account#create) | Vytvoří účet úložiště Azure v zadaná skupina prostředků. |
| [Vytvoření kontejneru úložiště az](/cli/azure/storage/container#create) | Vytvoří kontejner v úložišti objektů Blob Azure. |
| [AZ seznam kontejneru úložiště](/cli/azure/storage/container#list) | Uvádí kontejnery v účtu Azure Storage. |
| [odstranit kontejner úložiště az](/cli/azure/storage/container#delete) | Odstraní kontejnery v účtu Azure Storage. |

## <a name="next-steps"></a>Další kroky

Další informace o rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](/cli/azure/overview).

Ukázky skriptu rozhraní příkazového řádku další úložiště najdete v [ukázky rozhraní příkazového řádku Azure pro Azure Storage](../blobs/storage-samples-blobs-cli.md).
