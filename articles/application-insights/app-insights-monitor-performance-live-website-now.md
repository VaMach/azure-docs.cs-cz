<properties
    pageTitle="Diagnostika problémů s výkonem při spuštění webu IIS | Microsoft Azure"
    description="Monitorování výkonu webu bez opětovného nasazení. Používat samostatně nebo pomocí Application Insights SDK pro získání telemetrických závislostí."
    services="application-insights"
    documentationCenter=".net"
    authors="alancameronwills"
    manager="douge"/>

<tags
    ms.service="application-insights"
    ms.workload="tbd"
    ms.tgt_pltfrm="ibiza"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="03/09/2016"
    ms.author="awills"/>


# Nainstalujte monitorování stavu Application Insights pro sledování výkonu webu

*Application Insights je ve verzi Preview.*

Stav monitorování sady Visual Studio Application Insights umožňuje diagnostikovat výjimky a problémy s výkonem v aplikacích technologie ASP.NET. 

![ukázkové grafy](./media/app-insights-monitor-performance-live-website-now/10-intro.png)

> [AZURE.TIP] Existují samostatné články o instrumentaci [živých webových aplikací J2EE](app-insights-java-live.md) a [Azure Cloud Services](app-insights-cloudservices.md).


Máte možnost volby tří způsobů, jak použít Application Insights pro vaše webové aplikace služby IIS:

* **Čas sestavení:** [Přidejte Application Insights SDK][greenbrown] do kódu webové aplikace. To vám poskytuje:
 * Rozsah standardní diagnostiky a telemetrii využití.
 *  Rozhraní [Application Insights API][api] umožňuje psát vlastní telemetrii ke sledování podrobného využití nebo diagnostiky problémů.
* **Čas spuštění:** monitorování stavu použijte k instrumentaci vaší webové aplikace na server.
 * Sledování webových aplikací, které jsou již spuštěny: není třeba je znovu sestavit nebo znovu publikovat.
 * Rozsah standardní diagnostiky a telemetrii využití.
 * Diagnostika závislosti& #151; vyhledá chyby nebo nízký výkon, kde vaše aplikace používá jiné komponenty, například databáze, REST API nebo jiné služby.
 * Vyřešte všechny problémy s telemetrií.
* **Oboje:** Zkompilujte sadu SDK do kódu webové aplikace a spusťte monitorování stavu na webovém serveru.  Nejlepší z obou světů:
 * Standardní diagnostika a využití telemetrie.
 * Diagnostika závislostí.
 * Rozhraní API umožňuje psát vlastní telemetrii.
 * Vyřešte všechny problémy s SDK a telemetrií.


## Nainstalujte monitorování stavu služby Application Insights

Budete potřebovat předplatné [Microsoft Azure](http://azure.com).

### Pokud vaše aplikace běží na serveru služby IIS

1. Na webovém serveru IIS se přihlaste s pověřeními správce.
2. Stažení a spuštění [instalačního programu Sledování stavu](http://go.microsoft.com/fwlink/?LinkId=506648).
4. V průvodci instalací se přihlaste do Microsoft Azure.

    ![Přihlaste se do Azure pomocí přihlašovacích údajů účtu Microsoft](./media/app-insights-monitor-performance-live-website-now/appinsights-035-signin.png)

    *Chyby připojení? Viz [Poradce při potížích](#troubleshooting).*

5. Vyberte nainstalované webové aplikace nebo weby, které chcete monitorovat a potom nakonfigurujte prostředky, ve kterých chcete zobrazit výsledky v portálu služeb Application Insights.

    ![Vyberte aplikaci a prostředek.](./media/app-insights-monitor-performance-live-website-now/appinsights-036-configAIC.png)

    Za normálních okolností provedete konfiguraci nového prostředku a [skupiny prostředků][role].

    Jinak použijte existující prostředek, pokud jste již nastavili [webové testy][dostupnosti] pro lokalitu, nebo [monitorování webového klienta][klient].

6. Restartujte službu IIS.

    ![Zvolte restartování v horní části dialogového okna.](./media/app-insights-monitor-performance-live-website-now/appinsights-036-restart.png)

    Webová služba bude na krátkou dobu přerušena.

6. Všimněte si, že soubor ApplicationInsights.config byl vložen do webových aplikací, které chcete monitorovat.

    ![Najděte soubor .config společně se soubory kódu webové aplikace.](./media/app-insights-monitor-performance-live-website-now/appinsights-034-aiconfig.png)

   Existují také některé změny v souboru web.config.

#### Chcete (re)konfigurovat později?

Po dokončení průvodce můžete agenta znovu nakonfigurovat vždy, když chcete. Můžete také použít tuto hodnotu, pokud jste nainstalovali agenta, ale došlo k potížím s počátečním nastavením.

![Klikněte na ikonu Application Insights na hlavním panelu](./media/app-insights-monitor-performance-live-website-now/appinsights-033-aicRunning.png)


### Pokud vaše aplikace běží jako webová aplikace Azure

V ovládacích panelech webové aplikace Azure přidejte rozšíření Application Insights.

![Ve vaší webové aplikaci, nastavení, rozšíření, přidat, Application Insights](./media/app-insights-monitor-performance-live-website-now/05-extend.png)


### Pokud se jedná o projekt cloudových služeb Azure

[Přidat skripty pro webové a pracovní role](app-insights-cloudservices.md).


## Zobrazení výkonu telemetrie

Přihlaste se [na portál Azure](https://portal.azure.com), vyhledejte Application Insights a otevřete vytvořený zdroj.

![Vyberte možnost Procházet, Application Insights a pak vyberte aplikaci](./media/app-insights-monitor-performance-live-website-now/appinsights-08openApp.png)

Otevřete okno Výkon a zobrazte si žádost, dobu odezvy, závislosti a další data.

![Výkon](./media/app-insights-monitor-performance-live-website-now/21-perf.png)

Chcete-li upravit podrobnosti informací o zobrazení nebo přidat nový graf.


![](./media/app-insights-monitor-performance-live-website-now/appinsights-038-dependencies.png)

## Závislosti

Graf doby trvání závislosti zobrazuje dobu trvání zabranou voláním z aplikace na externí komponenty, například databáze, REST API nebo úložiště objektů blob Azure.

Pro segmentování grafu voláním různých závislostí vyberte graf, zapněte seskupování a pak zvolte závislost, typ závislosti nebo závislosti výkonu.

Můžete graf také filtrovat a podívat se na konkrétní závislosti, typ nebo výkon. Klikněte na tlačítko Filtry.

## Čítače výkonu

(Není pro webové aplikace Azure.) Klikněte na tlačítko Servery v okně Přehled a zobrazte grafy čítačů výkonu serveru, například využití procesoru a využití paměti.

Přidejte nový graf nebo klikněte na graf a změňte, co zobrazí. 

Můžete také [změnit sadu čítačů výkonu, které jsou hlášeny sadou SDK](app-insights-configuration-with-applicationinsights-config.md#nuget-package-3). 

## Výjimky

![Klikněte na tlačítko prostřednictvím grafu výjimek serveru](./media/app-insights-monitor-performance-live-website-now/appinsights-039-1exceptions.png)

Můžete přejít k podrobnostem a specifickým výjimkám (z posledních sedmi dnů) a získat trasování zásobníku a data kontextu.

## Vzorkování

Pokud vaše aplikace odešle velké množství dat a používáte Application Insights SDK pro verze technologie ASP.NET 2.0.0-beta3 nebo novější, může funkce adaptivního vzorkování pracovat a odesílat pouze procento vaší telemetrie. [Další informace o vzorkování.](app-insights-sampling.md)


## Řešení potíží

### Chyby připojení

Je třeba otevřít některé odchozí porty v bráně firewall a povolit funkci sledování stavu:

+ Telemetrie – je potřeba vždy:
 +  `dc.services.visualstudio.com:80`
 +  `dc.services.visualstudio.com:443`
 +  `dc.applicationinsights.microsoft.com`
+ Konfigurace – je potřeba pouze při provádění změn:
 -  `management.core.windows.net:443`
 -  `management.azure.com:443`
 -  `login.windows.net:443`
 -  `login.microsoftonline.com:443`
 -  `secure.aadcdn.microsoftonline-p.com:443`
 -  `auth.gfx.ms:443`
 -  `login.live.com:443`
+ Instalace:
 +  `packages.nuget.org:443`

Tento seznam se může občas měnit.

### Žádná telemetrie?

  * Použijte web k vygenerování některých dat.
  * Počkejte několik minut a nechte data dorazit a pak klikněte na tlačítko **Aktualizovat**.
  * Otevřete Vyhledávání diagnostiky (dlaždice Hledání) a zobrazte jednotlivé události. Události jsou často viditelné ve Vyhledávání diagnostiky před zobrazením agregovaných dat v grafech.
  * Otevřete monitorování stavu a vyberte svou aplikaci v levém podokně. Zkontrolujte, zda existují jakékoli zprávy diagnostiky pro tuto aplikaci v části „Konfigurace oznámení“:

  ![](./media/app-insights-monitor-performance-live-website-now/appinsights-status-monitor-diagnostics-message.png)

  * Ujistěte se, že server brány firewall umožňuje odchozí přenosy na těchto portech uvedených výše.
  * Na serveru, pokud se zobrazí zpráva o „nedostatečných oprávněních“, zkuste následující postup:
    * Ve Správci služby IIS vyberte fond aplikací, otevřete položku **Upřesnit nastavení**, a v části **Model procesu** si povšimněte identity.
    * V ovládacích panelech správy počítače přidejte tuto identitu do skupiny uživatelů Sledování výkonu.
  * Pokud máte MMA/SCOM nainstalovaný na serveru, může dojít u některých verzí ke konfliktu. Odinstalujte SCOM a sledování stavu a znovu nainstalujte nejnovější verze.
  * Viz [Poradce při potížích][qna].

## Systémové požadavky

Podpora operačního systému pro sledování stavu Application Insights na serveru:

- Windows Server 2008
- Windows Server 2008 R2
- Windows Server 2012
- Windows server 2012 R2

pomocí nejnovější aktualizace SP a rozhraní .NET Framework 4.0 a 4.5

Na straně klienta systému Windows 7, 8 a 8.1, znovu s rozhraním .NET Framework 4.0 a 4.5

Podpora služby IIS je: IIS 7, 7.5, 8, 8.5 (je vyžadována služba IIS)

## Automatizace v prostředí PowerShell

Můžete spustit a zastavit monitorování pomocí prostředí PowerShell.

`Get-ApplicationInsightsMonitoringStatus [-Name appName]`

* `-Name` (Volitelné) Název webové aplikace.
* Zobrazí sledování stavu Application Insights pro každou webovou aplikaci (nebo pojmenované aplikace) na tomto serveru služby IIS.

* Vrátí `ApplicationInsightsApplication` pro každou aplikaci:
 * `SdkState==EnabledAfterDeployment`: Aplikace je monitorována a byla instrumentována v době běhu, buď pomocí nástroje Monitor stavu, nebo `Start-ApplicationInsightsMonitoring`.
 * `SdkState==Disabled`: Aplikace není instrumentována pro službu Application Insights. Buď nebyla nikdy instrumentována, nebo bylo zakázáno spuštění sledování pomocí nástroje Monitor stavu nebo pomocí `Stop-ApplicationInsightsMonitoring`.
 * `SdkState==EnabledByCodeInstrumentation`: Aplikace byla instrumentována přidáním sady SDK ke zdrojovému kódu. Její SDK nelze aktualizovat ani zastavit.
 * `SdkVersion` zobrazuje verzi používanou pro monitorování této aplikace.
 * `LatestAvailableSdkVersion`zobrazuje verzi aktuálně k dispozici v galerii NuGet. Chcete-li upgradovat aplikaci na tuto verzi, použijte `Update-ApplicationInsightsMonitoring`.

`Start-ApplicationInsightsMonitoring -Name appName -InstrumentationKey 00000000-000-000-000-0000000`

* `-Name` Název aplikace v IIS
* `-InstrumentationKey` Ikey prostředku Application Insights, kde chcete zobrazit výsledky.

* Tato rutina ovlivní pouze aplikace, které již nejsou instrumentovány – to znamená, SdkState==NotInstrumented.

    Rutina nemá vliv na aplikaci, která je již instrumentována, v okamžiku sestavení přidáním sady SDK do kódu nebo v době běhu předchozí pomocí této rutiny.

    Verze sady SDK používaná k instrumentaci aplikace je verze, která byla naposledy stažena do tohoto serveru.

    Chcete-li stáhnout poslední verzi, použijte příkaz Update-ApplicationInsightsVersion.

* V případě úspěchu vrátí `ApplicationInsightsApplication`. Pokud se nezdaří, zaprotokoluje trasování do stderr.

    
          Name                      : Default Web Site/WebApp1
          InstrumentationKey        : 00000000-0000-0000-0000-000000000000
          ProfilerState             : ApplicationInsights
          SdkState                  : EnabledAfterDeployment
          SdkVersion                : 1.2.1
          LatestAvailableSdkVersion : 1.2.3

`Stop-ApplicationInsightsMonitoring [-Name appName | -All]`

* `-Name` Název aplikace v IIS
* `-All` Zastaví monitorování všech aplikací v tomto serveru IIS, pro který `SdkState==EnabledAfterDeployment`

* Zastaví monitorování zadané aplikace a odebere instrumentace. Pracuje pouze pro aplikace, které byly instrumentovány v době běhu pomocí nástroje pro monitorování stavu nebo příkazu Start-ApplicationInsightsApplication. (`SdkState==EnabledAfterDeployment`)

* Vrátí ApplicationInsightsApplication.

`Update-ApplicationInsightsMonitoring -Name appName [-InstrumentationKey "0000000-0000-000-000-0000"`]

* `-Name`: Název webové aplikace ve službě IIS.
* `-InstrumentationKey` (Volitelné.) Tuto položku použijte ke změně prostředku, na kterou se telemetrie aplikace odesílá.
* Tato rutina:
 * Upgrady pojmenované aplikace na verzi sady SDK naposledy stažené v tomto počítači. (Funguje pouze v případě `SdkState==EnabledAfterDeployment`)
 * Pokud jste zadali kód instrumentace, pojmenovaná aplikace se překonfiguruje na odeslání telemetrie do prostředku s tímto klíčem. (Funguje v případě `SdkState != Disabled`)

`Update-ApplicationInsightsVersion`

* Stáhne nejnovější Application Insights SDK na server.

## Šablony Azure

Pokud je webová aplikace v Azure a vy vytvoříte své prostředky pomocí šablony správce prostředků Azure, můžete nakonfigurovat Application Insights přidáním tohoto uzlu prostředků:

    {
      resources: [
        /* Create Application Insights resource */
        {
          "apiVersion": "2015-05-01",
          "type": "microsoft.insights/components",
          "name": "nameOfAIAppResource",
          "location": "centralus",
          "kind": "web",
          "properties": { "ApplicationId": "nameOfAIAppResource" },
          "dependsOn": [
            "[concat('Microsoft.Web/sites/', myWebAppName)]"
          ]
        }
       ]
     } 

* `nameOfAIAppResource` - název prostředku Application Insights
* `myWebAppName` - id webové aplikace

## <a name="next"></a>Další kroky

* [Tvorba webových testů][dostupnosti] a ověření, zda web zůstává živý.
* [Vyhledávejte diagnostiku událostí a protokolů][] pro pomoc s diagnostikou problémů.
* [Přidání využití telemetrie webového klienta][] pro zobrazení výjimek z kódu webové stránky a umožnění vložení trasovacího volání.
* [Přidání Application Insights SDK do vašeho kódu webové služby][greenbrown] tak, abyste mohli vložit trasování a protokol volání do kódu serveru.

## Video

#### Sledování výkonu

[AZURE.VIDEO app-insights-performance-monitoring]

<!--Link references-->

[api]: app-insights-api-custom-events-metrics.md
[dostupnosti]: app-insights-monitor-web-app-availability.md
[klient]: app-insights-javascript.md
[diagnostika]: app-insights-diagnostic-search.md
[greenbrown]: app-insights-asp-net.md
[qna]: app-insights-troubleshoot-faq.md
[role]: app-insights-resources-roles-access-control.md
[využití]: app-insights-web-track-usage.md



<!--HONumber=Jun16_HO2-->


