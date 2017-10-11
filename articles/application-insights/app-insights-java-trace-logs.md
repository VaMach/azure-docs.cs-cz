---
title: "Prozkoumejte Java protokolů trasování v Azure Application Insights | Microsoft Docs"
description: "Hledání Log4J nebo Logback trasování ve službě Application Insights"
services: application-insights
documentationcenter: java
author: CFreemanwa
manager: carmonm
ms.assetid: fc0a9e2f-3beb-4f47-a9fe-3f86cd29d97a
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 12/12/2016
ms.author: bwren
ms.openlocfilehash: 5baba3deaf58a1a24995c60381592a9c2ffefd81
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="explore-java-trace-logs-in-application-insights"></a>Prozkoumejte Java protokolů trasování ve službě Application Insights
Pokud používáte Logback nebo Log4J (verze 1.2 nebo v2.0) pro trasování, může mít vaše protokoly trasování automaticky odešlou do Application Insights, kde vám umožní zkoumat a hledat v nich.

## <a name="install-the-java-sdk"></a>Nainstalujte Java SDK

Nainstalujte [Application Insights SDK pro jazyk Java][java], pokud ještě neudělali.

(Pokud nechcete, aby pro sledování požadavků HTTP, můžete vynechat většinu konfiguračního souboru .xml, ale musí obsahovat alespoň `InstrumentationKey` elementu. Měli byste také zavolat `new TelemetryClient()` k chybě při inicializaci sady SDK.)


## <a name="add-logging-libraries-to-your-project"></a>Do projektu přidejte knihovny protokolování
*Zvolte vhodný způsob pro váš projekt.*

#### <a name="if-youre-using-maven"></a>Pokud používáte Maven...
Pokud je váš projekt již nastaven na sestavení s použitím nástroje Maven, slučte jednu z následujících fragmentů kódu do souboru pom.xml.

Pak obnovte závislosti projektu, k získání stažených binárních souborů.

*Logback*

```XML

    <dependencies>
       <dependency>
          <groupId>com.microsoft.azure</groupId>
          <artifactId>applicationinsights-logging-logback</artifactId>
          <version>[1.0,)</version>
       </dependency>
    </dependencies>
```

*Log4J v2.0*

```XML

    <dependencies>
       <dependency>
          <groupId>com.microsoft.azure</groupId>
          <artifactId>applicationinsights-logging-log4j2</artifactId>
          <version>[1.0,)</version>
       </dependency>
    </dependencies>
```

*Log4J v1.2*

```XML

    <dependencies>
       <dependency>
          <groupId>com.microsoft.azure</groupId>
          <artifactId>applicationinsights-logging-log4j1_2</artifactId>
          <version>[1.0,)</version>
       </dependency>
    </dependencies>
```

#### <a name="if-youre-using-gradle"></a>Pokud používáte Gradle...
Pokud váš projekt je již nastaven na Gradle použít pro sestavení, přidejte jeden z následujících řádky, které se `dependencies` v souboru build.gradle:

Pak obnovte závislosti projektu, k získání stažených binárních souborů.

**Logback**

```

    compile group: 'com.microsoft.azure', name: 'applicationinsights-logging-logback', version: '1.0.+'
```

**Log4J v2.0**

```
    compile group: 'com.microsoft.azure', name: 'applicationinsights-logging-log4j2', version: '1.0.+'
```

**Log4J v1.2**

```
    compile group: 'com.microsoft.azure', name: 'applicationinsights-logging-log4j1_2', version: '1.0.+'
```

#### <a name="otherwise-"></a>V opačném případě...
Stažení a extrakci odpovídající appender a pak přidejte příslušnou knihovnu do projektu:

| Protokoly | Ke stažení | Knihovna |
| --- | --- | --- |
| Logback |[SDK Logback appender](https://aka.ms/xt62a4) |applicationinsights. protokolování logback |
| Log4J v2.0 |[SDK Log4J v2 appender](https://aka.ms/qypznq) |applicationinsights. protokolování log4j2 |
| Log4j v1.2 |[SDK Log4J v1.2 appender](https://aka.ms/ky9cbo) |applicationinsights. protokolování log4j1_2 |

## <a name="add-the-appender-to-your-logging-framework"></a>Přidat appender do vašeho rozhraní protokolování
Chcete-li spustit načtení trasování, sloučení relevantní fragment kódu do konfiguračního souboru Log4J nebo Logback: 

*Logback*

```XML

    <appender name="aiAppender" 
      class="com.microsoft.applicationinsights.logback.ApplicationInsightsAppender">
    </appender>
    <root level="trace">
      <appender-ref ref="aiAppender" />
    </root>
```

*Log4J v2.0*

```XML

    <Configuration packages="com.microsoft.applicationinsights.Log4j">
      <Appenders>
        <ApplicationInsightsAppender name="aiAppender" />
      </Appenders>
      <Loggers>
        <Root level="trace">
          <AppenderRef ref="aiAppender"/>
        </Root>
      </Loggers>
    </Configuration>
```

*Log4J v1.2*

```XML

    <appender name="aiAppender" 
         class="com.microsoft.applicationinsights.log4j.v1_2.ApplicationInsightsAppender">
    </appender>
    <root>
      <priority value ="trace" />
      <appender-ref ref="aiAppender" />
    </root>
```

Appenders Application Insights může být odkaz žádné nakonfigurované protokolovacího nástroje a nemusí protokolovacího nástroje root (jak je znázorněno v ukázky kódu, výše).

## <a name="explore-your-traces-in-the-application-insights-portal"></a>Prozkoumat vaše trasování v portálu služby Application Insights
Nyní, když jste nakonfigurovali projekt k odeslání trasování do Application Insights, můžete zobrazit a vyhledávat tyto trasování v portálu služby Application Insights [vyhledávání] [ diagnostic] okno.

![Otevřete vyhledávání v portálu služby Application Insights](./media/app-insights-java-trace-logs/10-diagnostics.png)

## <a name="next-steps"></a>Další kroky
[Diagnostické vyhledávání][diagnostic]

<!--Link references-->

[diagnostic]: app-insights-diagnostic-search.md
[java]: app-insights-java-get-started.md


