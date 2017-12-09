---
title: "Zpracování – pořadí událostí a zpoždění pomocí služby Azure Stream Analytics | Microsoft Docs"
description: "Informace o fungování služby Stream Analytics s mimo pořadí nebo pozdní události v datových proudů."
keywords: "mimo provoz, pozdní, události"
documentationcenter: 
services: stream-analytics
author: jseb225
manager: jhubbard
editor: cgronlun
ms.assetid: 
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 04/20/2017
ms.author: jeanb
ms.openlocfilehash: f9854172e08785676a7804433d9a559e623a7b05
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/08/2017
---
# <a name="azure-stream-analytics-event-order-consideration"></a>Azure Stream Analytics událostí pořadí aspekt

## <a name="understand-arrival-time-and-application-time"></a>Porozumět čas doručení a doba aplikace.

V dočasné datovém proudu událostí je přiřazen každé události časového razítka. Azure Stream Analytics přiřadí každé události pomocí čas doručení nebo doba aplikace časové razítko. Sloupce "System.Timestamp" obsahuje časové razítko přiřazené k události. Čas doručení přiřazen za vstupní zdroj, když událost dosáhne zdroji. Čas doručení je EventEnqueuedTime pro vstup centra událostí a [čas poslední změny objektů blob](https://docs.microsoft.com/dotnet/api/microsoft.windowsazure.storage.blob.blobproperties.lastmodified?view=azurestorage-8.1.3) pro vstup objektů blob. Čas aplikace je přiřazena, když se vygeneruje událost a je součástí datové části. Ke zpracování události doba aplikace, použijte klauzuli "Časové razítko podle" v dotazu select. Chybí-li klauzuli "Časové razítko podle", jsou události zpracovává čas doručení. Čas doručení lze přistupovat pomocí vlastnosti EventEnqueuedTime pro centra událostí a pomocí vlastnosti BlobLastModified pro vstup objektů blob. Azure Stream Analytics vytvoří výstupní v pořadí, časové razítko a poskytuje několik nastavení, jak nakládat s daty mimo pořadí.


## <a name="azure-stream-analytics-handling-of-multiple-streams"></a>Azure Stream Analytics zpracování víc datových proudů

Úlohy Azure Stream Analytics kombinuje události z více časové osy v několika případech, včetně,

* Vytváření výstupu z více oddílů. Dotazy, které nemají explicitní "oddílu podle PartitionId" by měla mít kombinovat události ze všech oddílů.
* Sjednocení dvou nebo více různých vstupních zdrojů.
* Připojení vstupního zdroje.

Ve scénářích, kde jsou kombinovat více časových os, Azure Stream Analytics vytvoří výstupní pro časovým razítkem *t1* až po všechny zdroje, které jsou spojené se minimálně v době *t1*.
Například, pokud dotaz načte z *centra událostí* oddílu, který má dva oddíly a jeden z oddílu *P1* má událostí až při *t1* a další oddíl  *P2* má událostí až při *t1 + x*, výsledek až při *t1*.
Ale pokud došlo explicitní *"Oddílu podle PartitionId"* klauzuli oba oddíly hodnoty nezávisle.
Pozdní příchodem Tolerance nastavení se používá k řešení absenci dat v některé oddíly.

## <a name="configuring-late-arrival-tolerance-and-out-of-order-tolerance"></a>Konfigurace pozdní příchodem proti chybám a tolerance mimo pořadí
Vstupní datové proudy, které nejsou v pořadí jsou buď:
* Seřadit (a proto **odložené**).
* Upraví v systému podle zásad definovaných uživatelem.

Stream Analytics toleruje pozdní a mimo pořadí událostí při zpracování **doba aplikace**. Následující nastavení jsou k dispozici v **řazení událostí** možnost na portálu Azure: 

![Stream Analytics zpracování událostí](media/stream-analytics-event-handling/stream-analytics-event-handling.png)

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

* Pozdní tolerance příchodem = 10 minut<br/>
* Mimo pořadí tolerance = 3 minut<br/>
* Zpracování doba aplikace<br/>
* Události:
   * Událost 1 _doba aplikace_ = 00:00:00, _čas doručení_ = 00:10:01 _System.Timestamp_ = 00:00:01, upravit, protože (_čas doručení_  -  _Doba aplikace_) je větší než pozdní příchodem tolerance.
   * Událost 2 _doba aplikace_ = 00:00:01 _čas doručení_ = 00:10:01 _System.Timestamp_ = 00:00:01, není upraven, protože doba aplikace je v rámci pozdní přijetí okno.
   * Událost 3 _doba aplikace_ = 00:10:00, _čas doručení_ = 00:10:02 _System.Timestamp_ = 00:10:00, není upraven, protože souborů čas je v rámci okna pozdní doručení .
   * Událost 4 _doba aplikace_ = 00:09:00, _čas doručení_ = 00:10:03 _System.Timestamp_ = 00:09:00, bere s původní časové razítko, jako je doba aplikace v rámci mimo pořadí proti chybám.
   * Událost 5 _doba aplikace_ = 00:06:00, _čas doručení_ = 00:10:04 _System.Timestamp_ = 00:07:00, upravit, protože doba aplikace je starší než mimo pořadí tolerance.

## <a name="practical-considerations"></a>Praktické informace
Jak je uvedeno nahoře, *pozdní tolerance příchodem* je maximální rozdíl mezi aplikací čas a čas doručení.
Také při zpracování aplikace čas, události, které jsou novější než nakonfigurované *pozdní tolerance příchodem* jsou upravena před *mimo pořadí tolerance* nastavení se použije. Ano platné mimo pořadí je minimálně pozdní příchodem proti chybám a tolerance mimo pořadí.

Události mimo pořadí v rámci datového proudu dojít z důvodů včetně,
* Hodiny zkosení mezi odesílatelů.
* Variabilní latence mezi odesílatele a zdroj vstupní události.

Z důvodů včetně, se stane opožděné předání
* Odesílatelé dávky a odesílat události pro časový interval, později, po uplynutí intervalu.
* Latence mezi událost odesílatel odesílání a příjmu událostí v vstupní zdroj.

Při konfiguraci *pozdní tolerance příchodem* a *mimo pořadí tolerance* pro konkrétní úlohy, správnost, požadavky na latenci a vyšší faktory potřeba vzít v úvahu.

Následuje několik příkladů

### <a name="example-1"></a>Příklad 1: 
Dotaz obsahuje klauzuli "Oddílu podle PartitionId" a v rámci jednoho oddílu, události se posílají pomocí metody synchronní odesílání. Synchronní odesílání metody blok dlouho, dokud se neodešle události.
V takovém případě mimo pořadí totiž nula události jsou zasílány v pořadí s explicitní potvrzení před odesláním další události. Opožděné předání je maximální zpoždění mezi generování události a odesílání událostí + maximální latence mezi odesílatele a vstupní zdroj

### <a name="example-2"></a>Příklad 2:
Dotaz obsahuje klauzuli "Oddílu podle PartitionId" a v rámci jednoho oddílu, události se posílají pomocí metody asynchronní odesílání. Asynchronní odesílání metody můžete spustit více zasílá ve stejnou dobu, což může způsobit události mimo pořadí.
V takovém případě jsou mimo pořadí a pozdní příchodem alespoň Maximální zpoždění mezi generování události a odesílání událostí + maximální latence mezi odesílatele a vstupní zdroj.

### <a name="example-3"></a>Příklad 3:
Dotaz nemá "Oddílu podle PartitionId" a nejsou alespoň dva oddíly.
Konfigurace je stejné jako příklad 2. Však chybí data v jedné z oddílů zpoždění výstup podle dalších * pozdní příchodem tolerance "okno.

## <a name="to-summarize"></a>Vytváření souhrnů
* Pozdní příchodem proti chybám a okno mimo pořadí by měl být nakonfigurovaný podle správnost, požadavky na latenci a měli také zvážit, jak jsou odesílány události.
* Doporučuje se, že mimo pořadí tolerance je menší než pozdní příchodem tolerance.
* Při kombinování více časových os, nedostatku dat v jednom ze zdroje nebo oddíly zpozdit výstup v závislosti další interval tolerance pozdního.

## <a name="get-help"></a>Podpora
Pro další pomoc, vyzkoušejte naše [fórum Azure Stream Analytics](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).

## <a name="next-steps"></a>Další kroky
* [Úvod do služby Stream Analytics](stream-analytics-introduction.md)
* [Začínáme s Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Škálování úlohy Stream Analytics](stream-analytics-scale-jobs.md)
* [Referenční příručka jazyka dotazů Stream Analytics](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Stream Analytics správu odkazu k REST API](https://msdn.microsoft.com/library/azure/dn835031.aspx)
