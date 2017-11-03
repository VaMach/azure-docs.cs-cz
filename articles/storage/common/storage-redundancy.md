---
title: Replikace dat v Azure Storage | Microsoft Docs
description: "Data ve vašem účtu úložiště Microsoft Azure se replikují pro odolnost a vysokou dostupnost. Možnosti replikace zahrnují místně redundantní úložiště (LRS), zónově redundantní úložiště (ZRS), geograficky redundantní úložiště (GRS) a geograficky redundantní úložiště s přístupem pro čtení (RA-GRS)."
services: storage
documentationcenter: 
author: tamram
manager: timlt
editor: tysonn
ms.assetid: 86bdb6d4-da59-4337-8375-2527b6bdf73f
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/15/2017
ms.author: tamram
ms.openlocfilehash: dbc81edd24ee714fbb173ed395a2f2fc91773fff
ms.sourcegitcommit: d41d9049625a7c9fc186ef721b8df4feeb28215f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/02/2017
---
# <a name="azure-storage-replication"></a>Účet replikace Azure Storage
Data na vašem účtu Microsoft Azure Storage se vždy replikují, aby byla zajištěna jejich stálost a vysoká dostupnost. Replikace zkopíruje data, a to buď v rámci stejného datového centra, nebo do druhého datového centra (v závislosti na možnosti replikace, kterou zvolíte). Replikace chrání vaše data a udrží vaše aplikace v provozu v případě krátkodobého selhání hardwaru. Pokud vaše data se replikují do druhého datového centra, je chráněn v primárním umístění závažné selhání.

Replikace zajišťuje, že váš účet úložiště splňuje [smlouvu o úrovni služeb (SLA) pro Storage](https://azure.microsoft.com/support/legal/sla/storage/) i při selhání. Podívejte se do smlouvy SLA na informace o zárukách služby Azure Storage na stálost a dostupnost.

Při vytvoření účtu úložiště si můžete vybrat jednu z těchto možností replikace:

* [Místně redundantní úložiště (LRS)](#locally-redundant-storage)
* [Zónově redundantní úložiště (ZRS)](#zone-redundant-storage)
* [Geograficky redundantní úložiště (GRS)](#geo-redundant-storage)
* [Geograficky redundantní úložiště s přístupem pro čtení (RA-GRS)](#read-access-geo-redundant-storage)

Geograficky redundantní úložiště s přístupem pro čtení (RA-GRS) je výchozí možností při vytváření účtu úložiště.

Následující tabulka poskytuje rychlý přehled o rozdílech mezi LRS, ZRS, GRS a RA-GRS, zatímco dalších částech adres každý typ replikace podrobněji.

| Strategie replikace | LRS | ZRS | GRS | RA-GRS |
|:--- |:--- |:--- |:--- |:--- |
| Data se replikují přes několik datových center. |Ne |Ano |Ano |Ano |
| Ze sekundární lokality, stejně jako primární umístění můžete číst data. |Ne |Ne |Ne |Ano |
| Počet kopií dat uchovávaných na samostatných uzlech |3 |3 |6 |6 |

V tématu [Azure Storage – ceny](https://azure.microsoft.com/pricing/details/storage/) pro informace o cenách pro možnosti různých redundance.

> [!NOTE]
> Premium Storage podporuje pouze místně redundantní úložiště (LRS). Informace o Premium Storage najdete v tématu [úložiště Premium: vysoce výkonné úložiště pro úlohy virtuálních počítačů Azure](../../virtual-machines/windows/premium-storage.md).
>

## <a name="locally-redundant-storage"></a>Místně redundantní úložiště
[!INCLUDE [storage-common-redundancy-LRS](../../../includes/storage-common-redundancy-LRS.md)]

## <a name="zone-redundant-storage"></a>Zónově redundantní úložiště
Zónově redundantní úložiště (ZRS) asynchronně replikuje data mezi datovými centry v rámci jednoho nebo dvou oblastech kromě ukládání tři repliky podobný LRS, a zajišťuje tak větší odolnost LRS. Data uložená v ZRS byla odolná i v případě, že primární datové centrum není k dispozici nebo neopravitelné.
Zákazníci, kteří v úmyslu používat ZRS měli vědět:

* ZRS je dostupná jenom pro objekty BLOB bloku v účtech úložiště pro obecné účely a je podporována pouze v úložišti služby verze 2014-02-14 a novější.
* Vzhledem k tomu, že asynchronní replikaci zahrnuje zpoždění, v případě havárie místní je možné, že změny, které ještě nebyla replikována na sekundární se ztratí možné obnovit data z primární.
* Repliky nemusí být k dispozici, dokud Microsoft zahájí převzetí služeb při selhání na sekundární.
* ZRS účty nelze převést na LRS nebo GRS později. Podobně existující účet LRS nebo GRS nelze převést na účet ZRS.
* ZRS účty nemají metriky nebo možnosti protokolování.

## <a name="geo-redundant-storage"></a>Geograficky redundantní úložiště
[!INCLUDE [storage-common-redundancy-GRS](../../../includes/storage-common-redundancy-GRS.md)]

## <a name="read-access-geo-redundant-storage"></a>Geograficky redundantní úložiště s přístupem pro čtení
Geograficky redundantní úložiště s přístupem pro čtení (RA-GRS) maximalizuje dostupnost pro váš účet úložiště, tím, že poskytuje přístup jen pro čtení k datům v sekundárním umístění, kromě replikace rámci dvou oblastí poskytované GRS.

Když povolíte přístup jen pro čtení k datům v sekundární oblasti, vaše data jsou k dispozici na sekundárním koncovým bodem, kromě primární koncový bod pro váš účet úložiště. Sekundární koncový bod je podobná primární koncový bod, ale připojí přípona `–secondary` k názvu účtu. Například, pokud je váš primární koncový bod služby objektů Blob `myaccount.blob.core.windows.net`, pak je sekundární koncový bod `myaccount-secondary.blob.core.windows.net`. Přístupové klíče účtu úložiště jsou stejné pro obě primární a sekundární koncové body.

Aspekty:

* Aplikace má ke správě kterému koncovému bodu je interakci s při použití RA-GRS.
* Vzhledem k tomu, že asynchronní replikaci zahrnuje zpoždění, v případě havárie regionální je možné, že od primární oblasti nelze obnovit data budou ztracena změny, které ještě nebyla replikována do sekundární oblasti.
* Pokud Microsoft zahájí převzetí služeb při selhání pro sekundární oblast, budete mít ke čtení a zápis do dat po převzetí služeb po dokončení. Další informace najdete v tématu [pokyny pro zotavení po havárii](../storage-disaster-recovery-guidance.md). 
* RA-GRS, je určena pro účely vysokou dostupnost. Pokyny k škálovatelnost, přečtěte si [kontrolní seznam výkonu](../storage-performance-checklist.md).

## <a name="frequently-asked-questions"></a>Nejčastější dotazy

<a id="howtochange"></a>
#### <a name="1-how-can-i-change-the-geo-replication-type-of-my-storage-account"></a>1. Jak můžete změnit typ geografické replikace pro svůj účet úložiště?

   Můžete změnit typ geografická replikace účtu úložiště mezi LRS, GRS a RA-GRS pomocí [portál Azure](https://portal.azure.com/), [prostředí Azure Powershell](storage-powershell-guide-full.md) nebo programově pomocí jednoho z našich mnoho knihovny klienta úložiště . Upozorňujeme, že účty ZRS nemůže být převedená LRS nebo GRS. Podobně existující účet LRS nebo GRS nelze převést na účet ZRS.

<a id="changedowntime"></a>
#### <a name="2-will-there-be-any-down-time-if-i-change-the-replication-type-of-my-storage-account"></a>2. Bude k dispozici žádný výpadek když změníte typ replikace pro svůj účet úložiště?

   Ne, nebude dostupný žádný výpadek.

<a id="changecost"></a>
#### <a name="3-will-there-be-any-additional-cost-if-i-change-the-replication-type-of-my-storage-account"></a>3. Bude k dispozici žádné další náklady když změníte typ replikace pro svůj účet úložiště?

   Ano. Pokud změníte z LRS GRS (nebo RA-GRS) pro účet úložiště, by to být účtovány dalších poplatků odchozí účastnící se kopírování existujících dat z primárního umístění do sekundárního umístění. Po počáteční data se zkopírují je bezplatná další další odchozí pro geografickou replikaci dat z primárního na sekundární umístění. Podrobnosti o poplatky šířky pásma můžete najít na [Azure Storage – ceny stránky](https://azure.microsoft.com/pricing/details/storage/blobs/). Pokud změníte na LRS GRS, je bez dalších nákladů, ale data se odstraní ze sekundárního umístění.

<a id="ragrsbenefits"></a>
#### <a name="4-how-can-ra-grs-help-me"></a>4. Jak RA-GRS mi může pomoci?
   
   GRS úložiště poskytuje replikaci vašich dat z primárního na sekundární oblasti, která je stovky miles od primární oblasti. V takovém případě vaše data byla odolná i v případě výpadku dokončení místní nebo havárii, ve kterém není použitelná pro obnovení primární oblasti. RA-GRS úložiště zahrnuje to a přidá možnost číst data ze sekundárního umístění. Některé měli představu o tom, jak využít tuto možnost, naleznete v [navrhování vysoce dostupné aplikace pomocí úložiště RA-GRS](../storage-designing-ha-apps-with-ragrs.md). 

<a id="lastsynctime"></a>
#### <a name="5-is-there-a-way-for-me-to-figure-out-how-long-it-takes-to-replicate-my-data-from-the-primary-to-the-secondary-region"></a>5. Existuje způsob, jak mi zjistěte, jak dlouho trvá replikaci Moje dat z primárního na sekundární oblast?
   
   Pokud používáte RA-GRS úložiště, můžete zkontrolovat, čas poslední synchronizace účtu úložiště. Čas poslední synchronizace je hodnota, datum a čas GMT; všechny primární zápisy před čas poslední synchronizace byla úspěšně zapsána do sekundárního umístění, což znamená, že jsou k dispozici čtení ze sekundárního umístění. Primární zapíše po čas poslední synchronizace může nebo nemusí být k dispozici pro čtení ještě. Tato hodnota pomocí se můžete dotazovat [portál Azure](https://portal.azure.com/), [prostředí Azure PowerShell](storage-powershell-guide-full.md), nebo programově pomocí rozhraní REST API nebo jeden z knihovny klienta úložiště. 

<a id="outage"></a>
#### <a name="6-how-can-i-switch-to-the-secondary-region-if-there-is-an-outage-in-the-primary-region"></a>6. Jak lze přepnout sekundární oblast, pokud dojde k výpadku v primární oblasti?
   
   Podrobnosti najdete v článku na [co dělat, když dojde k výpadku Azure Storage](../storage-disaster-recovery-guidance.md) další podrobnosti.

<a id="rpo-rto"></a>
#### <a name="7-what-is-the-rpo-and-rto-with-grs"></a>7. Co je plánovaný bod obnovení a RTO s GRS?
   
   Cíl bodu obnovení (RPO): V GRS a RA-GRS, úložiště služby asynchronně geo replikují data z primární na sekundární umístění. Pokud dojde k havárii hlavní místní a převzetí služeb při selhání musí být provedena, může dojít ke ztrátě poslední rozdílové změny, které nebyly geograficky replikované. Počet minut potenciální – ztráta dat se označuje jako plánovaný bod obnovení (což znamená bod v čase, do kterých můžete obnovit data). Jsme obvykle mají RPO méně než 15 minut, i když je aktuálně trvá žádné SLA na jak dlouho se geografická replikace.

   Cíli času obnovení (RTO): Toto je míra o tom, jak dlouho trvalo nám dělat převzetí služeb při selhání a získat zpět do režimu online účet úložiště, pokud bychom měli udělat převzetí služeb při selhání. Čas převzetí služeb při selhání zahrnují následující:
    * Čas, kdy trvá nám prozkoumat a určete, jestli jsme můžete obnovit data v primární lokalitě nebo pokud je potřeba udělat převzetí služeb při selhání.
    * Převzetí služeb při selhání účet změnou primární záznamy DNS tak, aby odkazovaly do sekundárního umístění.

   Jsme převzít odpovědnost velmi vážně uchování dat tak, aby pokud je pravděpodobné, že obnovení dat, jsme zdržovat provádění převzetí služeb při selhání a zaměřit se na obnovení dat v primárním umístění. V budoucnu plánujeme zajistit rozhraní API, která umožňuje spustit převzetí služeb při selhání na úrovni účtu, který by pak umožňuje kontrolovat RTO sami, ale to ještě není k dispozici.
   
## <a name="next-steps"></a>Další kroky
* [Návrh aplikace s vysokou dostupností pomocí RA-GRS úložiště](../storage-designing-ha-apps-with-ragrs.md)
* [Ceny za Azure Storage](https://azure.microsoft.com/pricing/details/storage/)
* [Informace o účtech úložiště Azure](../storage-create-storage-account.md)
* [Azure Storage škálovatelnosti a cílech výkonnosti](storage-scalability-targets.md)
* [Microsoft Azure Storage redundance možnosti a přístup pro čtení geograficky redundantní úložiště](http://blogs.msdn.com/b/windowsazurestorage/archive/2013/12/11/introducing-read-access-geo-replicated-storage-ra-grs-for-windows-azure-storage.aspx)
* [STUDIE Sosp - Azure Storage: Vysoce dostupný cloudové úložiště služby se silnou konzistenci](http://blogs.msdn.com/b/windowsazurestorage/archive/2011/11/20/windows-azure-storage-a-highly-available-cloud-storage-service-with-strong-consistency.aspx)

