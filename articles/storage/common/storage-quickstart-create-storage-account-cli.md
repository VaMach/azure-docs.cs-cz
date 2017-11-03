---
title: "Azure rychlý start - vytvořit účet úložiště pomocí rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Rychle se Naučte se vytvořit nový účet úložiště pomocí rozhraní příkazového řádku Azure."
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
ms.devlang: na
ms.topic: quickstart
ms.date: 06/28/2017
ms.author: marsma
ms.openlocfilehash: b1fb2da4acf6e06219d790f2354cada4f1e34285
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="create-a-storage-account-using-the-azure-cli"></a>Vytvořit účet úložiště pomocí rozhraní příkazového řádku Azure

Azure CLI slouží k vytváření a správě prostředků Azure z příkazového řádku nebo ve skriptech. Tento rychlý start údaje, pomocí rozhraní příkazového řádku Azure k vytvoření účtu úložiště Azure.

Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) před tím, než začnete.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Pokud se rozhodnete nainstalovat a používat rozhraní příkazového řádku místně, musíte mít rozhraní příkazového řádku Azure ve verzi 2.0.4 nebo novější. Verzi zjistíte spuštěním příkazu `az --version`. Pokud potřebujete instalaci nebo upgrade, přečtěte si téma [Instalace Azure CLI 2.0](/cli/azure/install-azure-cli). 

## <a name="create-resource-group"></a>Vytvoření skupiny prostředků

Vytvořte skupinu prostředků Azure pomocí příkazu [az group create](/cli/azure/group#create). Skupina prostředků je logický kontejner, ve kterém se nasazují a spravují prostředky Azure. Tento příklad vytvoří skupinu prostředků s názvem *myResourceGroup* v *eastus* oblast.

```azurecli-interactive
az group create \
    --name myResourceGroup \
    --location eastus
```

Pokud si nejste jisti, které oblasti pro zadejte `--location` parametr, můžete načíst seznam podporovaných oblastí pro vaše předplatné s [míst seznamu účet az](/cli/azure/account#list) příkaz.

```azurecli-interactive
az account list-locations \
    --query "[].{Region:name}" \
    --out table
```

## <a name="create-a-general-purpose-standard-storage-account"></a>Vytvořit účet standardního úložiště pro obecné účely

Existuje několik typů účtů úložiště vhodné pro scénáře použití různých, z nichž každý podporuje jeden nebo více službách úložiště (objekty BLOB, soubory, tabulky nebo fronty). Následující tabulka uvádí typy účtů úložiště k dispozici.

|**Typ účtu úložiště**|**Standard pro obecné účely**|**Premium pro obecné účely**|**Blob Storage, horká a studená vrstva přístupu**|
|-----|-----|-----|-----|
|**Podporované služby**| Tabulka objektů BLOB, soubor, služba fronty | Blob Service | Blob Service|
|**Typy podporovaných objektů blob**|Objekty BLOB bloku, objekty BLOB stránky, doplňovací objekty BLOB | Objekty blob stránky | Objekty blob bloku a doplňovací objekty blob|

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

Pokud již nepotřebujete kterýkoli z prostředků ve vaší skupině prostředků, včetně účet úložiště, který jste vytvořili v tento rychlý start, odstraňte skupinu prostředků s [odstranění skupiny az](/cli/azure/group#delete) příkaz.

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Další kroky

V tento rychlý start jste vytvořili skupinu prostředků a účet standardního úložiště pro obecné účely. Pokud chcete dozvědět, jak k přenosu dat do a z vašeho účtu úložiště, pokračujte do úložiště objektů Blob rychlý start.

> [!div class="nextstepaction"]
> [Přenos objekty do a z Azure Blob storage pomocí rozhraní příkazového řádku Azure](../blobs/storage-quickstart-blobs-cli.md)