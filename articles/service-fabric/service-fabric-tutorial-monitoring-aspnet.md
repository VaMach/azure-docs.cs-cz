---
title: "Monitorovací a diagnostické pro služby ASP.NET Core v Azure Service Fabric | Microsoft Docs"
description: "Zjistěte, jak nastavit monitorování a Diagnostika pro aplikaci Azure Service Fabric ASP.NET Core."
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: tutorial
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 09/14/2017
ms.author: dekapur
ms.custom: mvc
ms.openlocfilehash: ce854a3dc41dec69c3f8de245a03d55a2354335f
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/08/2017
---
# <a name="monitor-and-diagnose-an-aspnet-core-application-on-service-fabric"></a>Monitorování a Diagnostika v Service Fabric aplikace ASP.NET Core
V tomto kurzu je součástí série, čtyři. Projde kroky při nastavení monitorování a Diagnostika pro aplikace ASP.NET Core běžící v clusteru Service Fabric pomocí Application Insights. Budou shromažďovány telemetrická data z aplikace vyvinuté v první část kurzu, [sestavení aplikace .NET Service Fabric](service-fabric-tutorial-create-dotnet-app.md). 

V části čtyři kurz řady se dozvíte, jak:
> [!div class="checklist"]
> * Konfigurace služby Application Insights pro aplikace
> * Shromažďovat telemetrická data odpovědi pro trasování komunikace na základě protokolu HTTP mezi službami
> * Použít funkci mapy aplikace ve službě Application Insights
> * Přidání vlastních událostí pomocí Application Insights API

V této série kurzu zjistíte, jak:
> [!div class="checklist"]
> * [Sestavení aplikace .NET Service Fabric](service-fabric-tutorial-create-dotnet-app.md)
> * [Nasazení aplikace na vzdálený cluster](service-fabric-tutorial-deploy-app-to-party-cluster.md)
> * [Konfigurace CI/CD pomocí Visual Studio Team Services](service-fabric-tutorial-deploy-app-with-cicd-vsts.md)
> * Nastavení monitorování a diagnostiky pro aplikaci

## <a name="prerequisites"></a>Požadavky
Před zahájením tohoto kurzu:
- Pokud nemáte předplatné Azure, vytvořte [bezplatný účet](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)
- [Nainstalovat Visual Studio 2017](https://www.visualstudio.com/) a nainstalujte **Azure development** a **ASP.NET a webové vývoj** úlohy.
- [Instalace Service Fabric SDK](service-fabric-get-started.md)

## <a name="download-the-voting-sample-application"></a>Stažení ukázkové aplikace Voting
Pokud není sestavení Voting ukázkovou aplikaci [součástí jeden z této série kurz](service-fabric-tutorial-create-dotnet-app.md), můžete ho stáhnout. V příkazovém okně nebo terminálu spusťte následující příkaz, který klonovat úložiště ukázkové aplikace do místního počítače.

```
git clone https://github.com/Azure-Samples/service-fabric-dotnet-quickstart
```

## <a name="set-up-an-application-insights-resource"></a>Nastavit prostředek Application Insights
Application Insights je platforma pro správu výkonu aplikace Azure a Service Fabric doporučené platforma pro monitorování aplikací a diagnostiky. Chcete-li vytvořit prostředek Application Insights, přejděte na [portál Azure](https://portal.azure.com). Klikněte na tlačítko **nový** v levé navigační nabídce otevřete Azure Marketplace. Klikněte na **monitorování + správu** a potom **Application Insights**.

![Vytvořit nový prostředek AI](./media/service-fabric-tutorial-monitoring-aspnet/new-ai-resource.png)

Teď musíte vyplnit požadované informace o atributech prostředku, který se má vytvořit. Zadejte odpovídající *název*, *skupiny prostředků*, a *předplatné*. Nastavte *umístění* kde by nasadit cluster Service Fabric v budoucnu. V tomto kurzu jsme se nasazení aplikace na místní cluster proto *umístění* pole je důležité. *Typ aplikace* by měl být ponecháno jako "Webové aplikace ASP.NET." 

![Atributy prostředků AI](./media/service-fabric-tutorial-monitoring-aspnet/new-ai-resource-attrib.png)

Jakmile jste vyplnili požadované informace, klikněte na možnost **vytvořit** ke zřízení prostředků - má o několik minut trvat. 
<!-- When completed, navigate to the newly deployed resource, and find the "Instrumentation Key" (visible in the "Essentials" drop down section). Copy it to clipboard, since we will need it in the next step. -->

## <a name="add-application-insights-to-the-applications-services"></a>Přidejte Application Insights do aplikace služby

Spusťte Visual Studio 2017 se zvýšenými oprávněními. To provedete tak, že kliknete pravým tlačítkem na ikonu sady Visual Studio v nabídce Start a výběr **spustit jako správce**. Klikněte na tlačítko **soubor** > **otevřete** > **projekt nebo řešení** a přejděte do aplikace Voting (buď vytvořené v rámci kurzu nebo git klonovat). Otevřete *Voting.sln* a pokud se zobrazí výzva k obnovení balíčků NuGet aplikace, klikněte na **Ano**.

Postupujte podle těchto kroků nakonfigurujete Application Insights pro VotingWeb a VotingData služby:
1. Klikněte pravým tlačítkem na název služby a klikněte na tlačítko **konfigurovat Application Insights...** .

    ![Konfigurace AI](./media/service-fabric-tutorial-monitoring-aspnet/configure-ai.png)

2. Klikněte na tlačítko **spustit volné**.
3. Přihlaste se ke svému účtu (který můžete také nastavit vašeho předplatného Azure) a vyberte předplatné, ve které jste vytvořili prostředku Application Insights. Najít prostředek v rámci *prostředek existující Application Insights* v rozevírací nabídce "Prostředek". Klikněte na tlačítko **zaregistrovat** pro přidání Application Insights do vaší služby.

    ![Zaregistrovat AI](./media/service-fabric-tutorial-monitoring-aspnet/register-ai.png)

4. Klikněte na tlačítko **Dokončit** po dokončení akce dialogových oken, která se objeví.
    
Zajistěte, aby uděláte výše uvedené kroky **i** služeb v aplikaci k dokončení konfigurace Application Insights pro aplikace. Stejný prostředek Application Insights se používá pro obě služby, chcete-li zobrazit příchozí a odchozí požadavky a komunikace mezi službami.

## <a name="add-the-microsoftapplicationinsightsservicefabricnative-nuget-to-the-services"></a>Přidat Microsoft.ApplicationInsights.ServiceFabric.Native NuGet ke službám.

Application Insights obsahuje dvě konkrétní NuGets Service Fabric, které lze použít v závislosti na scénáři. Jeden se používá s Service Fabric nativní služby a další s kontejnery a spustitelné soubory hosta. V takovém případě budeme používat Microsoft.ApplicationInsights.ServiceFabric.Native NuGet využívat znalost kontext služby, která přináší. Další informace o Application Insights SDK a konkrétní NuGets Service Fabric, najdete v části [Microsoft Application Insights pro Service Fabric](https://github.com/Microsoft/ApplicationInsights-ServiceFabric/blob/develop/README.md). 

Tady jsou kroky k nastavení NuGet:
1. Klikněte pravým tlačítkem na **řešení, hlasování,** v horní části Průzkumník řešení a klikněte na tlačítko **spravovat balíčky NuGet pro řešení...** .
2. Klikněte na tlačítko **Procházet** v horním navigačním panelu nabídce okno "NuGet – řešení" a kontrola **zahrnout předběžné verze** políčko vedle panel hledání.
3. Vyhledejte `Microsoft.ApplicationInsights.ServiceFabric.Native` a klikněte na odpovídající balíček NuGet.
4. Na pravé straně, klikněte na dvou políček vedle dvě služby v aplikaci **VotingWeb** a **VotingData** a klikněte na tlačítko **nainstalovat**.
    ![AI registrace je dokončena.](./media/service-fabric-tutorial-monitoring-aspnet/aisdk-sf-nuget.png)
5. Klikněte na tlačítko **OK** na *zkontrolujte změny* dialogu, který se zobrazí a přijměte *přijetí licence*. Tím dokončíte přidávání NuGet do služby.
6. Teď je potřeba nastavit inicializátoru telemetrie v dvě služby. Chcete-li to, otevřete *VotingWeb.cs* a *VotingData.cs*. U obou z nich proveďte následující dva kroky:
    1. Přidejte tyto dvě *pomocí* příkazy v horní části každé  *\<ServiceName > .cs*:
    
    ```csharp
    using Microsoft.ApplicationInsights.Extensibility;
    using Microsoft.ApplicationInsights.ServiceFabric;
    ```
    
    2. V vnořeného *vrátit* prohlášení o *CreateServiceInstanceListeners()* nebo *CreateServiceReplicaListeners()*v části *ConfigureServices*  >  *služby*, v rozmezí dvou služeb Singleton deklarovat, přidejte: `.AddSingleton<ITelemetryInitializer>((serviceProvider) => FabricTelemetryInitializerExtension.CreateFabricTelemetryInitializer(serviceContext))`.
    Tím se přidá *kontext služby* k telemetrie, což umožňuje lépe pochopit zdroj telemetrii ve službě Application Insights. Vaše vnořené *vrátit* příkaz v *VotingWeb.cs* by měla vypadat takto:
    
    ```csharp
    return new WebHostBuilder().UseWebListener()
        .ConfigureServices(
            services => services
                .AddSingleton<StatelessServiceContext>(serviceContext)
                .AddSingleton<ITelemetryInitializer>((serviceProvider) => FabricTelemetryInitializerExtension.CreateFabricTelemetryInitializer(serviceContext))
                .AddSingleton<HttpClient>())
        .UseContentRoot(Directory.GetCurrentDirectory())
        .UseStartup<Startup>()
        .UseApplicationInsights()
        .UseServiceFabricIntegration(listener, ServiceFabricIntegrationOptions.None)
        .UseUrls(url)
        .Build();
    ```

    Podobně v *VotingData.cs*, měli byste mít:

    ```csharp
    return new WebHostBuilder()
        .UseKestrel()
        .ConfigureServices(
            services => services
                .AddSingleton<StatefulServiceContext>(serviceContext)
                .AddSingleton<ITelemetryInitializer>((serviceProvider) => FabricTelemetryInitializerExtension.CreateFabricTelemetryInitializer(serviceContext))
                .AddSingleton<IReliableStateManager>(this.StateManager))
        .UseContentRoot(Directory.GetCurrentDirectory())
        .UseStartup<Startup>()
        .UseApplicationInsights()
        .UseServiceFabricIntegration(listener, ServiceFabricIntegrationOptions.UseUniqueServiceUrl)
        .UseUrls(url)
        .Build();
    ```

V tomto okamžiku jste připraveni k nasazení aplikace. Klikněte na tlačítko **spustit** nahoře (nebo **F5**), a Visual Studio bude sestavovat a balíček aplikace, nastavte místní cluster a nasazení aplikace do ní. 

Jakmile se aplikace provádí nasazení, přejděte přes [localhost: 8080](localhost:8080), kde byste měli vidět ukázková hlasování jednostránkové aplikace. Hlasovat pro několik různých položky podle svého výběru vytvořit některé ukázková data a telemetrie – I se pro zákusků!

![Ukázka hlasy AI](./media/service-fabric-tutorial-monitoring-aspnet/vote-sample.png)

Nebojte se *odebrat* některé hlasovací možnosti i po dokončení přidávání několik hlasy.

## <a name="view-telemetry-and-the-app-map-in-application-insights"></a>Zobrazení telemetrie a mapy aplikace ve službě Application Insights 

HEAD přes do zdroje Application Insights na portálu Azure a v levém navigačním panelu prostředku, klikněte na **verze Preview** pod *konfigurace*. Zapnout **na** *mapování aplikace s více rolí* v seznamu dostupná verze Preview.

![Povolit AI AppMap](./media/service-fabric-tutorial-monitoring-aspnet/ai-appmap-enable.png)

Klikněte na tlačítko **přehled** se vrátíte na cílovou stránku vaší prostředku. Pak klikněte na tlačítko **vyhledávání** v horní části zobrazíte trasování brzo. Jak dlouho trvá několik minut, než se objeví ve službě Application Insights trasování. V případě, že nemáte žádnou, počkejte několik minut a stiskněte tlačítko **aktualizovat** tlačítka v horní části.
![Trasování najdete AI](./media/service-fabric-tutorial-monitoring-aspnet/ai-search.png)

Posouvání dolů na *vyhledávání* v okně se zobrazí všechny příchozí telemetrii, zobrazí se na pole s Application Insights. Pro každou akci, kterou trvala v hlasovací aplikaci, musí být požadavek PUT odchozí ze *VotingWeb* (PUT hlasy nebo Put [name]), z příchozí žádosti PUT *VotingData* (PUT VoteData nebo Put název []), za nímž následuje pár požadavků GET pro aktualizace dat, které se zobrazí. Také bude závislostí trasování pro protokol HTTP na místního hostitele, protože existují požadavky HTTP. Tady je příklad se zobrazí pro jak jeden hlas se přidá: ![trasování požadavku ukázka AI](./media/service-fabric-tutorial-monitoring-aspnet/sample-request.png)

Kliknutím na jednom z trasování a zobrazit další podrobnosti o něm. Jsou užitečné informace o žádosti, které poskytuje Application Insights, včetně *doba odezvy* a *adrese URL žádosti*. Navíc vzhledem k tomu, že jste přidali konkrétní NuGet Service Fabric, se zobrazí také data o vaší aplikaci v kontextu clusteru Service Fabric v *vlastní Data* části níže. To zahrnuje kontext služby, abyste viděli *PartitionID* a *ReplicaId* zdroje požadavku a lepší lokalizaci problémy při diagnostikování chyb v aplikaci.

![Podrobnosti o trasování AI](./media/service-fabric-tutorial-monitoring-aspnet/trace-details.png)

Kromě toho, protože jsme zapnuta aplikace mapy, *přehled* stránky, kliknutím na **aplikace mapy** ikonu si ukážeme obě vaší služby připojení.

![Podrobnosti o trasování AI](./media/service-fabric-tutorial-monitoring-aspnet/app-map.png)

Mapa aplikace můžete vám pomůžou pochopit topologii aplikace lépe, zejména jako začnete přidávat několik různých služeb, které vzájemně spolupracují. Také poskytuje základní data na žádost úspěšnost a mohou pomoci diagnostikovat neúspěšné žádosti o zjistit, kde může být věcí mimo nesprávný. Další informace o používání mapy aplikace naleznete v tématu [mapy aplikací ve službě Application Insights](../application-insights/app-insights-app-map.md).

## <a name="add-custom-instrumentation-to-your-application"></a>Přidat vlastní instrumentace do vaší aplikace

I když Application Insights poskytuje mnoho telemetrie z pole, můžete přidat další vlastní instrumentaci. To může být založené na vašim obchodním potřebám nebo ke zlepšení diagnostiky v případě problémů v aplikaci. Application Insights obsahuje rozhraní API pro načítání vlastní události a metriky, které další informace o [zde](../application-insights/app-insights-api-custom-events-metrics.md).

Přidejme některých vlastních událostí k *VoteDataController.cs* (v části *VotingData* > *řadiče*) můžete sledovat, kdy bude hlasy přidat a odstranit z základní *votesDictionary*. 
1. Přidat `using Microsoft.ApplicationInsights;` na konci dalších příkazy using.
2. Deklarovat nové *TelemetryClient* na začátku třídy, v části Vytvoření *IReliableStateManager*: `private TelemetryClient telemetry = new TelemetryClient();`.
3. V *Put()* fungovat, přidejte byla přidána událost, která potvrdí hlas. Přidat `telemetry.TrackEvent($"Added a vote for {name}");` po dokončení transakce pravým před návratový *OkResult* příkaz.
4. V *Delete()*, je "if/else" základě za podmínky, že *votesDictionary* obsahuje hlasy pro danou hlasujících možnost. 
    1. Přidání události potvrzení odstranění hlas v *Pokud* prohlášení, po *await tx.CommitAsync()*:`telemetry.TrackEvent($"Deleted votes for {name}");`
    2. Přidat událost, která ukazují, že odstranění nedošlo *else* příkaz před příkaz return:`telemetry.TrackEvent($"Unable to delete votes for {name}, voting option not found");`

Tady je příklad co vaše *Put()* a *Delete()* funkce bude vypadat podobně jako po přidání události:

```csharp
// PUT api/VoteData/name
[HttpPut("{name}")]
public async Task<IActionResult> Put(string name)
{
    var votesDictionary = await this.stateManager.GetOrAddAsync<IReliableDictionary<string, int>>("counts");

    using (ITransaction tx = this.stateManager.CreateTransaction())
    {
        await votesDictionary.AddOrUpdateAsync(tx, name, 1, (key, oldvalue) => oldvalue + 1);
        await tx.CommitAsync();
    }

    telemetry.TrackEvent($"Added a vote for {name}");
    return new OkResult();
}

// DELETE api/VoteData/name
[HttpDelete("{name}")]
public async Task<IActionResult> Delete(string name)
{
    var votesDictionary = await this.stateManager.GetOrAddAsync<IReliableDictionary<string, int>>("counts");

    using (ITransaction tx = this.stateManager.CreateTransaction())
    {
        if (await votesDictionary.ContainsKeyAsync(tx, name))
        {
            await votesDictionary.TryRemoveAsync(tx, name);
            await tx.CommitAsync();
            telemetry.TrackEvent($"Deleted votes for {name}");
            return new OkResult();
        }
        else
        {
            telemetry.TrackEvent($"Unable to delete votes for {name}, voting option not found");
            return new NotFoundResult();
        }
    }
}
```

Jakmile jste hotovi provedení těchto změn **spustit** aplikace, které se vytvoří a nasadí nejnovější verzi. Jakmile se aplikace provádí nasazení, přejděte přes [localhost: 8080](localhost:8080)a přidat a odstranit některé hlasovací možnosti. Pak přejděte zpátky do zdroje Application Insights zobrazíte trasování pro nejnovější spustit (jak před, trasování může trvat 1 – 2 min objeví ve službě Application Insights). Pro všechny hlasování přidat a odstranit měli byste vidět "vlastní události * společně s všechny telemetrická data odpovědi. 

![vlastní události](./media/service-fabric-tutorial-monitoring-aspnet/custom-events.png)

## <a name="next-steps"></a>Další kroky
V tomto kurzu jste se naučili:
> [!div class="checklist"]
> * Konfigurace služby Application Insights pro aplikace
> * Shromažďovat telemetrická data odpovědi pro trasování komunikace na základě protokolu HTTP mezi službami
> * Použít funkci mapy aplikace ve službě Application Insights
> * Přidání vlastních událostí pomocí Application Insights API

Teď, když jste dokončili nastavení monitorování a Diagnostika pro vaši aplikaci ASP.NET, zkuste následující postup:
- [Prozkoumejte monitorování a Diagnostika v Service Fabric](service-fabric-diagnostics-overview.md)
- [Analýza události Service Fabric pomocí služby Application Insights](service-fabric-diagnostics-event-analysis-appinsights.md)
- Další informace o Application Insights, najdete v části [Application Insights dokumentace](https://docs.microsoft.com/azure/application-insights/)
