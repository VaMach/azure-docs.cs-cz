---
title: "Vyvolat Chaos v prostředí clusterů Service Fabric | Microsoft Docs"
description: "Použití clusteru Analysis Service API a pravděpodobnost vkládání ke správě Chaos v clusteru."
services: service-fabric
documentationcenter: .net
author: motanv
manager: anmola
editor: motanv
ms.assetid: 2bd13443-3478-4382-9a5a-1f6c6b32bfc9
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/09/2017
ms.author: motanv
ms.openlocfilehash: 3b3b93bc9ec5ecdcfc289e5b62e84de6aa4172ed
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="induce-controlled-chaos-in-service-fabric-clusters"></a>Vyvolat řízené Chaos v prostředí clusterů Service Fabric
Ve velkém měřítku distribuovaných systémů, jako jsou ze své podstaty nespolehlivé cloudových infrastruktur. Azure Service Fabric umožňuje vývojářům psát spolehlivé distribuované služby nespolehlivé infrastruktuře. Zápis robustní distribuované služby nespolehlivé infrastruktuře, vývojáři potřeba otestovat stability svých služeb, při odpovídající nespolehlivé infrastruktury prochází přes přechodů mezi stavy složité z důvodu chyb.

[Vkládání selhání a clusteru Analysis Service](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-testability-overview) (také označované jako služba analýza selhání) poskytuje vývojářům možnost způsobit chyby k otestování svých služeb. Tyto cílové simulated chyb, jako je třeba [restartování oddíl](https://docs.microsoft.com/en-us/powershell/module/servicefabric/start-servicefabricpartitionrestart?view=azureservicefabricps), může pomoci prověření nejběžnější přechodů mezi stavy. Ale cílové simulované chyb jsou tendenční podle definice a proto může dojít chyby které zobrazují se pouze v pevném předpovědi, dlouhé a komplikované posloupnost přechodů mezi stavy. Pro neposunutého testování, můžete použít Chaos.

Chaos simuluje pravidelné, prokládaná chyb (řádně i vynuceném) v rámci clusteru přes dlouhou dobu. Po konfiguraci Chaos s rychlost a druh chyb, můžete začít Chaos prostřednictvím jazyka C# nebo rozhraní API prostředí Powershell spustit generování chyb v clusteru a vaše služby. Chaos spuštění pro zadané časové období (například pro jednu hodinu), po které zastaví Chaos lze nastavit automaticky nebo můžete volat rozhraní API StopChaos (C# nebo prostředí Powershell) pro zastavení kdykoli.

> [!NOTE]
> V současné podobě Chaos indukuje pouze bezpečné chyb, což znamená, že chybí externí chyb ztrátě kvora nebo ztrátě dat se nikdy neprovádí.
>

Je spuštěn Chaos, vyvolá různé události, které zaznamenat stav spuštění v tuto chvíli. Například ExecutingFaultsEvent obsahuje všechny chyb, které se rozhodla Chaos provést v této iteraci. ValidationFailedEvent obsahuje podrobnosti o selhání ověření (stavu nebo stabilitu problémy), který byl nalezen během ověření clusteru. Rozhraní API GetChaosReport (C# nebo prostředí Powershell) Chcete-li získat sestavu Chaos spustí můžete vyvolat. Tyto události získat uchovávané v [spolehlivé slovník](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-reliable-services-reliable-collections), který má zásady zkrácení závisí na dvě konfigurace: **MaxStoredChaosEventCount** (výchozí hodnota je 25 000) a **StoredActionCleanupIntervalInSeconds** (výchozí hodnota je 3600). Každý *StoredActionCleanupIntervalInSeconds* Chaos kontroly a všechny, ale nejnovější *MaxStoredChaosEventCount* události, jsou vymazány ze slovníku spolehlivé.

## <a name="faults-induced-in-chaos"></a>Vyvolané v Chaos chyb
Chaos generuje chyby napříč celý cluster Service Fabric a komprimaci chyb, které jsou zobrazená v měsíců nebo let do několik hodin. Kombinace prokládaná chyb s vysokou odolnost rychlost vyhledá náročnějších případech, které jinak může být načteni. Toto cvičení Chaos vede k významné zlepšení kvality kódu služby.

Chaos indukuje chyb z následujících kategorií:

* Restartovat uzel
* Restartujte nasazený balíček kódu
* Odstranění repliky
* Restartujte repliky
* Přesunutí primární repliky (Konfigurovat)
* Přesunutí sekundární repliky (Konfigurovat)

Chaos běží ve více iterací. Každé iteraci se skládá z chyb a ověření clusteru pro toto období. Můžete nakonfigurovat čas strávený clusteru stabilizovat a pro ověření úspěšné. Pokud selhání naleznete v ověření clusteru, Chaos generuje a přetrvává ValidationFailedEvent s časovým razítkem UTC a podrobnosti o chybě. Představte si třeba instanci Chaos, který je nastaven na spouštění jednu hodinu s maximálně tři souběžných chyb. Chaos indukuje tři chyb a poté ověří stav clusteru. Ho iteruje předchozí krok, dokud není explicitně zastaven prostřednictvím rozhraní API StopChaosAsync nebo hodinových předá. Pokud cluster se změní na není v pořádku v kteroukoli iteraci (to znamená, že ho není stabilizovat v rámci MaxClusterStabilizationTimeout předané), Chaos generuje ValidationFailedEvent. Tato událost označuje, že něco nepovede a může být třeba další šetření.

Získat chyb, které Chaos vyvolané, můžete použít rozhraní API GetChaosReport (prostředí powershell nebo C#). Rozhraní API získá další segment Chaos sestavy na základě token pro pokračování předané nebo předané čas rozsahu. Můžete buď zadat ContinuationToken získat další segment Chaos sestavy nebo můžete zadat časový rozsah prostřednictvím StartTimeUtc a EndTimeUtc, ale ContinuationToken a časový rozsah nelze zadat ve stejném volání. Pokud nejsou k dispozici více než 100 Chaos události, Chaos sestavy je vrácený v segmenty, kde segment obsahuje více než 100 Chaos událostí.

## <a name="important-configuration-options"></a>Důležité konfigurační možnosti
* **TimeToRun**: celkový čas která Chaos spouští před dokončením s úspěch. Předtím, než byl spuštěn po dobu TimeToRun prostřednictvím rozhraní API StopChaos můžete zastavit Chaos.

* **MaxClusterStabilizationTimeout**: maximální množství času čekání na clusteru se nezotavila před generovala ValidationFailedEvent. Toto čekání je ke snížení zatížení v clusteru, zatímco probíhá obnovení. Provést kontroly jsou:
  * Pokud stav clusteru je v pořádku
  * Pokud stav služby je v pořádku.
  * Pokud cílový repliky nastavit velikost se dá dosáhnout oddílu služby
  * Že neexistuje žádná replik InBuild
* **MaxConcurrentFaults**: maximální počet souběžných chyb, které jsou v každé iteraci vyvolané. Je vyšší číslo, tím agresivnější Chaos a převzetí služeb při selhání a kombinace přechod stavu, které procházejí clusteru jsou i složitější. 

> [!NOTE]
> Bez ohledu na to jak velkou hodnotu *MaxConcurrentFaults* má Chaos zaručuje – chybí externí chyb - neexistuje ztrátě kvora nebo ztrátě dat.
>

* **EnableMoveReplicaFaults**: Povolí nebo zakáže chyb, které způsobují primární nebo sekundární repliky přesunout. Tyto chyby jsou ve výchozím nastavení zakázány.
* **WaitTimeBetweenIterations**: dobu čekání mezi iterací. To znamená množství času Chaos se pozastaví po provedení zaokrouhlit chyb a nutnosti dokončení odpovídající ověření stavu clusteru. Čím vyšší hodnota, čím nižší je míra vkládání průměrná selhání.
* **WaitTimeBetweenFaults**: dobu čekání mezi dvě po sobě jdoucích chyb v jednom iterací. Vyšší hodnota, čím nižší současnému (nebo překryv mezi) chyb.
* **ClusterHealthPolicy**: zásady stavu clusteru se používá k ověření stavu clusteru mezi Chaos iterací. Pokud stav clusteru došlo k chybě nebo pokud se stane neočekávané výjimce během zpracování chyby, budou Chaos Počkejte 30 minut před další-kontrolou stavu - poskytnout chvíli recuperate clusteru.
* **Kontext**: kolekce (řetězec, řetězec) zadejte páry klíč hodnota. Mapy slouží k zaznamenání informací o Chaos spustit. Nemůže být více než 100 tyto dvojice a každý řetězec (klíč nebo hodnota) může mít maximálně povolených 4095 znaků dlouhé. Tato mapa je nastavena podle starter zmatku spustit, aby se volitelně ukládat kontext o konkrétní spustit.

## <a name="how-to-run-chaos"></a>Jak spustit Chaos

```csharp
using System;
using System.Collections.Generic;
using System.Threading.Tasks;
using System.Fabric;

using System.Diagnostics;
using System.Fabric.Chaos.DataStructures;

class Program
{
    private class ChaosEventComparer : IEqualityComparer<ChaosEvent>
    {
        public bool Equals(ChaosEvent x, ChaosEvent y)
        {
            return x.TimeStampUtc.Equals(y.TimeStampUtc);
        }

        public int GetHashCode(ChaosEvent obj)
        {
            return obj.TimeStampUtc.GetHashCode();
        }
    }

    static void Main(string[] args)
    {
        var clusterConnectionString = "localhost:19000";
        using (var client = new FabricClient(clusterConnectionString))
        {
            var startTimeUtc = DateTime.UtcNow;
            var stabilizationTimeout = TimeSpan.FromSeconds(30.0);
            var timeToRun = TimeSpan.FromMinutes(60.0);
            var maxConcurrentFaults = 3;

            var parameters = new ChaosParameters(
                stabilizationTimeout,
                maxConcurrentFaults,
                true, /* EnableMoveReplicaFault */
                timeToRun);

            try
            {
                client.TestManager.StartChaosAsync(parameters).GetAwaiter().GetResult();
            }
            catch (FabricChaosAlreadyRunningException)
            {
                Console.WriteLine("An instance of Chaos is already running in the cluster.");
            }

            var filter = new ChaosReportFilter(startTimeUtc, DateTime.MaxValue);

            var eventSet = new HashSet<ChaosEvent>(new ChaosEventComparer());

            while (true)
            {
                var report = client.TestManager.GetChaosReportAsync(filter).GetAwaiter().GetResult();

                foreach (var chaosEvent in report.History)
                {
                    if (eventSet.Add(chaosEvent))
                    {
                        Console.WriteLine(chaosEvent);
                    }
                }

                // When Chaos stops, a StoppedEvent is created.
                // If a StoppedEvent is found, exit the loop.
                var lastEvent = report.History.LastOrDefault();

                if (lastEvent is StoppedEvent)
                {
                    break;
                }

                Task.Delay(TimeSpan.FromSeconds(1.0)).GetAwaiter().GetResult();
            }
        }
    }
}
```

```powershell
$connection = "localhost:19000"
$timeToRun = 60
$maxStabilizationTimeSecs = 180
$concurrentFaults = 3
$waitTimeBetweenIterationsSec = 60

Connect-ServiceFabricCluster $connection

$events = @{}
$now = [System.DateTime]::UtcNow

Start-ServiceFabricChaos -TimeToRunMinute $timeToRun -MaxConcurrentFaults $concurrentFaults -MaxClusterStabilizationTimeoutSec $maxStabilizationTimeSecs -EnableMoveReplicaFaults -WaitTimeBetweenIterationsSec $waitTimeBetweenIterationsSec

while($true)
{
    $stopped = $false
    $report = Get-ServiceFabricChaosReport -StartTimeUtc $now -EndTimeUtc ([System.DateTime]::MaxValue)

    foreach ($e in $report.History) {

        if(-Not ($events.Contains($e.TimeStampUtc.Ticks)))
        {
            $events.Add($e.TimeStampUtc.Ticks, $e)
            if($e -is [System.Fabric.Chaos.DataStructures.ValidationFailedEvent])
            {
                Write-Host -BackgroundColor White -ForegroundColor Red $e
            }
            else
            {
                if($e -is [System.Fabric.Chaos.DataStructures.StoppedEvent])
                {
                    $stopped = $true
                }

                Write-Host $e
            }
        }
    }

    if($stopped -eq $true)
    {
        break
    }

    Start-Sleep -Seconds 1
}

Stop-ServiceFabricChaos
```
