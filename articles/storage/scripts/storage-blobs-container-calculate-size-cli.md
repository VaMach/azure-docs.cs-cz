---
title: "Azure CLI skriptu ukázkové – vypočítat velikost kontejneru objektů blob | Microsoft Docs"
description: "Vypočítejte velikost kontejneru v úložiště objektů Blob v Azure pomocí součtem velikost objektů BLOB v kontejneru."
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
ms.date: 06/28/2017
ms.author: marsma
ms.openlocfilehash: 86922fac2154d4084f34275e761addeafe565fc0
ms.sourcegitcommit: 9a61faf3463003375a53279e3adce241b5700879
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/15/2017
---
# <a name="calculate-the-size-of-a-blob-storage-container"></a>Vypočítat velikost kontejner úložiště objektů Blob

Tento skript se vypočítá velikost kontejner v úložišti objektů Blob Azure součtem velikost objektů BLOB v kontejneru.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

> [!IMPORTANT]
> Tento skript rozhraní příkazového řádku poskytuje odhadovanou velikost kontejneru a by se neměla používat pro výpočty fakturace.

## <a name="sample-script"></a>Ukázkový skript

[!code-azurecli[main](../../../cli_scripts/storage/calculate-container-size/calculate-container-size.sh?highlight=2-3 "Calculate container size")]

## <a name="clean-up-deployment"></a>Vyčištění nasazení 

Spusťte následující příkaz pro odebrání skupiny prostředků, kontejneru a všechny související prostředky.

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>Vysvětlení skriptu

Tento skript používá následující příkazy vypočítat velikost kontejner úložiště objektů Blob. Každou položku v tabulce odkazy na dokumentaci specifické pro příkaz.

| Příkaz | Poznámky |
|---|---|
| [Vytvoření skupiny az](/cli/azure/group#create) | Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky. |
| [Nahrání objektu blob úložiště az](/cli/azure/storage/account#create) | Odešle místních souborů do kontejner úložiště objektů Blob v Azure. |
| [seznam az úložiště objektů blob](/cli/azure/storage/account/keys#list) | Zobrazí seznam objektů BLOB v kontejneru úložiště objektů Blob v Azure. |

## <a name="next-steps"></a>Další kroky

Další informace o rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](/cli/azure/overview).

Ukázky skriptu rozhraní příkazového řádku další úložiště najdete v [ukázky rozhraní příkazového řádku Azure pro úložiště objektů Azure Blob](../blobs/storage-samples-blobs-cli.md).
