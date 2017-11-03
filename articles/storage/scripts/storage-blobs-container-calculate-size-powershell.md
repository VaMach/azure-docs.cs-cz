---
title: "Azure skript prostředí PowerShell ukázkový – vypočítat velikost kontejneru objektů blob | Microsoft Docs"
description: "Vypočítejte velikost kontejneru v úložiště objektů Blob v Azure pomocí součtem velikost každého z jeho objekty BLOB."
services: storage
documentationcenter: na
author: fhryo-msft
manager: cbrooks
editor: tysonn
ms.assetid: 
ms.custom: mvc
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: powershell
ms.topic: sample
ms.date: 10/23/2017
ms.author: fryu
ms.openlocfilehash: 46f3eac0129da41062caba2da090f9e532505d67
ms.sourcegitcommit: b979d446ccbe0224109f71b3948d6235eb04a967
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/25/2017
---
# <a name="calculate-the-size-of-a-blob-storage-container"></a>Vypočítat velikost kontejner úložiště objektů Blob

Tento skript se vypočítá velikost kontejner v úložišti objektů Blob Azure součtem velikost objektů BLOB v kontejneru.

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="understand-the-size-of-blob-storage-container"></a>Pochopení velikost kontejner úložiště objektů Blob

Celková velikost kontejner úložiště objektů Blob zahrnuje, velikost kontejneru sám a velikost všech objektů BLOB v kontejneru.

Následující text popisuje, jak je vypočtena úložnou kapacitu pro kontejnery objektů Blob a objekty BLOB. V nižší než Len(X) znamená počet znaků v řetězci.

### <a name="blob-containers"></a>Kontejnery objektů BLOB

Toto je postup odhadnout velikost úložiště na kontejner objektů blob:

`
48 bytes + Len(ContainerName) * 2 bytes +
For-Each Metadata[3 bytes + Len(MetadataName) + Len(Value)] +
For-Each Signed Identifier[512 bytes]
`

Toto je rozdělení:
* 48 bajtů režie pro každý kontejner obsahuje čas poslední změny, oprávnění, nastavení veřejných a některá metadata systému.
* Název kontejneru je uložena, protože kódování Unicode trvat počet znaků a vynásobit 2.
* Pro každý metadata kontejneru objektu blob uložené uložíme délka názvu (uložené ve formátu ASCII) plus délka hodnotu řetězce.
* 512 bajtů na podepsané identifikátor zahrnuje název podepsaný identifikátoru, času spuštění, čas vypršení platnosti a oprávnění.

### <a name="blobs"></a>Objekty blob

Toto je postup odhadnout velikost úložiště na objekt blob:

* Objekt Blob bloku (základní objekt blob nebo snímek)

`
124 bytes + Len(BlobName) * 2 bytes +
For-Each Metadata[3 bytes + Len(MetadataName) + Len(Value)] +
8 bytes + number of committed and uncommitted blocks * Block ID Size in bytes +
SizeInBytes(data in unique committed data blocks stored) +
SizeInBytes(data in uncommitted data blocks)
`

* Objekt Blob stránky (základní objekt blob nebo snímek)

`
124 bytes + Len(BlobName) * 2 bytes +
For-Each Metadata[3 bytes + Len(MetadataName) + Len(Value)] +
number of nonconsecutive page ranges with data * 12 bytes +
SizeInBytes(data in unique pages stored)
`

Toto je rozdělení:

* 124 bajtů režie pro objekt blob, což zahrnuje času poslední změny, velikost, Cache-Control, Content-Type, Content-Language, kódování obsahu, MD5 obsah, oprávnění, snímku informace, zapůjčení a některá metadata systému.
* Název objektu blob je uloženo jako Unicode trvat tak počet znaků a více ve 2.
* Potom pro každou metadata uloží, délka názvu (uložené ve formátu ASCII) plus délka hodnotu řetězce.
* Pak pro objekty BLOB bloku
    * 8 bajtů pro do seznamu zakázaných položek
    * Počet bloků s časy velikost bloku ID v bajtech
    * Plus velikost dat ve všech potvrdit a nepotvrzené bloky. Poznámka: Pokud použijete snímky, tato velikost obsahuje pouze jedinečné data pro tento objekt blob základní nebo snímek. Pokud po týdnu se nepoužívají nepotvrzené bloky, budou uvolnění z paměti, a pak v ten moment se bude už započítávat fakturace, po který.
* Pak pro objekty BLOB stránky
    * Počet rozsahů nejdou stránek s daty případech 12 bajtů. Toto je počet rozsahů jedinečný stránek, které se zobrazí při volání rozhraní API GetPageRanges.
    * Plus velikost dat v bajtech všechny uložené stránky. Poznámka: Pokud se používá snímky, tato velikost pouze obsahuje jedinečný stránek pro základní objekt blob nebo objekt blob snímku se počítá.

## <a name="sample-script"></a>Ukázkový skript

[!code-powershell[main](../../../powershell_scripts/storage/calculate-container-size/calculate-container-size-ex.ps1 "Calculate container size")]

## <a name="next-steps"></a>Další kroky

Další informace o fakturaci úložiště Azure najdete v tématu [Principy Windows Azure Storage fakturace](https://blogs.msdn.microsoft.com/windowsazurestorage/2010/07/08/understanding-windows-azure-storage-billing-bandwidth-transactions-and-capacity/).

Další informace o modulu Azure PowerShell najdete v tématu [dokumentace Azure PowerShell](/powershell/azure/overview).

Ukázky skriptu PowerShell další úložiště naleznete v [ukázky prostředí PowerShell pro Azure Storage](../blobs/storage-samples-blobs-powershell.md).
