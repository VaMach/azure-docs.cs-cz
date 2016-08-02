<properties
    pageTitle="Analýzy webové aplikace Java pomocí nástroje Application Insights | Microsoft Azure"
    description="Sledování výkonu a využití vašeho webu Java pomocí Application Insights. "
    services="application-insights"
    documentationCenter="java"
    authors="alancameronwills"
    manager="douge"/>

<tags
    ms.service="application-insights"
    ms.workload="tbd"
    ms.tgt_pltfrm="ibiza"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="05/12/2016"
    ms.author="awills"/>

# Začínáme s Application Insights ve webovém projektu Java

*Application Insights je ve verzi Preview.*

[AZURE.INCLUDE [app-insights-selector-get-started](../../includes/app-insights-selector-get-started.md)]

[Application Insights](https://azure.microsoft.com/services/application-insights/) představují rozšiřitelnou analytickou službu, která vám pomůže pochopit výkon a využití vaší živé aplikace. Použijte ji ke [zjištění a diagnostice problémů s výkonem a výjimkami](app-insights-detect-triage-diagnose.md) a [zápisu kódu][rozhraní api] ke sledování, co uživatelé dělají s vaší aplikací.

![ukázková data](./media/app-insights-java-get-started/5-results.png)

Application Insights podporuje Java aplikace spuštěné v systému Linux, Unix nebo Windows.

Budete potřebovat:

* Oracle JRE 1.6 nebo novější nebo Zulu JRE 1.6 nebo novější
* Předplatné [Microsoft Azure](https://azure.microsoft.com/). (Můžete začít s [bezplatnou zkušební verzí](https://azure.microsoft.com/pricing/free-trial/).)

*Pokud máte webovou aplikaci, která je již živá, můžete sledovat alternativní postup pro [přidání sady SDK v době běhu na webovém serveru](app-insights-java-live.md). Tento alternativa zabraňuje opětovnému sestavení kódu, ale neobdržíte možnost zápisu kódu pro sledování činnosti uživatelů.*


## 1. Získejte klíč instrumentace Application Insights

1. Přihlaste se do [portálu Microsoft Azure](https://portal.azure.com).
2. Vytvořte nový prostředek Application Insights.

    ![Klikněte na + a vyberte Application Insights](./media/app-insights-java-get-started/01-create.png)
3. Nastavte typ aplikace na webovou aplikaci Java.

    ![Zadejte název, vyberte webovou aplikaci Java a klikněte na možnost Vytvořit](./media/app-insights-java-get-started/02-create.png)
4. Najděte klíč instrumentace nového prostředku. Ten budete muset krátce vložit do projektu kódu.

    ![V přehledu nového prostředku klikněte na tlačítko Vlastnosti a zkopírujte klíč instrumentace](./media/app-insights-java-get-started/03-key.png)

## 2. Do projektu přidejte Application Insights SDK pro jazyk Java

*Zvolte vhodný způsob pro váš projekt.*

#### Pokud používáte Eclipse k vytvoření Maven nebo dynamického webového projektu...

Použijte [Application Insights SDK pro modul Java plug-in][eclipse].

#### Pokud používáte Maven...

Pokud je váš projekt již nastaven na sestavení s použitím nástroje Maven, slučte následující kód do souboru pom.xml.

Pak obnovte závislosti projektu k získání stažených binárních souborů.

    <repositories>
       <repository>
          <id>central</id>
          <name>Central</name>
          <url>http://repo1.maven.org/maven2</url>
       </repository>
    </repositories>

    <dependencies>
      <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>applicationinsights-web</artifactId>
        <!-- or applicationinsights-core for bare API -->
        <version>[1.0,)</version>
      </dependency>
    </dependencies>


* *Chyby ověření sestavení nebo kontrolního součtu?* Zkuste použít konkrétní verzi, například: `<version>1.0.n</version>`. Nejnovější verzi naleznete v [poznámkách k verzi sady SDK](https://github.com/Microsoft/ApplicationInsights-Java#release-notes) nebo v našich [artefaktech Maven](http://search.maven.org/#search%7Cga%7C1%7Capplicationinsights).
* *Je nutné aktualizovat na novou sadu SDK?* Obnovte závislosti svého projektu.

#### Pokud používáte Gradle...

Pokud je váš projekt již nastaven na sestavení s použitím nástroje Gradle, slučte následující kód do souboru build.gradle.

Pak obnovte závislosti projektu k získání stažených binárních souborů.

    repositories {
      mavenCentral()
    }

    dependencies {
      compile group: 'com.microsoft.azure', name: 'applicationinsights-web', version: '1.+'
      // or applicationinsights-core for bare API
    }

* *Chyby ověření sestavení nebo kontrolního součtu? Zkuste použít konkrétní verzi, například: * `version:'1.0.n'`. *Nejnovější verzi naleznete v [poznámkách k verzi sady SDK](https://github.com/Microsoft/ApplicationInsights-Java#release-notes).*
* *Postup aktualizace na novou sadu SDK*
 * Obnovte závislosti svého projektu.

#### V opačném případě...

Ručně přidejte sadu SDK:

1. Stáhněte si [Application Insights SDK pro jazyk Java](https://azuredownloads.blob.core.windows.net/applicationinsights/sdk.html).
2. Rozbalte binární soubory ze souboru zip a přidejte je do projektu.

### Otázky...

* *Co je vztah mezi komponenty `-core` a `-web` v souboru zip?*

 * `applicationinsights-core` poskytuje úplné informace o API. Tuto funkci budete vždy potřebovat.
 * `applicationinsights-web` poskytuje metriky, které sledují počty žádostí HTTP a časy odezvy. Ty můžete vynechat, pokud nechcete automaticky shromažďovat tuto telemetrii. Pokud například chcete napsat vlastní.

* *Chcete-li aktualizovat sadu SDK, když publikujeme změny*
 * Stáhněte si poslední [Application Insights SDK pro jazyk Java](https://azuredownloads.blob.core.windows.net/applicationinsights/sdk.zip) a nahraďte staré.
 * Změny jsou popsány v [poznámkách k verzi sady SDK](https://github.com/Microsoft/ApplicationInsights-Java#release-notes).



## 3. Vytvořte soubor Application Insights .xml

Přidejte soubor ApplicationInsights.xml do složky zdrojů v projektu nebo zajistěte, aby byl přidán do cesty nasazení tříd projektu. Zkopírujte do něj následující kód XML.

Nahraďte klíč instrumentace, který jste dostali z portálu Azure.

    <?xml version="1.0" encoding="utf-8"?>
    <ApplicationInsights xmlns="http://schemas.microsoft.com/ApplicationInsights/2013/Settings" schemaVersion="2014-05-30">


      <!-- The key from the portal: -->

      <InstrumentationKey>** Your instrumentation key **</InstrumentationKey>


      <!-- HTTP request component (not required for bare API) -->

      <TelemetryModules>
        <Add type="com.microsoft.applicationinsights.web.extensibility.modules.WebRequestTrackingTelemetryModule"/>
        <Add type="com.microsoft.applicationinsights.web.extensibility.modules.WebSessionTrackingTelemetryModule"/>
        <Add type="com.microsoft.applicationinsights.web.extensibility.modules.WebUserTrackingTelemetryModule"/>
      </TelemetryModules>

      <!-- Events correlation (not required for bare API) -->
      <!-- These initializers add context data to each event -->

      <TelemetryInitializers>
        <Add   type="com.microsoft.applicationinsights.web.extensibility.initializers.WebOperationIdTelemetryInitializer"/>
        <Add type="com.microsoft.applicationinsights.web.extensibility.initializers.WebOperationNameTelemetryInitializer"/>
        <Add type="com.microsoft.applicationinsights.web.extensibility.initializers.WebSessionTelemetryInitializer"/>
        <Add type="com.microsoft.applicationinsights.web.extensibility.initializers.WebUserTelemetryInitializer"/>
        <Add type="com.microsoft.applicationinsights.web.extensibility.initializers.WebUserAgentTelemetryInitializer"/>

      </TelemetryInitializers>
    </ApplicationInsights>


* Klíč instrumentace se zasílá společně s každou položkou telemetrie a říká službě Application Insights, aby ho zobrazila v prostředku.
* Požadavek komponenty HTTP je volitelný. Automaticky odesílá telemetrii týkající se žádostí a časů odezvy na portál.
* Korelace událostí je doplněk komponenty požadavku HTTP. Přiřadí identifikátor každé žádosti přijaté serverem a přidá ji jako vlastnost každé položce telemetrie jako vlastnost 'Operation.Id'. Umožňuje korelovat telemetrii související s každou žádostí nastavením filtru v diagnostice [diagnostických vyhledávání][].

## 4. Přidat filtr HTTP

Poslední krok konfigurace umožňuje komponentě požadavku HTTP zaprotokolovat každý webový požadavek. (Není požadováno, pokud chcete úplné rozhraní API.)

Vyhledejte a otevřete soubor web.xml ve vašem projektu a slučte následující kód pod uzlem webové aplikace, které jsou nakonfigurované filtry aplikace.

Chcete-li získat nejpřesnější výsledky, musí být filtr namapován před všemi ostatními filtry.

    <filter>
      <filter-name>ApplicationInsightsWebFilter</filter-name>
      <filter-class>
        com.microsoft.applicationinsights.web.internal.WebRequestTrackingFilter
      </filter-class>
    </filter>
    <filter-mapping>
       <filter-name>ApplicationInsightsWebFilter</filter-name>
       <url-pattern>/*</url-pattern>
    </filter-mapping>

#### Pokud používáte MVC 3.1 nebo novější

Upravte tyto prvky, aby zahrnovaly balíček Application Insights:

    <context:component-scan base-package=" com.springapp.mvc, com.microsoft.applicationinsights.web.spring"/>

    <mvc:interceptors>
        <mvc:interceptor>
            <mvc:mapping path="/**"/>
            <bean class="com.microsoft.applicationinsights.web.spring.RequestNameHandlerInterceptorAdapter" />
        </mvc:interceptor>
    </mvc:interceptors>

#### Pokud používáte Struts 2

Tuto položku přidáte do konfiguračního souboru Struts (obvykle s názvem struts.xml nebo struts default.xml):

     <interceptors>
       <interceptor name="ApplicationInsightsRequestNameInterceptor" class="com.microsoft.applicationinsights.web.struts.RequestNameInterceptor" />
     </interceptors>
     <default-interceptor-ref name="ApplicationInsightsRequestNameInterceptor" />

(Pokud máte sběrače definované ve výchozím zásobníku, lze sběrač jednoduše přidat do tohoto balíku.)



## 5. Spusťte aplikaci

Buď ji spusťte v režimu ladění na vývojovém počítači, nebo publikujte na serveru.

## 6. Zobrazte telemetrii ve službě Application Insights


Vraťte se do prostředku Application Insights na [portálu Microsoft Azure](https://portal.azure.com).

Data požadavků HTTP se zobrazí v okně přehled. (Pokud zde nejsou, počkejte několik sekund a pak klikněte na tlačítko Aktualizovat.)

![ukázková data](./media/app-insights-java-get-started/5-results.png)

[Další informace o metrikách.][metriky]

Proklikejte se prostřednictvím jakékoli grafu pro zobrazení podrobnějších agregovaných metrik.

![](./media/app-insights-java-get-started/6-barchart.png)

> Application Insights předpokládá, že formát požadavků HTTP pro aplikace MVC je: `VERB controller/action`. Například `GET Home/Product/f9anuh81`, `GET Home/Product/2dffwrf5` a `GET Home/Product/sdf96vws` budou seskupeny do `GET Home/Product`. To umožňuje smysluplné agregace požadavků, například počtu požadavků a průměrné doba provádění požadavků.


### Data instance 

Proklikejte se jednotlivými typy konkrétního požadavku pro zobrazení jednotlivých instancí. 

Ve službě Application Insights se zobrazí dva druhy dat: agregovaná data, uložená a zobrazená jako průměry, počty a součty; a data instancí – jednotlivé sestavy požadavků protokolu HTTP, výjimky, zobrazení stránek nebo uživatelské události.

Při zobrazení vlastností požadavku uvidíte telemetrické události související s například požadavky a výjimkami.

![](./media/app-insights-java-get-started/7-instance.png)


### Analýzy: účinný dotazovací jazyk

Jak shromažďujete další data, můžete spouštět dotazy obou ke shromáždění dat a k nalezení jednotlivých instancí. [Analýzy]() představují výkonný nástroj jak pro vysvětlení výkonu, tak i využití a k diagnostickým účelům.

![Příklad analýz](./media/app-insights-java-get-started/025.png)


## 5. Nainstalujte aplikaci na server

Teď publikujte aplikaci na server, dovolte osobám ji používat a sledujte telemetrii zobrazenou na portálu.

* Ujistěte se, že brána firewall umožňuje vaší aplikace odesílat telemetrii na tyto porty:

 * dc.services.visualstudio.com:443
 * dc.services.visualstudio.com:80
 * f5.services.visualstudio.com:443
 * f5.services.visualstudio.com:80


* Na serverech Windows nainstalujte:

 * [Microsoft Visual C++ Redistributable](http://www.microsoft.com/download/details.aspx?id=40784)

    (Umožňuje čítače výkonu.)

## Výjimky a chyby požadavků

Nezpracované výjimky jsou shromažďovány automaticky:

![Přejděte dolů a klikněte na dlaždici Selhání](./media/app-insights-java-get-started/21-exceptions.png)

Chcete-li shromažďovat data o dalších výjimkách, máte dvě možnosti:

* [Vložit volání pro trackException() do vašeho kódu][apiexceptions]. 
* [Nainstalovat na server agenta Java](app-insights-java-agent.md). Určete metody, které chcete sledovat.


## Volání metody monitorování a externí závislosti

[Nainstalujte agenta Java](app-insights-java-agent.md) k protokolování určených vnitřních metod a volání provedená prostřednictvím JDBC s daty časování.


## Čítače výkonu

Klikněte na dlaždici **Servery** a uvidíte rozsah čítačů výkonu.


![](./media/app-insights-java-get-started/11-perf-counters.png)

### Vlastní nastavení kolekce čítačů výkonu

Pro zakázání shromažďování standardní sady čítačů výkonu přidejte následující kód do kořenového uzlu souboru ApplicationInsights.xml:

    <PerformanceCounters>
       <UseBuiltIn>False</UseBuiltIn>
    </PerformanceCounters>

### Shromažďování dalších čítačů výkonu

Můžete zadat další čítače výkonu, které se mají shromažďovat.

#### Čítače JMX (vystavené ve virtuálním počítači Java)

    <PerformanceCounters>
      <Jmx>
        <Add objectName="java.lang:type=ClassLoading" attribute="TotalLoadedClassCount" displayName="Loaded Class Count"/>
        <Add objectName="java.lang:type=Memory" attribute="HeapMemoryUsage.used" displayName="Heap Memory Usage-used" type="composite"/>
      </Jmx>
    </PerformanceCounters>

*   `displayName` – Název zobrazený na portálu služby Application Insights.
*   `objectName` – Název objektu JMX.
*   `attribute` – Atribut názvu objektu JMX k načtení
*   `type` (volitelné) – typ atributu JMX objektu:
 *  Výchozí hodnota: jednoduchý typ, například int nebo long.
 *  `composite`: data čítače výkonu jsou ve formátu „Attribute.Data“
 *  `tabular`: data čítače výkonu jsou ve formátu řádku tabulky



#### Čítače výkonu Windows

Každý [čítač výkonu systému Windows](https://msdn.microsoft.com/library/windows/desktop/aa373083.aspx) je členem určité kategorie (stejným způsobem, jakým je pole členem třídy). Kategorie mohou být buď globální, nebo mohou mít číslované nebo pojmenované instance.

    <PerformanceCounters>
      <Windows>
        <Add displayName="Process User Time" categoryName="Process" counterName="%User Time" instanceName="__SELF__" />
        <Add displayName="Bytes Printed per Second" categoryName="Print Queue" counterName="Bytes Printed/sec" instanceName="Fax" />
      </Windows>
    </PerformanceCounters>

*   displayName – Název zobrazený na portálu služby Application Insights.
*   categoryName – kategorie čítače výkonu (objekt výkonu) ke kterému je přiřazen tento čítač výkonu.
*   counterName – název čítače výkonu.
*   instanceName – název instance kategorie čítače výkonu nebo prázdný řetězec (""), pokud kategorie obsahuje jednu instanci. Pokud je categoryName proces a čítač výkonu, který chcete shromáždit, pochází z aktuálního procesu JVM, na kterém běží vaše aplikace, zadejte `"__SELF__"`.

Čítače výkonu jsou viditelné jako vlastní metriky v metrikách [Průzkumníka metrik][].

![](./media/app-insights-java-get-started/12-custom-perfs.png)


### Čítače výkonu Unix

* [Nainstalujte collectd s modulem plug-in Application Insights](app-insights-java-collectd.md) a získejte celou řadu dat systému a sítě.

## Získejte data uživatele a relace

Takže odesíláte telemetrii z webového serveru. Teď pokud chcete získat úplné 360stupňové zobrazení vaší aplikace, můžete přidat další monitorování:

* [Přidání telemetrických údajů do využití webových stránek][] pro monitorování zobrazení stránek a metrik uživatele.
* [Nastavit testy webu][dostupnosti] a ujistěte se, že vaše aplikace zůstává aktivní a reagující.

## Zaznamenat trasování protokolu

Službu Application Insights můžete použít k nařezání a rozčlenění protokolů z Log4J, Logback nebo jiných rozhraní protokolování. Protokoly mohou korelovat s požadavky HTTP a další telemetrií. [Jak na ][javalogs].

## Odeslat vlastní telemetrii

Teď, když jste nainstalovali sadu SDK, můžete použít rozhraní API k odeslání vlastní telemetrie.

* [Sledujte vlastní události a metriky][rozhraní api] a dozvíte se, jak uživatelé pracují s vaší aplikací.
* [Vyhledávejte diagnostiku událostí a protokolů][] pro pomoc s diagnostikou problémů.


## Testy dostupnosti webu

Application Insights může otestovat váš web v pravidelných intervalech a zkontrolovat, zda je funkční a dobře reaguje. [Chcete-li nastavit][dostupnosti], přejděte dolů a klikněte na tlačítko Dostupnost.

![Posuňte se dolů, klikněte na tlačítko Dostupnost a pak Přidat test webu](./media/app-insights-java-get-started/31-config-web-test.png)

Získáte tabulky s dobami odezvy a navíc e-mailová oznámení, pokud váš web nefunguje.

![Příklad webového testu](./media/app-insights-java-get-started/appinsights-10webtestresult.png)

[Další informace o dostupnosti webových testů .][dostupnost] 






## Máte dotazy? Problémy?

[Řešení potíží Java](app-insights-java-troubleshoot.md)

## Další kroky

Další informace naleznete ve [Středisku pro vývojáře Java](/develop/java/).

<!--Link references-->

[rozhraní api]: app-insights-api-custom-events-metrics.md
[apiexceptions]: app-insights-api-custom-events-metrics.md#track-exception
[dostupnosti]: app-insights-monitor-web-app-availability.md
[diagnostika]: app-insights-diagnostic-search.md
[eclipse]: app-insights-java-eclipse.md
[javalogs]: app-insights-java-trace-logs.md
[metriky]: app-insights-metrics-explorer.md
[využití]: app-insights-web-track-usage.md



<!--HONumber=Jun16_HO2-->


