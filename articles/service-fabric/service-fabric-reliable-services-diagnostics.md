---
title: "Azure Service Fabric stavové služby Reliable Services diagnostiky | Microsoft Docs"
description: "Diagnostické funkce pro stavové služby Reliable Services v Azure Service Fabric"
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
ms.date: 10/15/2017
ms.author: dekapur
ms.openlocfilehash: edcaaaf8f1619082b33195aedf1fb1abf32e85b1
ms.sourcegitcommit: 804db51744e24dca10f06a89fe950ddad8b6a22d
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/30/2017
---
# <a name="diagnostic-functionality-for-stateful-reliable-services"></a>Diagnostické funkce pro stavové služby Reliable Services
Třída Azure Service Fabric Stateful spolehlivé služby StatefulServiceBase vysílá [EventSource](https://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource.aspx) události, které slouží k ladění služby, poskytují přehled o tom, jak je modul runtime provoz a pomáhají při řešení problémů.

## <a name="eventsource-events"></a>EventSource události
Název EventSource pro třídu Stateful spolehlivé služby StatefulServiceBase je "Microsoft-ServiceFabric-Services." Události z tohoto zdroje událostí se zobrazí ve [diagnostiky události](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md#view-service-fabric-system-events-in-visual-studio) okno při službu, která má být [ladit v sadě Visual Studio](service-fabric-debugging-your-application.md).

Příklady nástroje a technologie, které pomáhají v shromažďování nebo zobrazování událostí EventSource [nástroje PerfView](http://www.microsoft.com/download/details.aspx?id=28567), [Azure Diagnostics](../cloud-services/cloud-services-dotnet-diagnostics.md)a [Microsoft TraceEvent Library](http://www.nuget.org/packages/Microsoft.Diagnostics.Tracing.TraceEvent).

## <a name="events"></a>Události
| Název události | ID události | Úroveň | Popis události |
| --- | --- | --- | --- |
| StatefulRunAsyncInvocation |1 |Informační |Když se úloha RunAsync služba je spuštěna. |
| StatefulRunAsyncCancellation |2 |Informační |Když se úloha RunAsync služby je zrušená. |
| StatefulRunAsyncCompletion |3 |Informační |Když se úloha RunAsync služby je dokončeno. |
| StatefulRunAsyncSlowCancellation |4 |Upozornění |Když se úloha RunAsync služby trvá příliš dlouho dokončení zrušení |
| StatefulRunAsyncFailure |5 |Chyba |Když se vyvolá výjimku, úloha RunAsync služby |

## <a name="interpret-events"></a>Interpretace události
Je užitečné do zapisovače služby pochopit životní cyklus služby, jakož i časování když služba spustí, zruší nebo dokončení StatefulRunAsyncInvocation, StatefulRunAsyncCompletion a StatefulRunAsyncCancellation události. Tato informace může být užitečné při ladění problémů služby nebo pochopení životního cyklu služby.

Služba zapisovače měli věnovat pozornost zavřít StatefulRunAsyncSlowCancellation a StatefulRunAsyncFailure události, protože indikují problémy se službou.

StatefulRunAsyncFailure jsou vydávány vždy, když úloha RunAsync() služby vyvolá výjimku. Výjimka vyvolaná obvykle označuje chyby nebo chyby ve službě. Kromě toho výjimka způsobí selhání, služby, se přesune do jiného uzlu. Tato operace může být drahé a může pozdržet příchozí požadavky během přesouvání služby. Služba zapisovače by měl zjistit příčinu výjimky a pokud je to možné, ji zmírnit.

StatefulRunAsyncSlowCancellation jsou vydávány vždy, když na žádost o zrušení úlohy RunAsync trvá déle než 4 sekundy. Pokud služby trvá příliš dlouho dokončení zrušení, ovlivní schopnost služby rychle restartována na jiný uzel. Tento scénář může mít vliv na celkovou dostupnost služby.

## <a name="next-steps"></a>Další kroky
[Zprostředkovatelé EventSource v nástroje PerfView](https://blogs.msdn.microsoft.com/vancem/2012/07/09/introduction-tutorial-logging-etw-events-in-c-system-diagnostics-tracing-eventsource/)
