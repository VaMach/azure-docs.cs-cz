---
title: "Filtrovat telemetrie Azure Application Insights ve webové aplikace Java | Microsoft Docs"
description: "Omezit přenos telemetrie filtrováním událostí, které nepotřebujete k monitorování."
services: application-insights
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 11/23/2016
ms.author: bwren
ms.openlocfilehash: 5f6d6d4ad590b85810c42e9f9520850024c5446a
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="filter-telemetry-in-your-java-web-app"></a>Filtr telemetrie webové aplikace Java

Filtry poskytnout způsob, jak vybrat telemetrii, vaše [webovou aplikaci Java odešle do služby Application Insights](app-insights-java-get-started.md). Existují některé filtry se na pole, které můžete použít, a můžete je zapsat také vlastní filtry.

Filtry se na pole zahrnují:

* Úroveň závažnosti trasování
* Konkrétní adresy URL, klíčová slova nebo kódy odpovědí
* Rychlou odezvu – to znamená, na které aplikace odpověděl rychle požadavky
* Názvy konkrétním událostí

> [!NOTE]
> Filtry zkreslit metrik vaší aplikace. Například můžete rozhodnout, že chcete-li diagnostikovat pomalé odezvy, můžete nastavit filtr vyřadí krátké doby odezvy. Ale je potřeba si uvědomit, že průměrná odezvy hlášené Application Insights bude nižší než skutečná rychlost a počet požadavků bude menší než skutečná počet.
> Pokud se jedná o problém, použijte [vzorkování](app-insights-sampling.md) místo.

## <a name="setting-filters"></a>Nastavení filtrů

Přidejte soubor ApplicationInsights.xml, `TelemetryProcessors` části jako tento ukázkový:


```XML

    <ApplicationInsights>
      <TelemetryProcessors>

        <BuiltInProcessors>
           <Processor type="TraceTelemetryFilter">
                  <Add name="FromSeverityLevel" value="ERROR"/>
           </Processor>

           <Processor type="RequestTelemetryFilter">
                  <Add name="MinimumDurationInMS" value="100"/>
                  <Add name="NotNeededResponseCodes" value="200-400"/>
           </Processor>

           <Processor type="PageViewTelemetryFilter">
                  <Add name="DurationThresholdInMS" value="100"/>
                  <Add name="NotNeededNames" value="home,index"/>
                  <Add name="NotNeededUrls" value=".jpg,.css"/>
           </Processor>

           <Processor type="TelemetryEventFilter">
                  <!-- Names of events we don't want to see -->
                  <Add name="NotNeededNames" value="Start,Stop,Pause"/>
           </Processor>

           <!-- Exclude telemetry from availability tests and bots -->
           <Processor type="SyntheticSourceFilter">
                <!-- Optional: specify which synthetic sources,
                     comma-separated
                     - default is all synthetics -->
                <Add name="NotNeededSources" value="Application Insights Availability Monitoring,BingPreview"
           </Processor>

        </BuiltInProcessors>

        <CustomProcessors>
          <Processor type="com.fabrikam.MyFilter">
            <Add name="Successful" value="false"/>
          </Processor>
        </CustomProcessors>

      </TelemetryProcessors>
    </ApplicationInsights>

```




[Zkontrolujte úplnou sadu předdefinovaných procesorů](https://github.com/Microsoft/ApplicationInsights-Java/tree/master/core/src/main/java/com/microsoft/applicationinsights/internal/processor).

## <a name="built-in-filters"></a>Integrované filtry

### <a name="metric-telemetry-filter"></a>Metriky Telemetrie filtru

```XML

           <Processor type="MetricTelemetryFilter">
                  <Add name="NotNeeded" value="metric1,metric2"/>
           </Processor>
```

* `NotNeeded`-Čárkami oddělený seznam názvů vlastní metriky.


### <a name="page-view-telemetry-filter"></a>Filtr Telemetrická zobrazení stránky

```XML

           <Processor type="PageViewTelemetryFilter">
                  <Add name="DurationThresholdInMS" value="500"/>
                  <Add name="NotNeededNames" value="page1,page2"/>
                  <Add name="NotNeededUrls" value="url1,url2"/>
           </Processor>
```

* `DurationThresholdInMS`-Doba trvání odkazuje na čas potřebný k načtení stránky. Pokud je toto nastaveno, nejsou hlášeny stránky, které načtené rychleji, než tuto chvíli.
* `NotNeededNames`-Čárkami oddělený seznam názvů stránky.
* `NotNeededUrls`-Fragmenty čárkami oddělený seznam adresy URL. Například `"home"` odfiltruje všechny stránky, které mají "Domů" v adrese URL.


### <a name="request-telemetry-filter"></a>Filtr Telemetrie požadavku


```XML

           <Processor type="RequestTelemetryFilter">
                  <Add name="MinimumDurationInMS" value="500"/>
                  <Add name="NotNeededResponseCodes" value="page1,page2"/>
                  <Add name="NotNeededUrls" value="url1,url2"/>
           </Processor>
```



### <a name="synthetic-source-filter"></a>Syntetické zdrojový filtr

Odfiltruje všechny telemetrická data, která mají hodnoty ve vlastnosti SyntheticSource. Mezi ně patří požadavky od robotů, pavouci a testy dostupnosti.

Filtrovat telemetrická data pro všechny syntetické požadavky:


```XML

           <Processor type="SyntheticSourceFilter" />
```

Filtrovat telemetrie pro konkrétní syntetické zdroje:


```XML

           <Processor type="SyntheticSourceFilter" >
                  <Add name="NotNeeded" value="source1,source2"/>
           </Processor>
```

* `NotNeeded`-Čárkami oddělený seznam názvů syntetické zdroje.

### <a name="telemetry-event-filter"></a>Filtr událostí telemetrie

Filtry vlastních událostí (přihlášení pomocí [TrackEvent()](app-insights-api-custom-events-metrics.md#trackevent)).


```XML

           <Processor type="TelemetryEventFilter" >
                  <Add name="NotNeededNames" value="event1, event2"/>
           </Processor>
```


* `NotNeededNames`-Čárkami oddělený seznam názvů událostí.


### <a name="trace-telemetry-filter"></a>Filtr Telemetrie trasování

Filtry protokolu trasování (přihlášení pomocí [TrackTrace()](app-insights-api-custom-events-metrics.md#tracktrace) nebo [protokolování framework kolekce](app-insights-java-trace-logs.md)).

```XML

           <Processor type="TraceTelemetryFilter">
                  <Add name="FromSeverityLevel" value="ERROR"/>
           </Processor>
```

* `FromSeverityLevel`Platné hodnoty jsou:
 *  VYPNOUT - vyfiltrovat všech trasování
 *  TRASOVÁNÍ – žádné filtrování. hodnotu úroveň trasování
 *  Informace o - filtru na úroveň trasování
 *  VAROVÁNÍ - trasování a informace o filtru
 *  Chyba: filtru se varování, informace trasování
 *  KRITICKÝ - filtru se jenom důležité


## <a name="custom-filters"></a>Vlastní filtry

### <a name="1-code-your-filter"></a>1. Kód filtru

V kódu, vytvořte třídu, která implementuje `TelemetryProcessor`:

```Java

    package com.fabrikam.MyFilter;
    import com.microsoft.applicationinsights.extensibility.TelemetryProcessor;
    import com.microsoft.applicationinsights.telemetry.Telemetry;

    public class SuccessFilter implements TelemetryProcessor {

       /* Any parameters that are required to support the filter.*/
       private final String successful;

       /* Initializers for the parameters, named "setParameterName" */
       public void setNotNeeded(String successful)
       {
          this.successful = successful;
       }

       /* This method is called for each item of telemetry to be sent.
          Return false to discard it.
          Return true to allow other processors to inspect it. */
       @Override
       public boolean process(Telemetry telemetry) {
        if (telemetry == null) { return true; }
        if (telemetry instanceof RequestTelemetry)
        {
            RequestTelemetry requestTelemetry = (RequestTelemetry)telemetry;
            return request.getSuccess() == successful;
        }
        return true;
       }
    }

```


### <a name="2-invoke-your-filter-in-the-configuration-file"></a>2. Vyvolání filtru v konfiguračním souboru

V ApplicationInsights.xml:

```XML


    <ApplicationInsights>
      <TelemetryProcessors>
        <CustomProcessors>
          <Processor type="com.fabrikam.SuccessFilter">
            <Add name="Successful" value="false"/>
          </Processor>
        </CustomProcessors>
      </TelemetryProcessors>
    </ApplicationInsights>

```

## <a name="troubleshooting"></a>Řešení potíží

*Moje filtru není funkční.*

* Zkontrolujte, zda jste zadali platný parametr hodnoty. Například doby trvání musí být celá čísla. Neplatné hodnoty způsobí, že filtr budou ignorovány. Pokud vaše vlastní filtr vyvolá výjimku z konstruktoru nebo metoda set, se budou ignorovat.

## <a name="next-steps"></a>Další kroky

* [Vzorkování](app-insights-sampling.md) – zvažte vzorkování jako alternativu, která není zkreslit vaše metriky.
