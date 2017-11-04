---
title: "Oddělení telemetrie z vývoj, testování a verzí v Azure Application Insights | Microsoft Docs"
description: "Přímé telemetrie na jiné prostředky pro vývoj, testování a provozním razítka."
services: application-insights
documentationcenter: 
author: mrbullwinkle
manager: carmonm
ms.assetid: 578e30f0-31ed-4f39-baa8-01b4c2f310c9
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 05/15/2017
ms.author: mbullwin
ms.openlocfilehash: 8d95958bce0597bfb16ef1c6b99b72ce9134e66f
ms.sourcegitcommit: e462e5cca2424ce36423f9eff3a0cf250ac146ad
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/01/2017
---
# <a name="separating-telemetry-from-development-test-and-production"></a>Oddělení telemetrie z vývoj, testování a provozním

Při vývoji příští verze webové aplikace, nechcete kombinovat [Application Insights](app-insights-overview.md) telemetrie z nové verze a již vydaná verze. Pokud chcete předejít nejasnostem, odešlete telemetrii z různých vývoj fázích jednotlivé prostředky Application Insights, s klíči samostatné instrumentace (ikeys). Aby bylo snazší změnit klíč instrumentace, protože verze přesouvá z jedné fáze do jiného, může být užitečné k nastavení ikey v kódu místo v konfiguračním souboru. 

(Pokud je váš systém cloudové služby Azure je [jinou metodu nastavení samostatné ikeys](app-insights-cloudservices.md).)

## <a name="about-resources-and-instrumentation-keys"></a>O prostředcích a klíčů instrumentace

Když jste nastavili Application Insights monitorování pro webovou aplikaci, můžete vytvořit Application Insights *prostředků* v Microsoft Azure. Tento prostředek otevřít na portálu Azure, aby bylo možné zobrazit a analyzovat telemetrii získanou z vašich aplikací. Prostředek je identifikována *klíč instrumentace* (ikey). Když nainstalujete balíček Application Insights pro sledování aplikace, musíte ho nakonfigurovat s klíč instrumentace, tak, aby věděl, že může kam má posílat telemetrii.

Obvykle je rozhodnete použít samostatné prostředky nebo jeden sdílený prostředek v různých scénářích:

* Jiné, nezávislé aplikace – použít samostatné prostředků a ikey pro každou aplikaci.
* Více součástí nebo role jeden obchodní aplikace – použijte [jeden sdílený prostředek](app-insights-monitor-multi-role-apps.md) pro všechny součásti aplikace. Telemetrická data lze filtrovat a segmentované cloud_RoleName vlastností.
* Vývoj, testování a verze – použijte samostatné prostředků a ikey pro verze systému, v 'razítka, nebo fáze produkce.
* A | Testování B – použít jeden zdroj. Vytvořte TelemetryInitializer k přidání vlastnosti do telemetrii, kterou identifikuje varianty.


## <a name="dynamic-ikey"></a>Klíč dynamické instrumentace

Aby bylo snazší ke změnám ikey podle kód přesune mezi fáze produkce, nastavte v kódu místo v konfiguračním souboru.

V metodě inicializace, jako jsou například souboru global.aspx.cs v službě ASP.NET nastavte:

*C#*

    protected void Application_Start()
    {
      Microsoft.ApplicationInsights.Extensibility.
        TelemetryConfiguration.Active.InstrumentationKey = 
          // - for example -
          WebConfigurationManager.AppSettings["ikey"];
      ...

V tomto příkladu ikeys pro různé prostředky jsou umístěny v různých verzích soubor webové konfigurace. Vzájemná záměna konfigurační soubor webu – což lze provést v rámci skriptu verze - bude Prohodit cílový prostředek.

### <a name="web-pages"></a>Webové stránky
IKey se také používá ve webových stránkách vaší aplikace, v [skript, který jste získali z okna rychlý start](app-insights-javascript.md). Místo kódování je oznámena do skriptu, vygenerujte ho z stavu serveru. Například v aplikaci ASP.NET:

*JavaScript ve Razor*

    <script type="text/javascript">
    // Standard Application Insights web page script:
    var appInsights = window.appInsights || function(config){ ...
    // Modify this part:
    }({instrumentationKey:  
      // Generate from server property:
      "@Microsoft.ApplicationInsights.Extensibility.
         TelemetryConfiguration.Active.InstrumentationKey"
    }) // ...


## <a name="create-additional-application-insights-resources"></a>Vytvořit další prostředky Application Insights
K oddělení telemetrie pro součásti jiné aplikace, nebo pro jiný razítka (dev/testovací/produkční) stejné komponenty, pak budete muset vytvořit nový prostředek Application Insights.

V [portal.azure.com](https://portal.azure.com), přidejte prostředek Application Insights:

![Klikněte na tlačítko Nový, Application Insights](./media/app-insights-separate-resources/01-new.png)

* **Typ aplikace** ovlivňuje, co se zobrazí v okně Přehled a vlastnosti, které jsou k dispozici v [metriky explorer](app-insights-metrics-explorer.md). Pokud nevidíte vašeho typu aplikace, vyberte jeden z typů webových pro webové stránky.
* **Skupina prostředků** je užitečný pro správu vlastnosti, například [řízení přístupu](app-insights-resources-roles-access-control.md). Pro vývoj, testování a provozním můžete použít samostatné skupiny prostředků.
* **Předplatné** je váš účet platebních v Azure.
* **Umístění** je, kde společnost Microsoft uchovávat data. Aktuálně jej nelze změnit. 
* **Přidat na řídicí panel** vloží dlaždici rychlý přístup pro prostředek na Azure domovské stránky. 

Vytvoření prostředku trvá několik sekund. Pokud se provádí, zobrazí se výstraha.

(Můžete napsat [skript prostředí PowerShell](app-insights-powershell-script-create-resource.md) vytvoření prostředku automaticky.)

### <a name="getting-the-instrumentation-key"></a>Získávání klíč instrumentace
Klíč instrumentace identifikuje prostředek, který jste vytvořili. 

![Klikněte na tlačítko Essentials, klikněte na klíč instrumentace, CTRL + C](./media/app-insights-separate-resources/02-props.png)

Je nutné klíčů instrumentace všech prostředků, do které bude vaše aplikace posílat data.

## <a name="filter-on-build-number"></a>Filtrovat podle čísla sestavení
Při publikování nové verze aplikace, budete chtít mít možnost oddělit telemetrii z různých sestavení.

Verze aplikace vlastnost lze nastavit tak, aby můžete filtrovat [vyhledávání](app-insights-diagnostic-search.md) a [metriky explorer](app-insights-metrics-explorer.md) výsledky.

![Filtrování u vlastnosti](./media/app-insights-separate-resources/050-filter.png)

Existuje několik různých metod nastavení vlastnosti verze aplikace.

* Nastavte přímo:

    `telemetryClient.Context.Component.Version = typeof(MyProject.MyClass).Assembly.GetName().Version;`
* Zabalení daného řádku v [telemetrie inicializátoru](app-insights-api-custom-events-metrics.md#defaults) zajistit, že jsou všechny instance TelemetryClient konzistentně nastavené.
* [ASP.NET] Nastavit verzi `BuildInfo.config`. Modul web vyzvedne, až bude verze z uzlu BuildLabel. Zahrnout tento soubor ve vašem projektu a nezapomeňte nastavit vlastnost kopírování vždy v Průzkumníku řešení.

    ```XML

    <?xml version="1.0" encoding="utf-8"?>
    <DeploymentEvent xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns="http://schemas.microsoft.com/VisualStudio/DeploymentEvent/2013/06">
      <ProjectName>AppVersionExpt</ProjectName>
      <Build type="MSBuild">
        <MSBuild>
          <BuildLabel kind="label">1.0.0.2</BuildLabel>
        </MSBuild>
      </Build>
    </DeploymentEvent>

    ```
* [ASP.NET] Automaticky generovat BuildInfo.config v nástroji MSBuild. K tomuto účelu přidat pár řádků do vaší `.csproj` souboru:

    ```XML

    <PropertyGroup>
      <GenerateBuildInfoConfigFile>true</GenerateBuildInfoConfigFile>    <IncludeServerNameInBuildInfo>true</IncludeServerNameInBuildInfo>
    </PropertyGroup>
    ```

    Tím se vygeneruje soubor s názvem *Názevvašehoprojektu*. BuildInfo.config. Proces publikování se přejmenuje na BuildInfo.config.

    Popisek sestavení obsahuje zástupný znak (AutoGen_...), když vytvoříte pomocí sady Visual Studio. Ale když vytvořené pomocí nástroje MSBuild, je naplněn číslo správnou verzi.

    Chcete-li povolit MSBuild ke generování čísel verzí, nastavte verze jako `1.0.*` v AssemblyReference.cs

## <a name="version-and-release-tracking"></a>Sledování verzí a vydání
Pokud chcete sledovat verzi aplikace, ujistěte se, že proces Microsoft Build Engine vygeneroval soubor `buildinfo.config`. Do souboru .csproj přidejte:  

```XML

    <PropertyGroup>
      <GenerateBuildInfoConfigFile>true</GenerateBuildInfoConfigFile>    <IncludeServerNameInBuildInfo>true</IncludeServerNameInBuildInfo>
    </PropertyGroup>
```

Pokud obsahuje informace o sestavení, webový modul Application Insights automaticky přidá položku **Verze aplikace** jako vlastnost pro každý předmět telemetrie. Díky tomu můžete při provádění [diagnostických hledání](app-insights-diagnostic-search.md) nebo při [zkoumání metrik](app-insights-metrics-explorer.md) filtrovat podle verze.

Všimněte si však, že číslo verze sestavení je generováno pouze pomocí procesu Microsoft Build Engine, ne sestavením vývojáře v sadě Visual Studio.

### <a name="release-annotations"></a>Poznámky k verzi
Pokud používáte Visual Studio Team Services, můžete [získat značku poznámek](app-insights-annotations.md), kterou můžete přidat do svých grafů pokaždé, když vydáte novou verzi. Následující obrázek ukazuje, jak se tato značka zobrazuje.

![Snímek obrazovky grafu s ukázkovou poznámkou k verzi](./media/app-insights-asp-net/release-annotation.png)
## <a name="next-steps"></a>Další kroky

* [Sdílené prostředky pro víc rolí](app-insights-monitor-multi-role-apps.md)
* [Vytvoření inicializátoru Telemetrie k rozlišení A | Variant B](app-insights-api-filtering-sampling.md#add-properties)
