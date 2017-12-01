---
title: "Azure CLI skriptu ukázkové – vypočítat velikost kontejneru objektů blob | Microsoft Docs"
description: "Vypočítejte velikost kontejneru v úložiště objektů Blob v Azure pomocí součtem velikost objektů BLOB v kontejneru."
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
ms.date: 06/28/2017
ms.author: tamram
ms.openlocfilehash: 61a553e47a642aead323a19d0724fdccc94a6282
ms.sourcegitcommit: 5a6e943718a8d2bc5babea3cd624c0557ab67bd5
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/01/2017
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
