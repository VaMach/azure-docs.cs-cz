---
title: "Přesun dat do a z Azure Blob Storage pomocí Python | Microsoft Docs"
description: "Přesun dat z a do služby Azure Blob Storage pomocí Pythonu"
services: machine-learning,storage
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 24276252-b3dd-4edf-9e5d-f6803f8ccccc
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/04/2017
ms.author: bradsev
ms.openlocfilehash: f6e325f70a37200552832b9c0ac38f6c56471f2c
ms.sourcegitcommit: 93902ffcb7c8550dcb65a2a5e711919bd1d09df9
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/09/2017
---
# <a name="move-data-to-and-from-azure-blob-storage-using-python"></a>Přesun dat do a z Azure Blob Storage používá Python
Toto téma popisuje, jak do seznamu, nahrávání a stahování objektům BLOB pomocí rozhraní API jazyka Python. Není součástí sady Azure SDK rozhraní API jazyka Python můžete:

* Vytvoření kontejneru
* Nahrání objektu blob do kontejneru
* Stáhnout objekty blob
* Zobrazí seznam objektů blob v kontejneru
* Odstranění objektu blob

Další informace o používání rozhraní API Python najdete v tématu [jak používat služby úložiště objektů Blob z Pythonu](../../storage/blobs/storage-python-how-to-use-blob-storage.md).

[!INCLUDE [blob-storage-tool-selector](../../../includes/machine-learning-blob-storage-tool-selector.md)]

> [!NOTE]
> Pokud používáte virtuální počítač, který byl nastaven pomocí skriptů poskytovaných [datové vědy virtuálních počítačů v Azure](virtual-machines.md), pak AzCopy je již nainstalován ve virtuálním počítači.
> 
> [!NOTE]
> Dokončení Úvod do Azure blob storage, najdete v části [základy objektu Blob Azure](../../storage/blobs/storage-dotnet-how-to-use-blobs.md) a [služby objektů Blob Azure](https://msdn.microsoft.com/library/azure/dd179376.aspx).
> 
> 

## <a name="prerequisites"></a>Požadavky
Tento dokument předpokládá, že máte předplatné Azure, účet úložiště a odpovídající klíč úložiště pro tento účet. Před nahrávání nebo stahování dat, musíte znát klíč účet a název účtu úložiště Azure.

* Předplatné Azure, naleznete v tématu [bezplatnou zkušební verzi jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).
* Pokyny pro vytvoření účtu úložiště a získávání účet a klíčové informace naleznete v tématu [účty Azure storage](../../storage/common/storage-create-storage-account.md).

## <a name="upload-data-to-blob"></a>Nahrání dat do objektu Blob
Přidejte následující fragment kódu v horní jakéhokoli kódu jazyka Python, ve kterém chcete k programovému přístupu ke službě Azure Storage:

    from azure.storage.blob import BlobService

**BlobService** objekt vám umožňuje spolupracovat s kontejnery a objekty BLOB. Následující kód vytvoří objekt BlobService pomocí klíč účet a název účtu úložiště. Nahraďte název účtu a klíč účtu skutečné účtu a klíč.

    blob_service = BlobService(account_name="<your_account_name>", account_key="<your_account_key>")

Nahrání dat do objektu blob pomocí následujících metod:

1. uveďte\_bloku\_blob\_z\_cesta (odešle obsah souboru ze zadané cesty)
2. uveďte\_block_blob\_z\_souboru (odešle obsah z již otevřeného souboru/proud)
3. uveďte\_bloku\_blob\_z\_bajtů (nahrávání na pole bajtů)
4. uveďte\_bloku\_blob\_z\_text (odešle zadanou textovou hodnotu pomocí zadaného kódování)

Následující vzorový kód uloží místní soubor do kontejneru:

    blob_service.put_block_blob_from_path("<your_container_name>", "<your_blob_name>", "<your_local_file_name>")

Následující vzorový kód odesílá všechny soubory (s výjimkou adresáře) do místního adresáře do úložiště objektů blob:

    from azure.storage.blob import BlobService
    from os import listdir
    from os.path import isfile, join

    # Set parameters here
    ACCOUNT_NAME = "<your_account_name>"
    ACCOUNT_KEY = "<your_account_key>"
    CONTAINER_NAME = "<your_container_name>"
    LOCAL_DIRECT = "<your_local_directory>"        

    blob_service = BlobService(account_name=ACCOUNT_NAME, account_key=ACCOUNT_KEY)
    # find all files in the LOCAL_DIRECT (excluding directory)
    local_file_list = [f for f in listdir(LOCAL_DIRECT) if isfile(join(LOCAL_DIRECT, f))]

    file_num = len(local_file_list)
    for i in range(file_num):
        local_file = join(LOCAL_DIRECT, local_file_list[i])
        blob_name = local_file_list[i]
        try:
            blob_service.put_block_blob_from_path(CONTAINER_NAME, blob_name, local_file)
        except:
            print "something wrong happened when uploading the data %s"%blob_name


## <a name="download-data-from-blob"></a>Stahování dat z objektu Blob
Použijte následující metody ke stahování dat z objektu blob:

1. získat\_blob\_k\_cesta
2. získat\_blob\_k\_souboru
3. získat\_blob\_k\_bajtů
4. získat\_blob\_k\_textu

Tyto metody, které provádějí potřebné rozdělování, když velikost dat přesáhne 64 MB.

Následující vzorový kód stáhne obsah objektu blob v kontejneru do místního souboru:

    blob_service.get_blob_to_path("<your_container_name>", "<your_blob_name>", "<your_local_file_name>")

Následující vzorový kód stáhne všechny objekty BLOB kontejneru. Použije seznam\_získat seznam dostupných objektů BLOB v kontejneru objektů BLOB a je stáhne do místního adresáře.

    from azure.storage.blob import BlobService
    from os.path import join

    # Set parameters here
    ACCOUNT_NAME = "<your_account_name>"
    ACCOUNT_KEY = "<your_account_key>"
    CONTAINER_NAME = "<your_container_name>"
    LOCAL_DIRECT = "<your_local_directory>"        

    blob_service = BlobService(account_name=ACCOUNT_NAME, account_key=ACCOUNT_KEY)

    # List all blobs and download them one by one
    blobs = blob_service.list_blobs(CONTAINER_NAME)
    for blob in blobs:
        local_file = join(LOCAL_DIRECT, blob.name)
        try:
            blob_service.get_blob_to_path(CONTAINER_NAME, blob.name, local_file)
        except:
            print "something wrong happened when downloading the data %s"%blob.name
