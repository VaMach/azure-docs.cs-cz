---
title: "Rychlý start Azure – Přenos objektů do a z úložiště objektů blob v Azure pomocí Azure CLI | Dokumentace Microsoftu"
description: "Rychle se naučíte, jak přenášet objekty do a z úložiště objektů blob v Azure pomocí Azure CLI"
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
ms.date: 07/19/2017
ms.author: tamram
ms.openlocfilehash: a300294c83cb206e6211985c736e3ff01bb1ab43
ms.sourcegitcommit: 5a6e943718a8d2bc5babea3cd624c0557ab67bd5
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/01/2017
---
# <a name="transfer-objects-tofrom-azure-blob-storage-using-the-azure-cli"></a>Přenos objektů do a z úložiště objektů blob v Azure pomocí Azure CLI

Azure CLI slouží k vytváření a správě prostředků Azure z příkazového řádku nebo ve skriptech. Tento rychlý start podrobně popisuje použití Azure CLI k nahrávání dat do úložiště objektů blob v Azure a stahování dat z úložiště objektů blob v Azure.

Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) před tím, než začnete.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Pokud se rozhodnete nainstalovat a používat rozhraní příkazového řádku místně, musíte mít rozhraní příkazového řádku Azure ve verzi 2.0.4 nebo novější. Verzi zjistíte spuštěním příkazu `az --version`. Pokud potřebujete instalaci nebo upgrade, přečtěte si téma [Instalace Azure CLI 2.0](/cli/azure/install-azure-cli).

[!INCLUDE [storage-quickstart-tutorial-intro-include-cli](../../../includes/storage-quickstart-tutorial-intro-include-cli.md)]

## <a name="create-a-container"></a>Vytvoření kontejneru

Objekty blob se vždy nahrávají do kontejneru. Kontejnery umožňují organizovat skupiny objektů blob podobně, jako organizujete soubory do adresářů na svém počítači.

K vytvoření kontejneru pro ukládání objektů blob použijte příkaz [az storage container create](/cli/azure/storage/container#create).

```azurecli-interactive
az storage container create --name mystoragecontainer
```

## <a name="upload-a-blob"></a>Nahrání objektu blob

Úložiště objektů blob podporuje objekty blob bloku, doplňovací objekty blob a objekty blob stránky. Většina souborů uložených v úložišti objektů blob se ukládá jako objekty blob bloku. Doplňovací objekty blob se používají, když je k existujícímu objektu blob potřeba přidat data a neměnit přitom jeho stávající obsah, například pro účely protokolování. Objekty blob stránky zálohují soubory virtuálního pevného disku virtuálních počítačů IaaS.

V tomto příkladu nahrajeme objekt blob do kontejneru, který jsme vytvořili v předchozím kroku, a to pomocí příkazu [az storage blob upload](/cli/azure/storage/blob#upload).

```azurecli-interactive
az storage blob upload \
    --container-name mystoragecontainer \
    --name blobName \
    --file ~/path/to/local/file
```

Tato operace vytvoří objekt blob, pokud ještě neexistuje, a přepíše ho, pokud už existoval. Než budete pokračovat, můžete nahrát libovolné množství souborů.

## <a name="list-the-blobs-in-a-container"></a>Zobrazí seznam objektů blob v kontejneru

Pomocí příkazu [az storage blob list](/cli/azure/storage/blob#list) zobrazte seznam objektů blob v kontejneru.

```azurecli-interactive
az storage blob list \
    --container-name mystoragecontainer \
    --output table
```

## <a name="download-a-blob"></a>Stažení objektu blob

Pomocí příkazu [az storage blob download](/cli/azure/storage/blob#download) stáhněte objekt blob, který jste nahráli dříve.

```azurecli-interactive
az storage blob download \
    --container-name mystoragecontainer \
    --name blobName \
    --file ~/destination/path/for/file
```

## <a name="data-transfer-with-azcopy"></a>Přenos dat pomocí AzCopy

Nástroj [AzCopy](../common/storage-use-azcopy-linux.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) představuje další možnost vysoce výkonného a skriptovatelného přenosu dat pro službu Azure Storage. Pomocí AzCopy můžete přenášet data do a ze služeb Blob, File a Table Storage.

Tady je jednoduchý příklad příkazu AzCopy pro nahrání souboru *myfile.txt* do kontejneru *mystoragecontainer*.

```bash
azcopy \
    --source /mnt/myfiles \
    --destination https://mystorageaccount.blob.core.windows.net/mystoragecontainer \
    --dest-key <storage-account-access-key> \
    --include "myfile.txt"
```

## <a name="clean-up-resources"></a>Vyčištění prostředků

Pokud již ze skupiny prostředků nepotřebujete žádné prostředky, včetně účtu úložiště, který jste vytvořili v rámci tohoto rychlého startu, odstraňte ji pomocí příkazu [az group delete](/cli/azure/group#delete).

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Další kroky

V tomto rychlém startu jste zjistili, jak přenášet soubory mezi místním diskem a kontejnerem v úložišti objektů blob v Azure. Další informace o práci s objekty blob ve službě Azure Storage najdete v kurzu práce s úložištěm objektů blob v Azure.

> [!div class="nextstepaction"]
> [Postup: Operace s úložištěm objektů blob pomocí Azure CLI](storage-how-to-use-blobs-cli.md)
