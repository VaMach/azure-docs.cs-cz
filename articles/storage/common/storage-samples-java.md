---
title: "Ukázek Azure Storage pomocí Java | Microsoft Docs"
description: "Zobrazit, stáhnout a spustit ukázkový kód a aplikace pro Azure Storage. Umožňuje zjistit Začínáme ukázky pro objekty BLOB, fronty, tabulky a soubory, pomocí knihovny klienta úložiště Java."
services: storage
documentationcenter: na
author: seguler
manager: jahogg
editor: tysonn
ms.assetid: 
ms.service: storage
ms.devlang: java
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage
ms.date: 01/12/2017
ms.author: seguler
ms.openlocfilehash: fd27e1ac9a773e7b0f5245aa74acdb0521cd098c
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="azure-storage-samples-using-java"></a>Ukázek Azure Storage používá Java

## <a name="java-sample-index"></a>Ukázka index Java

Následující tabulka obsahuje přehled o našem úložišti ukázky a pokryté v každé ukázkové scénáře. Kliknutím na odkazy k zobrazení odpovídající ukázkový kód v Githubu.

<table style="font-size:90%"><thead><tr><th style="font-size:110%">Koncový bod</th><th style="font-size:110%">Scénář</th><th style="font-size:110%">Ukázkový kód</th></tr></thead><tbody> 
<tr> 
<td rowspan="16"><b>Objekt BLOB</b></td>
<td>Append – objekt Blob</td> 
<td><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Začínáme se službou objektů Blob v Azure v jazyce Java</a></td> 
</tr> 
<tr> 
<td>Objekt Blob bloku</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Začínáme se službou objektů Blob v Azure v jazyce Java</a></td>
</tr> 
<tr> 
<td>Šifrování na straně klienta</td>
<td><a href="https://github.com/Azure-Samples/storage-java-client-side-encryption">Začínáme s šifrování na straně klienta Azure v jazyce Java</a></td>
</tr> 
<tr> 
<td>Zkopírování objektu Blob</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Začínáme se službou objektů Blob v Azure v jazyce Java</a></td>
</tr> 
<tr> 
<td>Vytvoření kontejneru</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Začínáme se službou objektů Blob v Azure v jazyce Java</a></td>
</tr> 
<tr> 
<td>Odstranit objekt Blob</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Začínáme se službou objektů Blob v Azure v jazyce Java</a></td>
</tr> 
<tr> 
<td>Odstranit kontejner</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Začínáme se službou objektů Blob v Azure v jazyce Java</a></td>
</tr> 
<tr> 
<td>Objekt BLOB Metadata nebo vlastnosti nebo statistiky</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobAdvanced.java">Začínáme se službou objektů Blob v Azure v jazyce Java</a></td>
</tr> 
<tr> 
<td>Seznam ACL nebo Metadata nebo vlastnosti kontejneru</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobAdvanced.java">Začínáme se službou objektů Blob v Azure v jazyce Java</a></td>
</tr> 
<tr> 
<td>Get rozsahů stránek</td>
<td><a href="https://github.com/Azure/azure-storage-java/blob/master/microsoft-azure-storage-test/src/com/microsoft/azure/storage/blob/CloudPageBlobTests.java">Ukázka testů objekt Blob stránky</a></td>
</tr> 
<tr> 
<td>Zapůjčení nebo kontejner objektů Blob</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Začínáme se službou objektů Blob v Azure v jazyce Java</a></td>
</tr> 
<tr> 
<td>Seznam objektů Blob nebo kontejneru</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Začínáme se službou objektů Blob v Azure v jazyce Java</a></td>
</tr> 
<tr> 
<td>Objekt Blob stránky</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Začínáme se službou objektů Blob v Azure v jazyce Java</a></td>
</tr>
<tr> 
<td>SAS</td>
<td><a href="https://github.com/Azure/azure-storage-java/blob/master/microsoft-azure-storage-test/src/com/microsoft/azure/storage/blob/SasTests.java">Ukázka testů SAS</a></td>
</tr>   
<tr> 
<td>Vlastnosti služby</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobAdvanced.java">Začínáme se službou objektů Blob v Azure v jazyce Java</a></td>
</tr>           
<tr> 
<td>Objekt Blob snímku</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Začínáme se službou objektů Blob v Azure v jazyce Java</a></td>
</tr> 
<tr> 
<td rowspan="9"><b>File</b></td>
<td>Vytvoření sdílené složky, adresáře nebo soubory</td> 
<td><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileBasics.java">Začínáme se službou Azure souboru v jazyce Java</a></td> 
</tr>
<tr> 
<td>Odstranění sdílené složky, adresáře nebo soubory</td> 
<td><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileBasics.java">Začínáme se službou Azure souboru v jazyce Java</a></td> 
</tr> 
<tr> 
<td>Directory vlastnosti nebo Metadata</td> 
<td><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileAdvanced.java">Začínáme se službou Azure souboru v jazyce Java</a></td> 
</tr> 
<tr> 
<td>Stažení souborů</td> 
<td><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileBasics.java">Začínáme se službou Azure souboru v jazyce Java</a></td> 
</tr> 
<tr> 
<td>Soubor vlastnosti nebo Metadata nebo metriky</td> 
<td><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileAdvanced.java">Začínáme se službou Azure souboru v jazyce Java</a></td> 
</tr> 
<tr> 
<td>Vlastnosti souboru služby</td> 
<td><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileAdvanced.java">Začínáme se službou Azure souboru v jazyce Java</a></td> 
</tr> 
<tr> 
<td>Seznam adresářů a souborů</td> 
<td><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileBasics.java">Začínáme se službou Azure souboru v jazyce Java</a></td> 
</tr>
<tr> 
<td>Zobrazit seznam sdílených složek</td> 
<td><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileBasics.java">Začínáme se službou Azure souboru v jazyce Java</a></td> 
</tr>
<tr> 
<td>Sdílené složky vlastnosti nebo Metadata nebo statistiky</td> 
<td><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileAdvanced.java">Začínáme se službou Azure souboru v jazyce Java</a></td> 
</tr>
<tr> 
<td rowspan="8"><b>Fronty</b></td>
<td>Přidat zprávu</td> 
<td><a href="https://github.com/Azure/azure-storage-java/blob/master/microsoft-azure-storage-samples/src/com/microsoft/azure/storage/queue/gettingstarted/QueueBasics.java">Ukázky knihovny klienta Java úložiště</a></td> 
</tr> 
<tr> 
<td>Šifrování na straně klienta</td> 
<td><a href="https://github.com/Azure/azure-storage-java/blob/master/microsoft-azure-storage-samples/src/com/microsoft/azure/storage/encryption/queue/gettingstarted/QueueGettingStarted.java">Ukázky knihovny klienta Java úložiště</a></td> 
</tr> 
<tr> 
<td>Vytvoření fronty</td> 
<td><a href="https://github.com/Azure-Samples/storage-queue-java-getting-started/blob/master/src/QueueBasics.java">Začínáme se službou Azure fronty v jazyce Java</a></td> 
</tr> 
<tr> 
<td>Odstranit nebo fronta zpráv</td> 
<td><a href="https://github.com/Azure-Samples/storage-queue-java-getting-started/blob/master/src/QueueBasics.java">Začínáme se službou Azure fronty v jazyce Java</a></td> 
</tr> 
<tr> 
<td>Prohlížení zpráv</td> 
<td><a href="https://github.com/Azure-Samples/storage-queue-java-getting-started/blob/master/src/QueueBasics.java">Začínáme se službou Azure fronty v jazyce Java</a></td> 
</tr> 
<tr> 
<td>Fronty seznamu ACL nebo Metadata nebo statistiky</td> 
<td><a href="https://github.com/Azure-Samples/storage-queue-java-getting-started/blob/master/src/QueueAdvanced.java">Začínáme se službou Azure fronty v jazyce Java</a></td> 
</tr> 
<tr> 
<td>Vlastnosti fronty služby</td> 
<td><a href="https://github.com/Azure-Samples/storage-queue-java-getting-started/blob/master/src/QueueAdvanced.java">Začínáme se službou Azure fronty v jazyce Java</a></td> 
</tr> 
<tr> 
<td>Zpráva aktualizace</td> 
<td><a href="https://github.com/Azure-Samples/storage-queue-java-getting-started/blob/master/src/QueueBasics.java">Začínáme se službou Azure fronty v jazyce Java</a></td> 
</tr> 
<tr> 
<td rowspan="7"><b>Tabulka</b></td>
<td>Vytvoření tabulky</td> 
<td><a href="https://github.com/Azure-Samples/storage-table-java-getting-started/blob/master/src/TableBasics.java">Začínáme se službou Azure tabulky v jazyce Java</a></td> 
</tr> 
<tr> 
<td>Odstranit Entity nebo tabulku</td> 
<td><a href="https://github.com/Azure-Samples/storage-table-java-getting-started/blob/master/src/TableBasics.java">Začínáme se službou Azure tabulky v jazyce Java</a></td> 
</tr> 
<tr> 
<td>Vložení a sloučení nebo nahrazení Entity</td> 
<td><a href="https://github.com/Azure/azure-storage-java/blob/master/microsoft-azure-storage-samples/src/com/microsoft/azure/storage/table/gettingtstarted/TableBasics.java">Ukázky knihovny klienta Java úložiště</a></td> 
</tr> 
<tr> 
<td>Dotaz entity</td> 
<td><a href="https://github.com/Azure-Samples/storage-table-java-getting-started/blob/master/src/TableBasics.java">Začínáme se službou Azure tabulky v jazyce Java</a></td> 
</tr> 
<tr> 
<td>Dotazu na tabulky</td> 
<td><a href="https://github.com/Azure-Samples/storage-table-java-getting-started/blob/master/src/TableBasics.java">Začínáme se službou Azure tabulky v jazyce Java</a></td> 
</tr> 
<tr> 
<td>Tabulka seznamu ACL nebo vlastnosti</td> 
<td><a href="https://github.com/Azure-Samples/storage-table-java-getting-started/blob/master/src/TableAdvanced.java">Začínáme se službou Azure tabulky v jazyce Java</a></td> 
</tr> 
<tr> 
<td>Aktualizace Entity</td> 
<td><a href="https://github.com/Azure/azure-storage-java/blob/master/microsoft-azure-storage-samples/src/com/microsoft/azure/storage/table/gettingtstarted/TableBasics.java">Ukázky knihovny klienta Java úložiště</a></td> 
</tr> 
</tbody> 
</table>
<br/>

## <a name="azure-code-samples-library"></a>Knihovna Azure ukázky kódu

Chcete-li zobrazit knihovně ucelenou ukázku, přejděte na [ukázky kódu Azure](https://azure.microsoft.com/resources/samples/?service=storage) knihovny, která obsahuje ukázky pro úložiště Azure, které můžete stáhnout a spustit místně. Knihovna ukázka kódu obsahuje ukázkový kód ve formátu ZIP. Alternativně můžete procházet a naklonujte úložiště GitHub pro jednotlivé vzorky.

[!INCLUDE [storage-java-samples-include](../../../includes/storage-java-samples-include.md)]

## <a name="getting-started-guides"></a>Získávání příručky Začínáme

Pokud hledáte pokyny, jak nainstalovat a začít pracovat s knihovny klienta Azure Storage, projděte si následující příručky.

* [Začínáme se službou objektů Blob v Azure v jazyce Java](../blobs/storage-java-how-to-use-blob-storage.md)
* [Začínáme se službou Azure fronty v jazyce Java](../storage-java-how-to-use-queue-storage.md)
* [Začínáme se službou Azure tabulky v jazyce Java](../../cosmos-db/table-storage-how-to-use-java.md)
* [Začínáme se službou Azure souboru v jazyce Java](../storage-java-how-to-use-file-storage.md)

## <a name="next-steps"></a>Další kroky

Informace o ukázky pro jiné jazyky:

* Rozhraní .NET: [ukázky azure Storage pomocí rozhraní .NET](../storage-samples-dotnet.md)
* Všechny ostatní jazyky: [ukázky Azure Storage](../storage-samples.md)
