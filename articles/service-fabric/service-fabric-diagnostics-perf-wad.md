---
title: "Azure Service Fabric - s rozšíření Windows Azure Diagnostics pro monitorování výkonu | Microsoft Docs"
description: "Windows Azure Diagnostics použijte ke shromažďování čítače výkonu pro Azure Service Fabric clusterů."
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/05/2017
ms.author: dekapur
ms.openlocfilehash: f71c483eba201eae91c15b84a2ed42fcb68ae575
ms.sourcegitcommit: 6a6e14fdd9388333d3ededc02b1fb2fb3f8d56e5
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/07/2017
---
# <a name="performance-monitoring-with-windows-azure-diagnostics-extension"></a>Monitorování výkonu pomocí rozšíření Windows Azure Diagnostics

Tento dokument popisuje kroky potřebné k nastavení kolekce čítače výkonu prostřednictvím rozšíření Windows Azure Diagnostics (WAD) pro Windows clustery. Nastavení pro Linux clusterů [agenta OMS](service-fabric-diagnostics-oms-agent.md) ke shromažďování čítače výkonu pro uzly. 

 > [!NOTE]
> Rozšíření WAD musí být nasazené na clusteru pro tyto kroky při práci za vás. Pokud není nastavena, přejděte na [agregaci událostí a kolekce s použitím Windows Azure Diagnostics](service-fabric-diagnostics-event-aggregation-wad.md) a postupujte podle kroků v *nasazení rozšíření diagnostiky*.

## <a name="collect-performance-counters-via-the-wadcfg"></a>Shromažďování čítačů výkonu prostřednictvím WadCfg

Shromažďování čítačů výkonu prostřednictvím WAD, budete muset upravit konfiguraci správně v šabloně Resource Manager vašeho clusteru. Postupujte podle těchto kroků přidejte čítače výkonu, které chcete shromažďovat do šablony a spuštění upgradu prostředků Resource Manager.

1. Najít konfiguraci WAD v šabloně váš cluster – najít `WadCfg`. Budete přidávat čítače výkonu v rámci `DiagnosticMonitorConfiguration`.

2. Nastavit konfiguraci ke shromažďování čítačů výkonu přidáním následující části vaší `DiagnosticMonitorConfiguration`. 

    ```json
    "PerformanceCounters": {
        "scheduledTransferPeriod": "PT1M",
        "PerformanceCounterConfiguration": []
    }
    ```

    `scheduledTransferPeriod` Definuje, jak frquently hodnoty čítačů, které byly shromážděny přenesou do tabulky úložiště Azure a žádné nakonfigurované jímky. 

3. Přidání čítačů výkonu chcete shromažďovat k `PerformanceCounterConfiguration` , byla deklarována v předchozím kroku. Jednotlivé čítače, které chcete shromažďovat je definovaná pomocí `counterSpecifier`, `sampleRate`, `unit`, `annotation`a všechny relevantní `sinks`. Tady je příklad čítače výkonu pro *celkový čas procesoru* (množství času procesoru bylo používáno pro operace zpracování):

    ```json
    "PerformanceCounters": {
        "scheduledTransferPeriod": "PT1M",
        "PerformanceCounterConfiguration": [
            {
                "counterSpecifier": "\\Processor(_Total)\\% Processor Time",
                "sampleRate": "PT1M",
                "unit": "Percent",
                "annotation": [
                ],
                "sinks": ""
            }
        ]
    }
    ```

    Vzorkovací frekvence čítače lze upravit podle potřeb. Formát pro něj je `PT<time><unit>`, takže pokud chcete počítadlo shromažďované za sekundu, pak byste měli nastavit `"sampleRate": "PT15S"`.

    Podobně můžete shromáždit několik dalších čítačů výkonu, jejich přidáním do seznamu `PerformanceCounterConfiguration`. I když můžete použít `*` určit skupiny čítačů výkonu, které byly pojmenovány podobně, odesílání sledovat nějaké čítače prostřednictvím jímky (Application Insights) vyžaduje, jsou jednotlivě deklarovány. 

4. Jakmile měl přidat čítače odpovídající výkonu, které musí být shromažďovány, budete muset upgradovat prostředku clusteru tak, aby tyto změny se projeví v spuštěnému clusteru. Uložit vaše změny `template.json` a otevřete prostředí PowerShell. Můžete upgradovat clusteru pomocí `New-AzureRmResourceGroupDeployment`. Volání vyžaduje název skupiny prostředků, soubor aktualizovanou šablonu a soubor parametrů a vyzve správce prostředků prostředky, které jste aktualizovali provádět odpovídající změny. Jakmile jsou přihlášení k účtu a jsou ve správné předplatné, použijte následující příkaz ke spuštění upgradu:

    ```sh
    New-AzureRmResourceGroupDeployment -ResourceGroupName <ResourceGroup> -TemplateFile <PathToTemplateFile> -TemplateParameterFile <PathToParametersFile> -Verbose
    ```

5. Po dokončení upgradu zavedením (trvá mezi 15 – 45 minut), WAD by měl být shromažďování čítačů výkonu a jejich odeslání do tabulky v účtu úložiště deklarované v `WadCfg`.

## <a name="next-steps"></a>Další kroky
* Viz jímky čítače výkonu ve službě Application Insights přidáním Application Insights do vaší `WadCfg`. Pro čtení *jímky AI přidat do šablony Resource Manageru* kapitoly [analýza události a vizualizace s Application Insights ](service-fabric-diagnostics-event-analysis-appinsights.md) nakonfigurovat podřízený Application Insights.
* Shromažďujte další čítače výkonu pro váš cluster. V tématu [metriky výkonu](service-fabric-diagnostics-event-generation-perf.md) seznam čítačů, které mají shromažďovat.
* [Použití monitorování a Diagnostika pomocí šablony virtuálního počítače s Windows a Azure Resource Manager](../virtual-machines/windows/extensions-diagnostics-template.md) provést další změny vašeho `WadCfg`, včetně konfiguraci účtů úložiště pro odesílat data diagnostiky.