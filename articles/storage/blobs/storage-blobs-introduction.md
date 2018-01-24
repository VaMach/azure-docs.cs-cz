---
title: "Úvod do Azure Blob storage | Microsoft Docs"
description: "Úvod do Azure Blob storage"
services: storage
documentationcenter: 
author: tamram
manager: timlt
editor: tysonn
ms.assetid: 
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/28/2017
ms.author: tamram
ms.openlocfilehash: 0097f1c02b88343a135b6489130a6e0d35cf6fba
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/23/2018
---
# <a name="introduction-to-blob-storage"></a>Úvod do úložiště objektů Blob

Azure Blob Storage je služba, která slouží k ukládání velkých objemů nestrukturovaných dat objektů, například textu nebo binárních dat, která jsou přístupná odkudkoli na světě prostřednictvím protokolů HTTP nebo HTTPS. Službu Blob Storage můžete používat ke zveřejňování dat pro celý svět, nebo k soukromému ukládání dat aplikací.

Mezi běžná použití služby Blob Storage patří:

* poskytování obrázků nebo dokumentů přímo do prohlížeče
* ukládání souborů pro distribuovaný přístup
* streamování videa a zvuku
* ukládání dat pro zálohování a obnovování, zotavení po havárii a pro archivaci
* ukládání dat, které bude analyzovat místní nebo v Azure hostovaná služba

## <a name="blob-service-concepts"></a>Koncepty služby Blob service

Služba Blob service obsahuje následující součásti:

![Architektura objektu blob](./media/storage-blobs-introduction/blob1.png)

* **Účet úložiště:** Veškerý přístup k úložišti Azure se provádí prostřednictvím účtu úložiště. Tento účet úložiště může být **účtu úložiště pro obecné účely (v1 nebo v2)** nebo **účty úložiště Blob**. Další informace najdete v tématu [Účty Azure Storage](../common/storage-create-storage-account.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).

* **Kontejner:** Kontejner zajišťuje seskupení sady objektů blob. Všechny objekty blob musí být v kontejneru. Účet může obsahovat neomezený počet kontejnerů. Kontejner můžete pojmout neomezený počet objektů blob. Všimněte si, že název kontejneru musí být psaný malými písmeny.

* **Objekt blob:** Soubor libovolného typu a velikosti. Azure Storage nabízí tři typy objektů blob: blokovat objekty BLOB, [stránek](storage-blob-pageblob-overview.md)a doplňovací objekty BLOB.
  
    *Objekty blob bloků* jsou ideální pro ukládání textových nebo binárních souborů, například dokumentů a multimediálních souborů. *Doplňovací objekty blob* jsou podobné objektům blob bloku v tom, že je tvoří bloky, ale jsou optimalizované pro doplňovací operace, takže se hodí pro scénáře protokolování. Jeden objekt blob bloku může obsahovat až 50 000 bloků, každý o velikosti až 100 MB, v celkové velikosti o něco větší než 4,75 TB (100 MB × 50 000). Jeden doplňovací objekt blob může obsahovat až 50 000 bloků, každý o velikosti až 4 MB, v celkové velikosti o něco větší než 195 GB (4 MB × 50 000).
  
    *Objekty BLOB stránek* může být až 8 TB a jsou efektivnější pro časté čtení/zápisu operace. Služba Azure Virtual Machines používá objekty blob stránek jako disky pro operační systém a ukládání dat.
  
    Podrobnosti o vytváření názvů kontejnerů a objektů blob najdete v článku [Názvy kontejnerů, objektů blob a metadat a odkazování na ně](/rest/api/storageservices/Naming-and-Referencing-Containers--Blobs--and-Metadata).

## <a name="next-steps"></a>Další postup

* [vytvořit účet úložiště](../common/storage-create-storage-account.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)
* [Začínáme s úložištěm Blob pomocí rozhraní .NET](storage-dotnet-how-to-use-blobs.md)
* [Ukázek Azure Storage pomocí rozhraní .NET](../common/storage-samples-dotnet.md)
* [Ukázek Azure Storage používá Java](../common/storage-samples-java.md)
