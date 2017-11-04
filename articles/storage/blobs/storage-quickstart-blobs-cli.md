---
title: "Azure rychlý start - objekty přenosu do nebo z Azure Blob storage pomocí rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Naučte se rychle převést objekty do a z Azure Blob storage pomocí rozhraní příkazového řádku Azure"
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
ms.date: 07/19/2017
ms.author: marsma
ms.openlocfilehash: c9b7e7a1fbc6b67821183ce31bdf2527de490c92
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="transfer-objects-tofrom-azure-blob-storage-using-the-azure-cli"></a>Objekty přenosu do nebo z Azure Blob storage pomocí rozhraní příkazového řádku Azure

Azure CLI slouží k vytváření a správě prostředků Azure z příkazového řádku nebo ve skriptech. Tento rychlý start údaje, nahrávání a stahování dat do a z Azure Blob storage pomocí rozhraní příkazového řádku Azure.

Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) před tím, než začnete.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Pokud se rozhodnete nainstalovat a používat rozhraní příkazového řádku místně, musíte mít rozhraní příkazového řádku Azure ve verzi 2.0.4 nebo novější. Verzi zjistíte spuštěním příkazu `az --version`. Pokud potřebujete instalaci nebo upgrade, přečtěte si téma [Instalace Azure CLI 2.0](/cli/azure/install-azure-cli).

[!INCLUDE [storage-quickstart-tutorial-intro-include-cli](../../../includes/storage-quickstart-tutorial-intro-include-cli.md)]

## <a name="create-a-container"></a>Vytvoření kontejneru

Objekty BLOB jsou vždy nahrát do kontejneru. Kontejnery umožňují uspořádat skupiny objektů BLOB jako uspořádání souborů v adresářích ve vašem počítači.

K vytvoření kontejneru pro ukládání objektů blob použijte příkaz [az storage container create](/cli/azure/storage/container#create).

```azurecli-interactive
az storage container create --name mystoragecontainer
```

## <a name="upload-a-blob"></a>Nahrání objektu blob

Úložiště objektů blob podporuje objekty blob bloku, doplňovací objekty blob a objekty blob stránky. Většina souborů uložených v úložišti objektů Blob jsou uloženy jako objekty BLOB bloku. Append – objekty BLOB se používají při data musí být přidaný do existující objekt blob bez úpravy jeho existující obsah, například konkrétní protokolování. Objekty blob stránky zálohují soubory virtuálního pevného disku virtuálních počítačů IaaS.

V tomto příkladu jsme nahrát objekt blob do kontejneru jsme vytvořili v předchozím kroku s [az úložiště objektů blob nahrávání](/cli/azure/storage/blob#upload) příkaz.

```azurecli-interactive
az storage blob upload \
    --container-name mystoragecontainer \
    --name blobName \
    --file ~/path/to/local/file
```

Tato operace vytvoří objekt blob, pokud ještě neexistuje, a přepíše ho, pokud už existoval. Nahrání libovolný počet souborů, jak se vám líbí, než budete pokračovat.

## <a name="list-the-blobs-in-a-container"></a>Zobrazí seznam objektů blob v kontejneru

Pomocí příkazu [az storage blob list](/cli/azure/storage/blob#list) zobrazte seznam objektů blob v kontejneru.

```azurecli-interactive
az storage blob list \
    --container-name mystoragecontainer \
    --output table
```

## <a name="download-a-blob"></a>Stažení objektu blob

Použití [az úložiště objektů blob stažení](/cli/azure/storage/blob#download) příkaz ke stažení do objektu blob, který jste nahráli dříve.

```azurecli-interactive
az storage blob download \
    --container-name mystoragecontainer \
    --name blobName \
    --file ~/destination/path/for/file
```

## <a name="data-transfer-with-azcopy"></a>Přenos dat pomocí nástroje AzCopy

[AzCopy](../common/storage-use-azcopy-linux.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) nástroj je další možností pro přenos dat Skriptovatelná vysoce výkonné úložiště Azure. AzCopy můžete použít k přenosu dat do a z úložiště objektů Blob, soubor a tabulku.

Jako zběžný příklad, zde je AzCopy příkaz pro nahrání souboru názvem *myfile.txt* k *mystoragecontainer* kontejneru.

```bash
azcopy \
    --source /mnt/myfiles \
    --destination https://mystorageaccount.blob.core.windows.net/mystoragecontainer \
    --dest-key <storage-account-access-key> \
    --include "myfile.txt"
```

## <a name="clean-up-resources"></a>Vyčištění prostředků

Pokud již nepotřebujete kterýkoli z prostředků ve vaší skupině prostředků, včetně účet úložiště, který jste vytvořili v tento rychlý start, odstraňte skupinu prostředků s [odstranění skupiny az](/cli/azure/group#delete) příkaz.

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Další kroky

V tento rychlý start zjistili, jak pro přenos souborů mezi místní disk a kontejner v úložišti objektů Blob Azure. Další informace o práci s objekty BLOB ve službě Azure Storage, i nadále tento kurz pro práci s úložištěm Azure Blob.

> [!div class="nextstepaction"]
> [Postupy: Blob operace úložiště pomocí Azure CLI](storage-how-to-use-blobs-cli.md)
