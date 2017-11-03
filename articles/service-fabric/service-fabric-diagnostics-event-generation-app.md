---
title: "Úrovni aplikace Azure Service Fabric monitorování | Microsoft Docs"
description: "Další informace o aplikaci a události na úrovni služby a protokoly použít k monitorování a diagnostice Azure Service Fabric clustery."
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
ms.date: 10/15/2017
ms.author: dekapur
ms.openlocfilehash: 9b07c7cd256cea31c5654f0c3325e9fb675b39f8
ms.sourcegitcommit: a7c01dbb03870adcb04ca34745ef256414dfc0b3
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/17/2017
---
# <a name="application-and-service-level-event-and-log-generation"></a>Aplikace a služby na úrovni generování událostí a protokolů

## <a name="instrumenting-the-code-with-custom-events"></a>Instrumentace kód vlastní události

Instrumentace kód je základem pro většině ostatních aspektů monitorování vašim službám. Instrumentace je jediným způsobem, jak můžete víte, že je něco špatně a ke zjištění toho, co je nutné opravit. Přestože je technicky možné se připojit ladicí program k produkčním služby, není běžnou praxi. Nutnosti podrobně data instrumentace je tedy důležité.

Některé produkty, automaticky instrumentace vašeho kódu. I když tato řešení můžete dobře fungovat, je vyžadováno téměř vždy ruční instrumentace. V části end musí mít dostatek informací k forensically ladění aplikace. Tento dokument popisuje různé přístupy k instrumentaci kódu a kdy zvolit jeden ze způsobů oproti jinému.

## <a name="eventsource"></a>EventSource
Když vytvoříte řešení Service Fabric ze šablony v sadě Visual Studio **EventSource**-odvozené třídy (**ServiceEventSource** nebo **ActorEventSource**) se vygeneruje. Je vytvořit šablonu, ve kterém můžete přidat události pro vaše aplikace nebo služba. **EventSource** název **musí** být jedinečné a musí jej přejmenovat z výchozí šablony řetězec Moje_firma -&lt;řešení&gt;-&lt;projektu&gt;. S více **EventSource** definice, které používají stejný název způsobuje problém v době běhu. Každé definované události, musí mít jedinečný identifikátor. Pokud není jedinečný identifikátor, dojde k selhání běhového prostředí. Některé organizace preassign rozsahy hodnoty pro identifikátory, aby nedocházelo ke konfliktům mezi samostatnou vývojové týmy. Další informace najdete v tématu [hovou na blogu](https://blogs.msdn.microsoft.com/vancem/2012/07/09/introduction-tutorial-logging-etw-events-in-c-system-diagnostics-tracing-eventsource/) nebo [dokumentace MSDN](https://msdn.microsoft.com/library/dn774985(v=pandp.20).aspx).

### <a name="using-structured-eventsource-events"></a>Používání strukturovaného událostí EventSource

Každá z událostí v příklady kódu v této části jsou definovány pro konkrétní případ, například při registraci typu služby. Když definujete zprávy případu použití, data se dá zabalit s text chyby a více můžete snadno hledat a filtr na základě názvy nebo hodnoty zadané vlastnosti. Strukturování díky výstup instrumentace usnadňují využívat, ale vyžaduje další myšlenku a čas pro definovat novou událost pro každý případ použití. Některé události definice mohlo sdílet napříč celou aplikaci. Například metoda spuštění nebo zastavení by být událostí opětovně použít napříč mnoha služeb v rámci aplikace. Může mít specifické pro doménu služby, jako jsou systému pořadí **CreateOrder** událost, která má svůj vlastní jedinečný událostí. Tento přístup může generovat mnoho událostí a potenciálně vyžadují koordinaci identifikátory v rámci projektové týmy. 

```csharp
    [EventSource(Name = "MyCompany-VotingState-VotingStateService")]
    internal sealed class ServiceEventSource : EventSource
    {
        public static readonly ServiceEventSource Current = new ServiceEventSource();

        // The instance constructor is private to enforce singleton semantics.
        private ServiceEventSource() : base() { }

        ...

        // The ServiceTypeRegistered event contains a unique identifier, an event attribute that defined the event, and the code implementation of the event.
        private const int ServiceTypeRegisteredEventId = 3;
        [Event(ServiceTypeRegisteredEventId, Level = EventLevel.Informational, Message = "Service host process {0} registered service type {1}", Keywords = Keywords.ServiceInitialization)]
        public void ServiceTypeRegistered(int hostProcessId, string serviceType)
        {
            WriteEvent(ServiceTypeRegisteredEventId, hostProcessId, serviceType);
        }

        // The ServiceHostInitializationFailed event contains a unique identifier, an event attribute that defined the event, and the code implementation of the event.
        private const int ServiceHostInitializationFailedEventId = 4;
        [Event(ServiceHostInitializationFailedEventId, Level = EventLevel.Error, Message = "Service host initialization failed", Keywords = Keywords.ServiceInitialization)]
        public void ServiceHostInitializationFailed(string exception)
        {
            WriteEvent(ServiceHostInitializationFailedEventId, exception);
        }
```

### <a name="using-eventsource-generically"></a>Pomocí EventSource obecně

Protože definování určité události může být složité, definujte Spousta lidí několik událostí s společnou sadu parametrů, které obvykle výstup své informace jako řetězec. Velká část strukturovaných aspekt dojde ke ztrátě, a je obtížnější k vyhledávání a filtrování výsledků. V tomto přístupu jsou definovány několik události, které obvykle odpovídají úrovně protokolování. Následující fragment kódu definuje ladění a chybové zprávy:

```csharp
    [EventSource(Name = "MyCompany-VotingState-VotingStateService")]
    internal sealed class ServiceEventSource : EventSource
    {
        public static readonly ServiceEventSource Current = new ServiceEventSource();

        // The Instance constructor is private, to enforce singleton semantics.
        private ServiceEventSource() : base() { }

        ...

        private const int DebugEventId = 10;
        [Event(DebugEventId, Level = EventLevel.Verbose, Message = "{0}")]
        public void Debug(string msg)
        {
            WriteEvent(DebugEventId, msg);
        }

        private const int ErrorEventId = 11;
        [Event(ErrorEventId, Level = EventLevel.Error, Message = "Error: {0} - {1}")]
        public void Error(string error, string msg)
        {
            WriteEvent(ErrorEventId, error, msg);
        }
```

Pomocí hybridní strukturovaných a obecná instrumentace můžete použít také. Strukturované instrumentace se používá pro vytváření sestav chyb a metriky. Obecné události lze použít pro podrobné protokolování, který využívá techniky pro řešení potíží.

## <a name="aspnet-core-logging"></a>ASP.NET Core protokolování

Je důležité pečlivě naplánovat, jak bude instrumentace vašeho kódu. Plán správné instrumentace můžete vyhnout potenciálně destabilizing vaše základu kódu a pak se museli reinstrument kód. Aby se snížilo riziko, můžete knihovna nástrojů, jako je [Microsoft.Extensions.Logging](https://www.nuget.org/packages/Microsoft.Extensions.Logging/), který je součástí Microsoft ASP.NET Core. ASP.NET Core má [objektu ILogger](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.ilogger) rozhraní, které můžete použít s poskytovatelem podle vaší volby, a současně minimalizujete její vliv na existující kód. Můžete použít kód v ASP.NET Core v systému Windows a Linux, a v úplné rozhraní .NET Framework, takže je váš kód instrumentace standardizované. To je dále zkoumat níže:

### <a name="using-microsoftextensionslogging-in-service-fabric"></a>Použití Microsoft.Extensions.Logging v Service Fabric

1. Do projektu, které chcete přidat balíček Microsoft.Extensions.Logging NuGet nástrojích. Navíc přidat všechny balíčky zprostředkovatele (pro balíček třetích stran, viz následující příklad). Další informace najdete v tématu [protokolování v ASP.NET Core](https://docs.microsoft.com/aspnet/core/fundamentals/logging).
2. Přidat **pomocí** direktivy pro Microsoft.Extensions.Logging do souboru služby.
3. Definování soukromé proměnné v třídě služby.

  ```csharp
  private ILogger _logger = null;

  ```
4. V konstruktoru třídy vaší služby přidejte tento kód:

  ```csharp
  _logger = new LoggerFactory().CreateLogger<Stateless>();

  ```
5. Spuštění kódu v vaše metody instrumentace. Tady je několik ukázky:

  ```csharp
  _logger.LogDebug("Debug-level event from Microsoft.Logging");
  _logger.LogInformation("Informational-level event from Microsoft.Logging");

  // In this variant, we're adding structured properties RequestName and Duration, which have values MyRequest and the duration of the request.
  // Later in the article, we discuss why this step is useful.
  _logger.LogInformation("{RequestName} {Duration}", "MyRequest", requestDuration);

  ```

## <a name="using-other-logging-providers"></a>Použití dalších zprostředkovatelů protokolování

Některé použití poskytovatelů třetích stran přístupu popsané v předchozí části, včetně [Serilog](https://serilog.net/), [NLog](http://nlog-project.org/), a [Loggr](https://github.com/imobile3/Loggr.Extensions.Logging). Každá z těchto do ASP.NET Core protokolování je možné připojit nebo je můžete použít samostatně. Serilog obsahuje funkce, která vylepšuje všech zpráv odeslaných z protokolovacího nástroje. Tato funkce může být užitečné k vypsání názvu služby, typ a informace o oddílu. Chcete-li použít tuto funkci v infrastruktuře ASP.NET Core, proveďte tyto kroky:

1. Přidání balíčků Serilog, Serilog.Extensions.Logging a Serilog.Sinks.Observable NuGet do projektu. Další příklad také přidáte Serilog.Sinks.Literate. Lepší přístup se zobrazí později v tomto článku.
2. V Serilog vytvořte LoggerConfiguration a instance protokolovacího nástroje.

  ```csharp
  Log.Logger = new LoggerConfiguration().WriteTo.LiterateConsole().CreateLogger();
  ```

3. Přidejte Serilog.ILogger argument konstruktoru služby a předat nově vytvořený protokolovacího nástroje.

  ```csharp
  ServiceRuntime.RegisterServiceAsync("StatelessType", context => new Stateless(context, Log.Logger)).GetAwaiter().GetResult();
  ```

4. V konstruktoru služby, přidejte následující kód, který vytvoří enrichers vlastnost pro **ServiceTypeName**, **ServiceName**, **PartitionId**, a **identifikátor InstanceId** vlastnosti služby. Také přidá vlastnost enricher k objektu pro vytváření protokolování ASP.NET Core, abyste je mohli používat Microsoft.Extensions.Logging.ILogger ve vašem kódu.

  ```csharp
  public Stateless(StatelessServiceContext context, Serilog.ILogger serilog)
      : base(context)
  {
      PropertyEnricher[] properties = new PropertyEnricher[]
      {
          new PropertyEnricher("ServiceTypeName", context.ServiceTypeName),
          new PropertyEnricher("ServiceName", context.ServiceName),
          new PropertyEnricher("PartitionId", context.PartitionId),
          new PropertyEnricher("InstanceId", context.ReplicaOrInstanceId),
      };

      serilog.ForContext(properties);

      _logger = new LoggerFactory().AddSerilog(serilog.ForContext(properties)).CreateLogger<Stateless>();
  }
  ```

5. Instrumentace kód stejné jako kdyby byly pomocí ASP.NET Core bez Serilog.

  >[!NOTE]
  >Doporučujeme vám, že nepoužíváte statické Log.Logger s v předchozím příkladu. Service Fabric může být hostitelem více instancí stejného typu služby v rámci jednoho procesu. Pokud používáte statické Log.Logger, zobrazí poslednímu zapisujícímu enrichers vlastnost hodnoty pro všechny instance, které jsou spuštěné. Toto je jedním z důvodů, proč proměnnou _logger je proměnná privátního člena třídy služeb. Navíc musíte nastavit _logger k dispozici pro společný kód, který může být použit ve službách.

## <a name="choosing-a-logging-provider"></a>Výběr zprostředkovatele protokolování

Pokud vaše aplikace využívá vysoký výkon, **EventSource** je obvykle dobrou přístup. **EventSource** *obecně* využívá méně prostředků a provede lépe než ASP.NET Core protokolování ani žádný z dostupných řešení třetí strany.  Tato akce není problém pro mnoho služby, ale pokud je vaše služba orientovaných na výkon, pomocí **EventSource** může být vhodnější. Však získat tyto výhody strukturovaná protokolování, **EventSource** vyžaduje větší investice z technickému týmu. Pokud je to možné nezadávejte rychlé prototyp několik možností protokolování a potom vyberte ten, který nejlépe vyhovuje vašim potřebám.

## <a name="next-steps"></a>Další kroky

Jakmile jste vybrali poskytovatele protokolování a instrumentace aplikací a služeb, protokolů a událostí muset agregovat před odesláním pro žádnou platformu analysis. Přečtěte si informace o [EventFlow](service-fabric-diagnostics-event-aggregation-eventflow.md) a [WAD](service-fabric-diagnostics-event-aggregation-wad.md) lépe pochopit některé doporučené možnosti.
