---
title: "Rychlý start Azure – Vytvoření účtu úložiště pomocí Azure CLI | Dokumentace Microsoftu"
description: "Rychle se naučíte, jak vytvořit nový účet úložiště pomocí Azure CLI."
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
ms.devlang: na
ms.topic: quickstart
ms.date: 06/28/2017
ms.author: marsma
ms.openlocfilehash: 7186c5e2ce94d06b21d95a557e960b82e268cdce
ms.sourcegitcommit: 29bac59f1d62f38740b60274cb4912816ee775ea
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/29/2017
---
# <a name="create-a-storage-account-using-the-azure-cli"></a>Vytvoření účtu úložiště pomocí Azure CLI

Azure CLI slouží k vytváření a správě prostředků Azure z příkazového řádku nebo ve skriptech. Tento rychlý start podrobně popisuje použití Azure CLI k vytvoření účtu Azure Storage.

Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) před tím, než začnete.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Pokud se rozhodnete nainstalovat a používat rozhraní příkazového řádku místně, musíte mít rozhraní příkazového řádku Azure ve verzi 2.0.4 nebo novější. Verzi zjistíte spuštěním příkazu `az --version`. Pokud potřebujete instalaci nebo upgrade, přečtěte si téma [Instalace Azure CLI 2.0](/cli/azure/install-azure-cli). 

## <a name="create-resource-group"></a>Vytvoření skupiny prostředků

Vytvořte skupinu prostředků Azure pomocí příkazu [az group create](/cli/azure/group#create). Skupina prostředků je logický kontejner, ve kterém se nasazují a spravují prostředky Azure. Tento příklad vytvoří skupinu prostředků *myResourceGroup* v oblasti *eastus*.

```azurecli-interactive
az group create \
    --name myResourceGroup \
    --location eastus
```

Pokud si nejste jisti, jakou oblast máte zadat v parametru `--location`, pomocí příkazu [az account list-locations](/cli/azure/account#list) můžete načíst seznam podporovaných oblastí pro vaše předplatné.

```azurecli-interactive
az account list-locations \
    --query "[].{Region:name}" \
    --out table
```

## <a name="create-a-general-purpose-standard-storage-account"></a>Vytvoření účtu úložiště úrovně Standard pro obecné účely

Existuje několik typů účtů úložiště vhodných pro různé scénáře použití, z nichž každý podporuje jednu nebo několik služeb úložiště (objekty blob, soubory, tabulky nebo fronty). Následující tabulka obsahuje podrobnosti o dostupných typech účtů úložiště.

|**Typ účtu úložiště**|**Standard pro obecné účely**|**Premium pro obecné účely**|**Blob Storage, horká a studená vrstva přístupu**|
|-----|-----|-----|-----|
|**Podporované služby**| Služby Blob, File, Table a služba front | Služba Blob | Služba Blob|
|**Typy podporovaných objektů blob**|Objekty blob bloku, objekty blob stránky a doplňovací objekty blob | Objekty blob stránky | Objekty blob bloku a doplňovací objekty blob|

Vytvořte pomocí příkazu [az storage account create](/cli/azure/storage/account#create) účet úložiště úrovně Standard pro obecné účely.

```azurecli-interactive
az storage account create \
    --name mystorageaccount \
    --resource-group myResourceGroup \
    --location eastus \
    --sku Standard_LRS \
    --encryption blob
```

## <a name="clean-up-resources"></a>Vyčištění prostředků

Pokud již ze skupiny prostředků nepotřebujete žádné prostředky, včetně účtu úložiště, který jste vytvořili v rámci tohoto rychlého startu, odstraňte ji pomocí příkazu [az group delete](/cli/azure/group#delete).

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Další kroky

V tomto rychlém startu jste vytvořili skupinu prostředků a účet úložiště úrovně Standard pro obecné účely. Pokud chcete zjistit, jak přenášet data do a z účtu úložiště, pokračujte k rychlému startu pro úložiště objektů blob.

> [!div class="nextstepaction"]
> [Přenos objektů do a z úložiště objektů blob v Azure pomocí Azure CLI](../blobs/storage-quickstart-blobs-cli.md)