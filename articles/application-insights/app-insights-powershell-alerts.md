---
title: "Pomocí prostředí Powershell nastavit výstrahy ve službě Application Insights | Microsoft Docs"
description: "Automatizovat konfiguraci Application Insights získat e-mailů o metriky změny."
services: application-insights
documentationcenter: 
author: mrbullwinkle
manager: carmonm
ms.assetid: 05d6a9e0-77a2-4a35-9052-a7768d23a196
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 10/31/2016
ms.author: mbullwin
ms.openlocfilehash: b90a540afd1c2815db8f5a99ee210ce21ea4d874
ms.sourcegitcommit: e462e5cca2424ce36423f9eff3a0cf250ac146ad
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/01/2017
---
# <a name="use-powershell-to-set-alerts-in-application-insights"></a>Použití prostředí PowerShell k nastavení výstrahy v nástroji Application Insights
Můžete automatizovat konfiguraci [výstrahy](app-insights-alerts.md) v [Application Insights](app-insights-overview.md).

Kromě toho můžete [nastavit webhooky k automatizaci vaše odpověď na výstrahu](../monitoring-and-diagnostics/insights-webhooks-alerts.md).

> [!NOTE]
> Pokud chcete vytvořit prostředky a výstrahy ve stejnou dobu, zvažte [pomocí šablony Azure Resource Manager](app-insights-powershell.md).
>
>

## <a name="one-time-setup"></a>Jednorázové instalace
Pokud jste nepoužili prostředí PowerShell s předplatným Azure před:

Instalace modulu Azure Powershell na počítači, kde chcete spustit skripty.

* Nainstalujte [instalačního programu webové platformy (verze 5 nebo novější)](http://www.microsoft.com/web/downloads/platform.aspx).
* Jeho použití k instalaci aplikace Microsoft Azure Powershell

## <a name="connect-to-azure"></a>Připojení k Azure
Otevřete prostředí Azure PowerShell a [připojení k vašemu předplatnému](/powershell/azure/overview):

```PowerShell

    Add-AzureAccount
```


## <a name="get-alerts"></a>Získání výstrahy
    Get-AzureAlertRmRule -ResourceGroup "Fabrikam" [-Name "My rule"] [-DetailedOutput]

## <a name="add-alert"></a>Přidání oznámení
    Add-AlertRule  -Name "{ALERT NAME}" -Description "{TEXT}" `
     -ResourceGroup "{GROUP NAME}" `
     -ResourceId "/subscriptions/{SUBSCRIPTION ID}/resourcegroups/{GROUP NAME}/providers/microsoft.insights/components/{APP RESOURCE NAME}" `
     -MetricName "{METRIC NAME}" `
     -Operator GreaterThan  `
     -Threshold {NUMBER}   `
     -WindowSize {HH:MM:SS}  `
     [-SendEmailToServiceOwners] `
     [-CustomEmails "EMAIL1@X.COM","EMAIL2@Y.COM" ] `
     -Location "East US" // must be East US at present
     -RuleType Metric



## <a name="example-1"></a>Příklad 1
Poslat mi e-mail, pokud odpověď serveru na požadavky HTTP, průměr 5 minut, je nižší než 1 sekunda. Moje prostředek Application Insights se nazývá IceCreamWebApp a je ve skupině prostředků společnosti Fabrikam. Jsem vlastník předplatného Azure.

Identifikátor GUID je ID předplatného (ne klíč instrumentace aplikace).

    Add-AlertRule -Name "slow responses" `
     -Description "email me if the server responds slowly" `
     -ResourceGroup "Fabrikam" `
     -ResourceId "/subscriptions/00000000-0000-0000-0000-000000000000/resourcegroups/Fabrikam/providers/microsoft.insights/components/IceCreamWebApp" `
     -MetricName "request.duration" `
     -Operator GreaterThan `
     -Threshold 1 `
     -WindowSize 00:05:00 `
     -SendEmailToServiceOwners `
     -Location "East US" -RuleType Metric

## <a name="example-2"></a>Příklad 2
Je nutné aplikaci, ve které používám [TrackMetric()](app-insights-api-custom-events-metrics.md#trackmetric) nahlásit metriky s názvem "salesPerHour." Odesílání že e-mailu na kolegové Pokud "salesPerHour" klesne pod 100, průměr po dobu 24 hodin.

    Add-AlertRule -Name "poor sales" `
     -Description "slow sales alert" `
     -ResourceGroup "Fabrikam" `
     -ResourceId "/subscriptions/00000000-0000-0000-0000-000000000000/resourcegroups/Fabrikam/providers/microsoft.insights/components/IceCreamWebApp" `
     -MetricName "salesPerHour" `
     -Operator LessThan `
     -Threshold 100 `
     -WindowSize 24:00:00 `
     -CustomEmails "satish@fabrikam.com","lei@fabrikam.com" `
     -Location "East US" -RuleType Metric

Stejného pravidla lze použít pro metriku hlášené pomocí [měření parametr](app-insights-api-custom-events-metrics.md#properties) jiné sledování volání například TrackEvent nebo trackPageView.

## <a name="metric-names"></a>Metriky názvy
| Název metriky | Název obrazovky | Popis |
| --- | --- | --- |
| `basicExceptionBrowser.count` |Výjimky prohlížečů |Počet nezachycená výjimky vydané v prohlížeči. |
| `basicExceptionServer.count` |Výjimky serveru |Počet neošetřené výjimky vyvolané aplikace |
| `clientPerformance.clientProcess.value` |Doba zpracování klienta |Doba mezi přijetí poslední bajt dokumentu, dokud nebude modelu DOM vložen. Asynchronních připojení může být stále aktivní. |
| `clientPerformance.networkConnection.value` |Doba připojení sítě zatížení stránky |Čas, kdy prohlížeč potřebné k připojení k síti. Může být 0, pokud do mezipaměti. |
| `clientPerformance.receiveRequest.value` |Přijetí doba odezvy |Doba mezi prohlížeče odesílání požadavku na spuštění na obdrží odpověď. |
| `clientPerformance.sendRequest.value` |Odeslat doba požadavku |Doba, za kterou prohlížeč k odeslání požadavku. |
| `clientPerformance.total.value` |Čas načítání stránky prohlížeče |Čas od požadavku uživatele, dokud se načtou DOM, předlohy se styly, skripty a obrázků. |
| `performanceCounter.available_bytes.value` |Dostupná paměť |Fyzické paměti k dispozici pro zpracování nebo pro použití systémem. |
| `performanceCounter.io_data_bytes_per_sec.value` |Rychlost zpracování vstupně-výstupní operace |Celkový počet bajtů za sekundu číst a zapisovat do souborů, sítě a zařízení. |
| `performanceCounter.number_of_exceps_thrown_per_sec.value` |míra výjimky |Výjimek vyvolaných za sekundu. |
| `performanceCounter.percentage_processor_time.value` |Proces procesoru |Procentuální hodnotu uplynulého času, podprocesy procesu používá procesor pro spouštění instrukcí pro proces aplikace. |
| `performanceCounter.percentage_processor_total.value` |Čas procesoru |Procento času, kterou procesor stráví jiných než nečinných vláken. |
| `performanceCounter.process_private_bytes.value` |Proces nesdílených bajtů |Paměť výhradně přiřazená procesy v monitorované aplikaci. |
| `performanceCounter.request_execution_time.value` |Čas provádění požadavku ASP.NET |Čas provedení posledního požadavku. |
| `performanceCounter.requests_in_application_queue.value` |ASP.NET požadavků ve frontě provádění |Délka frontě požadavků aplikace. |
| `performanceCounter.requests_per_sec.value` |Rychlost požadavků ASP.NET |Počet všech požadavků na pro aplikaci za sekundu z technologie ASP.NET. |
| `remoteDependencyFailed.durationMetric.count` |Selhání závislostí |Počet selhání volání od serverové aplikace na externí zdroje. |
| `request.duration` |Doba odezvy serveru |Doba mezi přijetí požadavku HTTP a dokončením odeslání odpovědi. |
| `request.rate` |Rychlost požadavků |Počet všech požadavků na pro aplikaci za sekundu. |
| `requestFailed.count` |Neúspěšné požadavky |Počet HTTP požadavků, které kód odpovědi > = 400 |
| `view.count` |Zobrazení stránky |Počet žádostí uživatele klienta pro webovou stránku. Syntetické provoz odfiltrována. |
| {váš vlastní název metriky} |{Název metriky} |Metriky hodnota hlášené [TrackMetric](app-insights-api-custom-events-metrics.md#trackmetric) nebo [parametr měření volání sledování](app-insights-api-custom-events-metrics.md#properties). |

Metriky se odesílají moduly různých telemetrie:

| Metriky skupiny | Kolekce modulu |
| --- | --- |
| basicExceptionBrowser,<br/>clientPerformance,<br/>zobrazit |[JavaScript prohlížeče](app-insights-javascript.md) |
| performanceCounter |[Výkon](app-insights-configuration-with-applicationinsights-config.md) |
| remoteDependencyFailed |[Závislost](app-insights-configuration-with-applicationinsights-config.md) |
| žádost,<br/>requestFailed |[Požadavek na serveru](app-insights-configuration-with-applicationinsights-config.md) |

## <a name="webhooks"></a>Webhooky
Můžete [automatizovat vaše odpověď na výstrahu](../monitoring-and-diagnostics/insights-webhooks-alerts.md). Azure bude volat webovou adresu podle vaší volby, když je vydána výstraha.

## <a name="see-also"></a>Viz také
* [Skript, který nakonfiguruje Application Insights](app-insights-powershell-script-create-resource.md)
* [Vytvoření služby Application Insights a web test prostředky ze šablon](app-insights-powershell.md)
* [Automatizovat spojovacích Microsoft Azure Diagnostics Application insights](app-insights-powershell-azure-diagnostics.md)
* [Automatizovat vaše odpověď na výstrahu](../monitoring-and-diagnostics/insights-webhooks-alerts.md)
