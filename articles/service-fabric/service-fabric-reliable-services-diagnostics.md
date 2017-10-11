---
title: "Stavová diagnostiky spolehlivé služby | Microsoft Docs"
description: "Diagnostické funkce pro stavové služby Reliable Services"
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: 
ms.assetid: ae0e8f99-69ab-4d45-896d-1fa80ed45659
ms.service: Service-Fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/30/2017
ms.author: dekapur
ms.openlocfilehash: 63a7707f16bbf037c0c91da1d02093e2314dc06e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="diagnostic-functionality-for-stateful-reliable-services"></a>Diagnostické funkce pro stavové služby Reliable Services
Třída Stateful spolehlivé služby StatefulServiceBase vysílá [EventSource](https://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource.aspx) události, které slouží k ladění služby, poskytují přehled o tom, jak je modul runtime provoz a pomáhají při řešení problémů.

## <a name="eventsource-events"></a>EventSource události
EventSource název pro třídu Stateful spolehlivé služby StatefulServiceBase je "Služba společnosti Microsoft ServiceFabric". Události z tohoto zdroje událostí se zobrazí ve [diagnostiky události](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md#view-service-fabric-system-events-in-visual-studio) okno při službu, která má být [ladit v sadě Visual Studio](service-fabric-debugging-your-application.md).

Příklady nástroje a technologie, které pomáhají v shromažďování nebo zobrazování událostí EventSource [nástroje PerfView](http://www.microsoft.com/download/details.aspx?id=28567), [Microsoft Azure Diagnostics](../cloud-services/cloud-services-dotnet-diagnostics.md)a [Microsoft TraceEvent Library](http://www.nuget.org/packages/Microsoft.Diagnostics.Tracing.TraceEvent).

## <a name="events"></a>Události
| Název události | ID události | Úroveň | Popis události |
| --- | --- | --- | --- |
| StatefulRunAsyncInvocation |1 |Informační |Když se služba RunAsync úloha je spuštěna. |
| StatefulRunAsyncCancellation |2 |Informační |Když se služba RunAsync úloha byla zrušena |
| StatefulRunAsyncCompletion |3 |Informační |Když se po dokončení úkolu RunAsync služby |
| StatefulRunAsyncSlowCancellation |4 |Upozornění |Když se úloha RunAsync služby trvá příliš dlouho dokončení zrušení |
| StatefulRunAsyncFailure |5 |Chyba |Když se vyvolá výjimku, úloha RunAsync služby |

## <a name="interpret-events"></a>Interpretace události
Je užitečné do zapisovače služby pochopit životní cyklus služby--, jakož i časování při zahájení, zrušena nebo byla dokončena služby StatefulRunAsyncInvocation, StatefulRunAsyncCompletion a StatefulRunAsyncCancellation události. To může být užitečné při ladění problémů služby nebo pochopení životního cyklu služby.

Služba zapisovače měli věnovat pozornost zavřít StatefulRunAsyncSlowCancellation a StatefulRunAsyncFailure události, protože indikují problémy se službou.

StatefulRunAsyncFailure jsou vydávány vždy, když úloha RunAsync() služby vyvolá výjimku. Výjimka vyvolaná obvykle označuje chyby nebo chyby ve službě. Kromě toho výjimka způsobí selhání, služby, se přesune do jiného uzlu. To může být náročná operace a můžou zdržet příchozí požadavky během přesouvání služby. Služba zapisovače by měl zjistit příčinu výjimky a pokud je to možné, ji zmírnit.

StatefulRunAsyncSlowCancellation jsou vydávány vždy, když na žádost o zrušení úlohy RunAsync trvá déle než 4 sekundy. Pokud služby trvá příliš dlouho dokončení zrušení, má vliv schopnost služby rychle restartování v jiném uzlu. To může mít vliv na celkovou dostupnost služby.

## <a name="next-steps"></a>Další kroky
* [Zprostředkovatelé EventSource v nástroje PerfView](https://blogs.msdn.microsoft.com/vancem/2012/07/09/introduction-tutorial-logging-etw-events-in-c-system-diagnostics-tracing-eventsource/)
