---
title: Povolit profileru Application Insights pro aplikace Azure Compute | Microsoft Docs
description: "Zjistěte, jak nastavit profileru Application Insights na aplikace běžící v Azure Compute."
services: application-insights
documentationcenter: 
author: ramach-msft
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 10/16/2017
ms.author: ramach
ms.openlocfilehash: 57a4cb560825e0c05ac49df26ac12ee52da52c3c
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/08/2017
---
# <a name="enable-application-insights-profiler-for-azure-vms-service-fabric-and-cloud-services"></a>Povolit Application Insights profileru pro virtuální počítače Azure, Service Fabric a cloudové služby

Tento článek ukazuje, jak povolit Azure Application Insights profileru na aplikace ASP.NET, který je hostitelem prostředek výpočtů Azure. 

V příkladech v tomto článku zahrnují podporu pro virtuální počítače Azure, sady škálování virtuálního počítače, Azure Service Fabric a cloudových služeb Azure. Příklady spoléhají na šablony, které podporují [Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) modelu nasazení.  


## <a name="overview"></a>Přehled

Následující obrázek ukazuje, jak profileru Application Insights funguje s prostředky Azure. Bitovou kopii virtuálního počítače Azure používá jako příklad.

  ![Přehled](./media/enable-profiler-compute/overview.png)

Chcete-li plně profileru, musíte změnit konfiguraci ve třech umístěních:

* V podokně instance Application Insights na portálu Azure.
* Zdrojový kód aplikace (například webovou aplikaci ASP.NET).
* Prostředí nasazení definice zdrojový kód (například počítač nasazení .json souboru šablony).


## <a name="set-up-the-application-insights-instance"></a>Nastavení instance služby Application Insights

Na portálu Azure vytvořit nebo na instanci služby Application Insights, která chcete použít. Všimněte si klíč instrumentace instance. Klíč instrumentace použijete v další kroky konfigurace.

  ![Umístění klíč instrumentace](./media/enable-profiler-compute/CopyAIKey.png)

Tato instance musí být stejná jako aplikace. Je nakonfigurován pro odesílat telemetrická data, aby na každý požadavek.
Profileru výsledky jsou také k dispozici v této instanci.  

Na portálu Azure, proveďte kroky popsané v [povolit profileru](https://docs.microsoft.com/azure/application-insights/app-insights-profiler#enable-the-profiler) a dokončete nastavení instance služby Application Insights pro profileru. Nemusíte odkaz webové aplikace, aby v příkladu v tomto článku. Právě Ujistěte se, že profileru je povolena na portálu.


## <a name="set-up-the-application-source-code"></a>Nastavit zdrojovému kódu aplikace

Nastavení vaší aplikace odesílat telemetrická data do instance Application Insights na každém `Request` operace:  

1. Přidat [Application Insights SDK](https://docs.microsoft.com/azure/application-insights/app-insights-overview#get-started) na projekt aplikace. Ujistěte se, že verze balíčku NuGet jsou následující:  
  - Pro aplikace ASP.NET: [Microsoft.ApplicationInsights.Web](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Web/) 2.3.0 nebo novější.
  - Pro aplikace ASP.NET Core: [Microsoft.ApplicationInsights.AspNetCore](https://www.nuget.org/packages/Microsoft.ApplicationInsights.AspNetCore/) 2.1.0 nebo novější.
  - U ostatních aplikací .NET a .NET Core (například bezstavové služby Service Fabric nebo roli pracovního procesu cloudové služby): [Microsoft.ApplicationInsights](https://www.nuget.org/packages/Microsoft.ApplicationInsights/) nebo [Microsoft.ApplicationInsights.Web](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Web/) 2.3.0 nebo novější.  

2. Pokud je vaše aplikace *není* aplikace ASP.NET nebo ASP.NET Core (např. je-li role pracovního procesu cloudové služby nebo Service Fabric bezstavové rozhraní API), je požadované nastavení následující další instrumentace:  

  1. Přidejte následující kód v rané fázi životního cyklu aplikace:  

    ```csharp
    using Microsoft.ApplicationInsights.Extensibility;
    ...
    // Replace with your own Application Insights instrumentation key.
    TelemetryConfiguration.Active.InstrumentationKey = "00000000-0000-0000-0000-000000000000";
    ```
  Další informace o této globální instrumentace klíče konfiguraci najdete v tématu [pomocí Service Fabric pomocí služby Application Insights](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started/blob/dev/appinsights/ApplicationInsights.md).  

  2. Pro jakékoliv kód, který chcete instrumentace, přidejte `StartOperation<RequestTelemetry>` **pomocí** příkaz kolem něj, jako v následujícím příkladu:

    ```csharp
    using Microsoft.ApplicationInsights;
    using Microsoft.ApplicationInsights.DataContracts;
    ...
    var client = new TelemetryClient();
    ...
    using (var operation = client.StartOperation<RequestTelemetry>("Insert_Your_Custom_Event_Unique_Name"))
    {
      // ... Code I want to profile.
    }
    ```

  Volání metody `StartOperation<RequestTelemetry>` v rámci jiného `StartOperation<RequestTelemetry>` oboru není podporován. Můžete použít `StartOperation<DependencyTelemetry>` v vnořeného obor místo. Například:  

    ```csharp
    using (var getDetailsOperation = client.StartOperation<RequestTelemetry>("GetProductDetails"))
    {
      try
      {
        ProductDetail details = new ProductDetail() { Id = productId };
        getDetailsOperation.Telemetry.Properties["ProductId"] = productId.ToString();

        // By using DependencyTelemetry, 'GetProductPrice' is correctly linked as part of the 'GetProductDetails' request.
        using (var getPriceOperation = client.StartOperation<DependencyTelemetry>("GetProductPrice"))
        {
            double price = await _priceDataBase.GetAsync(productId);
            if (IsTooCheap(price))
            {
                throw new PriceTooLowException(productId);
            }
            details.Price = price;
        }

        // Similarly, note how 'GetProductReviews' doesn't establish another RequestTelemetry.
        using (var getReviewsOperation = client.StartOperation<DependencyTelemetry>("GetProductReviews"))
        {
            details.Reviews = await _reviewDataBase.GetAsync(productId);
        }

        getDetailsOperation.Telemetry.Success = true;
        return details;
      }
      catch(Exception ex)
      {
        getDetailsOperation.Telemetry.Success = false;

        // This exception gets linked to the 'GetProductDetails' request telemetry.
        client.TrackException(ex);
        throw;
      }
    }
    ```


## <a name="set-up-the-environment-deployment-definition"></a>Nastavit definice nasazení prostředí

Prostředí, ve kterém může být profileru a vaše aplikace spuštění virtuálního počítače, škálovací sadu virtuálních počítačů, cluster Service Fabric nebo instance cloudové služby.  

### <a name="virtual-machines-virtual-machine-scale-sets-or-service-fabric"></a>Virtuální počítače, sady škálování virtuálního počítače nebo Service Fabric

Úplné příklady:  
  * [Virtuální počítač](https://github.com/Azure/azure-docs-json-samples/blob/master/application-insights/WindowsVirtualMachine.json)
  * [Škálovací sadu virtuálních počítačů](https://github.com/Azure/azure-docs-json-samples/blob/master/application-insights/WindowsVirtualMachineScaleSet.json)
  * [Cluster Service Fabric](https://github.com/Azure/azure-docs-json-samples/blob/master/application-insights/ServiceFabricCluster.json)

1. Zajistit, aby [rozhraní .NET Framework 4.6.1](https://docs.microsoft.com/dotnet/framework/migration-guide/how-to-determine-which-versions-are-installed) nebo novějším, je používán, je dostačující k potvrzení, že je nasazený operační systém `Windows Server 2012 R2` nebo novější.

2. Vyhledejte [Azure Diagnostics](https://docs.microsoft.com/azure/monitoring-and-diagnostics/azure-diagnostics) rozšíření v šabloně nasazení souboru a potom přidejte následující `SinksConfig` části jako podřízeného prvku `WadCfg`. Nahraďte `ApplicationInsightsProfiler` hodnotu vlastnosti s vlastní klíč instrumentace Application Insights:  
  ```json
  "SinksConfig": {
    "Sink": [
      {
        "name": "MyApplicationInsightsProfilerSink",
        "ApplicationInsightsProfiler": "00000000-0000-0000-0000-000000000000"
      }
    ]
  }
  ```

  Informace o přidání rozšíření diagnostiky do šablony nasazení najdete v tématu [použití monitorování a Diagnostika pomocí šablony virtuálního počítače s Windows a Azure Resource Manager](https://docs.microsoft.com/azure/virtual-machines/windows/extensions-diagnostics-template?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).


### <a name="cloud-services"></a>Cloud Services

1. Zajistit, aby [rozhraní .NET Framework 4.6.1](https://docs.microsoft.com/dotnet/framework/migration-guide/how-to-determine-which-versions-are-installed) nebo novějším, je v použití, stačí k potvrzení, že objekt ServiceConfiguration.\*. soubory cscfg `osFamily` hodnotu **"5"** nebo novější.

2. Vyhledejte [Azure Diagnostics](https://docs.microsoft.com/azure/monitoring-and-diagnostics/azure-diagnostics) diagnostics.wadcfgx soubor pro aplikační role:  
  ![Umístění souboru konfigurace diagnostiky](./media/enable-profiler-compute/cloudservice-solutionexplorer.png)  
  Pokud nemůže najít soubor, se dozvíte, jak povolit rozšíření diagnostiky v projektu cloudové služby, přečtěte si téma [nastavení diagnostiky pro Azure Cloud Services a virtuálních počítačů](https://docs.microsoft.com/azure/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines#enable-diagnostics-in-cloud-service-projects-before-deploying-them).

3. Přidejte následující `SinksConfig` části jako podřízeného prvku `WadCfg`:  
  ```xml
  <WadCfg>
    <DiagnosticMonitorConfiguration>...</DiagnosticMonitorConfiguration>
    <SinksConfig>
      <Sink name="MyApplicationInsightsProfiler">
        <!-- Replace with your own Application Insights instrumentation key. -->
        <ApplicationInsightsProfiler>00000000-0000-0000-0000-000000000000</ApplicationInsightsProfiler>
      </Sink>
    </SinksConfig>
  </WadCfg>
  ```

> [!NOTE]  
> Pokud `diagnostics.wadcfgx` soubor zároveň obsahuje jiné podřízený typu `ApplicationInsights`, všechny tři z těchto klíčů instrumentace musí odpovídat:  
>  * Klíč instrumentace, který používá vaše aplikace.  
>  * Používá klíč instrumentace `ApplicationInsights` jímky.  
>  * Používá klíč instrumentace `ApplicationInsightsProfiler` jímky.  
>
> Hodnota skutečného instrumentace klíče používané můžete najít `ApplicationInsights` jímky v objekt ServiceConfiguration.\*. cscfg soubory.  
> Po vydání sady Visual Studio 15,5 Azure SDK pouze klíčů instrumentace používá aplikace a `ApplicationInsightsProfiler` potřeba podřízený vzájemně odpovídaly.


## <a name="environment-deployment-and-runtime-configurations"></a>Konfigurace nasazení a modulu runtime prostředí

1. Definice nasazení upravené prostředí nasaďte.  

  Chcete-li použít změny, obvykle kompletní šablonou nasazení nebo cloudové služby publikování pomocí rutin prostředí PowerShell nebo sady Visual Studio je zahrnuta.  

  Toto je alternativní postup pro existující virtuální počítače, který dotykem jenom jeho rozšíření diagnostiky Azure:  
  ```powershell
  $ConfigFilePath = [IO.Path]::GetTempFileName()
  # After you export the currently deployed Diagnostics config to a file, edit it to include the ApplicationInsightsProfiler sink.
  (Get-AzureRmVMDiagnosticsExtension -ResourceGroupName "MyRG" -VMName "MyVM").PublicSettings | Out-File -Verbose $ConfigFilePath
  # Set-AzureRmVMDiagnosticsExtension might require the -StorageAccountName argument
  # if your original diagnostics configuration had the storageAccountName property in the protectedSettings section
  # (which is not downloadable). Make sure to pass the same original value you had in this cmdlet call.
  Set-AzureRmVMDiagnosticsExtension -ResourceGroupName "MyRG" -VMName "MyVM" -DiagnosticsConfigurationPath $ConfigFilePath
  ```

2. Pokud příslušné aplikace běží [IIS](https://www.microsoft.com/web/platform/server.aspx), povolte `IIS Http Tracing` funkce systému Windows:  
  
  1. Vytvoření vzdáleného přístupu v prostředí a potom pomocí [přidat funkce Windows]( https://docs.microsoft.com/iis/configuration/system.webserver/tracing/) okna nebo spusťte následující příkaz v prostředí PowerShell (jako správce):  
    ```powershell
    Enable-WindowsOptionalFeature -FeatureName IIS-HttpTracing -Online -All
    ```  
  2. Pokud navazování vzdáleného přístupu k potížím, můžete použít [rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli) spusťte následující příkaz:  
    ```powershell
    az vm run-command invoke -g MyResourceGroupName -n MyVirtualMachineName --command-id RunPowerShellScript --scripts "Enable-WindowsOptionalFeature -FeatureName IIS-HttpTracing -Online -All"
    ```


## <a name="enable-the-profiler-on-on-premises-servers"></a>Povolit profileru na místní servery

Povolení profileru na místním serveru se taky říká spuštěné profileru Statistika aplikace v samostatném režimu (není vázána k Azure Diagnostics rozšíření změny). 

Máme žádný plán oficiálně podporovalo profileru pro místní servery. Pokud vás zajímá experimentování se tento scénář, můžete [stáhnout kód podpory](https://github.com/ramach-msft/AIProfiler-Standalone). Snažíme se *není* zodpovědná za údržbu tento kód a reagovat na problémy a žádosti o funkce související s kód.

## <a name="next-steps"></a>Další kroky

- Generovat provoz vaší aplikace (například spuštění [test dostupnosti](https://docs.microsoft.com/azure/application-insights/app-insights-monitor-web-app-availability)). Potom počkejte 10 až 15 minut pro trasování k odeslání do instance Application Insights.
- V tématu [profileru trasování](https://docs.microsoft.com/azure/application-insights/app-insights-profiler#enable-the-profiler) na portálu Azure.
- Zobrazit nápovědu k řešení potíží s profileru v [profileru řešení potíží s](app-insights-profiler.md#troubleshooting).
- Další informace o profileru v [Application Insights profileru](app-insights-profiler.md).
