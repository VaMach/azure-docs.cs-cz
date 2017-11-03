---
title: "Zpracování – pořadí událostí a zpoždění pomocí služby Azure Stream Analytics | Microsoft Docs"
description: "Informace o fungování služby Stream Analytics s mimo pořadí nebo pozdní události v datových proudů."
keywords: "mimo provoz, pozdní, události"
documentationcenter: 
services: stream-analytics
author: samacha
manager: jhubbard
editor: cgronlun
ms.assetid: 
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 04/20/2017
ms.author: samacha
ms.openlocfilehash: cf57bd12a62b3de8ac49b26ce7cdc40aec0b6738
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="azure-stream-analytics-event-order-handling"></a>Zpracování pořadí událostí služby Azure Stream Analytics

V dočasné datovém proudu událostí je přiřazen každé události časového razítka. Azure Stream Analytics přiřadí každé události pomocí čas doručení nebo doba aplikace časové razítko. Sloupce "System.Timestamp" obsahuje časové razítko přiřazené k události. Čas doručení přiřazen za vstupní zdroj, když událost dosáhne zdroji. Čas doručení je EventEnqueuedTime pro vstup centra událostí a [čas poslední změny objektů blob](https://docs.microsoft.com/en-us/dotnet/api/microsoft.windowsazure.storage.blob.blobproperties.lastmodified?view=azurestorage-8.1.3) pro vstup objektů blob. Čas aplikace je přiřazena, když se vygeneruje událost a je součástí datové části. Ke zpracování události doba aplikace, použijte klauzuli "Časové razítko podle" v dotazu select. Chybí-li klauzuli "Časové razítko podle", jsou události zpracovává čas doručení. Čas doručení lze přistupovat pomocí vlastnosti EventEnqueuedTime pro centra událostí a pomocí vlastnosti BlobLastModified pro vstup objektů blob. Azure Stream Analytics vytvoří výstupní v pořadí, časové razítko a poskytuje několik nastavení, jak nakládat s daty mimo pořadí.

![Stream Analytics zpracování událostí](media/stream-analytics-event-handling/stream-analytics-event-handling.png)

Vstupní datové proudy, které nejsou v pořadí jsou buď:
* Seřadit (a proto **odložené**).
* Upraví v systému podle zásad definovaných uživatelem.

Stream Analytics toleruje pozdní a mimo pořadí událostí při zpracování aplikace doby.

**Pozdní příchodem toleranci**

* Toto nastavení platí jenom v případě, že to tak, že doba aplikace, v opačném případě je ignorován.
* Toto je maximální rozdíl mezi čas doručení a aplikace. Pokud je aplikace doba před (čas doručení - pozdní příchodem okno), je nastavený na (čas doručení - pozdní příchodem okna)
* Společně se kombinaci více oddílů ze stejného vstupního datového proudu nebo více vstupní datové proudy, pozdní příchodem tolerance je maximální množství času, které každý oddíl čeká na nová data. 

Stručně řečeno pozdní příchodem okno se jako maximální zpoždění mezi vytvořením událostí a přijímání událostí v vstupní zdroj.
Nejprve provádí úpravu podle pozdní tolerance doručení a mimo pořadí se provádí další. **System.Timestamp** sloupce budou mít poslední časové razítko, které jsou přiřazené k události.

**Tolerance mimo pořadí**

* Události, které přicházejí mimo pořadí, ale v rámci sady "mimo pořadí tolerance okno" **přeuspořádány pomocí časového razítka**. 
* Události, které přicházejí později než proti chybám jsou **vyřadit nebo upravena**.
    * **Upraví**: upravený tak, aby se zobrazí určeny na nejnovější přijatelný čas. 
    * **Vyřadit**: zahozeny.

Chcete-li změnit pořadí událostí přijatých v rámci "mimo pořadí tolerance okno", je výstup dotazu **zpožděn interval tolerance mimo pořadí**.

**Příklad**

Pozdní tolerance příchodem = 10 minut<br/>
Mimo pořadí tolerance = 3 minut<br/>
Zpracování doba aplikace<br/>

Události:

Událost 1 _doba aplikace_ = 00:00:00, _čas doručení_ = 00:10:01 _System.Timestamp_ = 00:00:01, upravit, protože (_čas doručení_  -  _Doba aplikace_) je větší než pozdní příchodem tolerance.

Událost 2 _doba aplikace_ = 00:00:01 _čas doručení_ = 00:10:01 _System.Timestamp_ = 00:00:01, není upraven, protože doba aplikace je v rámci pozdní přijetí okno.

Událost 3 _doba aplikace_ = 00:10:00, _čas doručení_ = 00:10:02 _System.Timestamp_ = 00:10:00, není upraven, protože souborů čas je v rámci okna pozdní doručení .

Událost 4 _doba aplikace_ = 00:09:00, _čas doručení_ = 00:10:03 _System.Timestamp_ = 00:09:00, bere s původní časové razítko, jako je doba aplikace v rámci mimo pořadí proti chybám.

Událost 5 _doba aplikace_ = 00:06:00, _čas doručení_ = 00:10:04 _System.Timestamp_ = 00:07:00, upravit, protože doba aplikace je starší než mimo pořadí tolerance.



## <a name="get-help"></a>Podpora
Pro další pomoc, vyzkoušejte naše [fórum Azure Stream Analytics](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).

## <a name="next-steps"></a>Další kroky
* [Úvod do služby Stream Analytics](stream-analytics-introduction.md)
* [Začínáme s Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Škálování úlohy Stream Analytics](stream-analytics-scale-jobs.md)
* [Referenční příručka jazyka dotazů Stream Analytics](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Stream Analytics správu odkazu k REST API](https://msdn.microsoft.com/library/azure/dn835031.aspx)
