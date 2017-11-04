---
title: "Ukázka skriptu Azure CLI - přístupových klíčů k účtu úložiště otáčení | Microsoft Docs"
description: "Vytvoření účtu úložiště Azure potom načíst a otočit jeho přístupových klíčů k účtu."
services: storage
documentationcenter: na
author: mmacy
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
ms.author: marsma
ms.openlocfilehash: 1b59144e0426b50c2ac49acbd7914d975ff037ec
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="create-a-storage-account-and-rotate-its-account-access-keys"></a>Vytvoření účtu úložiště a otočit jeho přístupových klíčů k účtu

Tento skript vytvoří účet úložiště Azure, zobrazí nový účet úložiště přístupové klávesy a potom obnovuje (otočí) klíče.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Ukázkový skript

[!code-azurecli-interactive[main](../../../cli_scripts/storage/rotate-storage-account-keys/rotate-storage-account-keys.sh "Rotate storage account keys")]

## <a name="clean-up-deployment"></a>Vyčištění nasazení 

Spusťte následující příkaz pro odebrání skupiny prostředků, účet úložiště a všechny související prostředky.

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>Vysvětlení skriptu

Tento skript používá následující příkazy k vytvoření účtu úložiště a načtení a otočit jeho přístupové klíče. Každou položku v tabulce odkazy na dokumentaci specifické pro příkaz.

| Příkaz | Poznámky |
|---|---|
| [Vytvoření skupiny az](/cli/azure/group#create) | Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky. |
| [Vytvořit účet úložiště az](/cli/azure/storage/account#create) | Vytvoří účet úložiště Azure v zadaná skupina prostředků. |
| [seznam klíčů účtu úložiště az](/cli/azure/storage/account/keys#list) | Zobrazí přístupové klíče účtu úložiště pro zadaný účet. |
| [obnovit klíče účtu úložiště az](/cli/azure/storage/account/keys#renew) | Znovu generuje přístupový klíč účtu úložiště primární nebo sekundární. |

## <a name="next-steps"></a>Další kroky

Další informace o rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](/cli/azure/overview).

Ukázky skriptu rozhraní příkazového řádku další úložiště najdete v [ukázky rozhraní příkazového řádku Azure pro úložiště objektů Azure Blob](../blobs/storage-samples-blobs-cli.md).
