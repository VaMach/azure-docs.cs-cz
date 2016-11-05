---
title: Použití prostředí PowerShell k odesílání Azure Diagnostics Application Insights | Microsoft Docs
description: Automatizujte konfiguraci kanálu Azure Diagnostics pro službu Application Insights.
services: application-insights
documentationcenter: .net
author: sbtron
manager: douge

ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: get-started-article
ms.date: 11/17/2015
ms.author: awills

---
# Použití prostředí PowerShell k odesílání Azure Diagnostics Application Insights
[Microsoft Azure](https://azure.com) může být [konfigurované k odesílání Azure Diagnostics](app-insights-azure-diagnostics.md) do [Visual Studio Application Insights](app-insights-overview.md). Diagnostika se týká cloudových služeb Azure a virtuálních počítačů Azure. Doplňují telemetrii, kterou odesíláte z aplikace pomocí Application Insights SDK. Jako součást automatizace procesu vytváření nových prostředků v Azure můžete nakonfigurovat diagnostiku pomocí prostředí PowerShell.

## Povolit rozšíření diagnostiky jako součást nasazení cloudové služby
Rutina `New-AzureDeployment` obsahuje parametr `ExtensionConfiguration`, který přijímá pole konfigurace diagnostiky. Ty lze vytvořit pomocí rutiny `New-AzureServiceDiagnosticsExtensionConfig`. Příklad:

```ps

    $service_package = "CloudService.cspkg"
    $service_config = "ServiceConfiguration.Cloud.cscfg"
    $diagnostics_storagename = "myservicediagnostics"
    $webrole_diagconfigpath = "MyService.WebRole.PubConfig.xml" 
    $workerrole_diagconfigpath = "MyService.WorkerRole.PubConfig.xml"

    $primary_storagekey = (Get-AzureStorageKey `
     -StorageAccountName "$diagnostics_storagename").Primary
    $storage_context = New-AzureStorageContext `
       -StorageAccountName $diagnostics_storagename `
       -StorageAccountKey $primary_storagekey

    $webrole_diagconfig = `
     New-AzureServiceDiagnosticsExtensionConfig `
      -Role "WebRole" -Storage_context $storageContext `
      -DiagnosticsConfigurationPath $webrole_diagconfigpath
    $workerrole_diagconfig = `
     New-AzureServiceDiagnosticsExtensionConfig `
      -Role "WorkerRole" `
      -StorageContext $storage_context `
      -DiagnosticsConfigurationPath $workerrole_diagconfigpath

    New-AzureDeployment `
      -ServiceName $service_name `
      -Slot Production `
      -Package $service_package `
      -Configuration $service_config `
      -ExtensionConfiguration @($webrole_diagconfig,$workerrole_diagconfig)

``` 

## Povolit rozšíření diagnostiky na existující službu Cloud
Na existující službu použijte `Set-AzureServiceDiagnosticsExtension`.

```ps

    $service_name = "MyService"
    $diagnostics_storagename = "myservicediagnostics"
    $webrole_diagconfigpath = "MyService.WebRole.PubConfig.xml" 
    $workerrole_diagconfigpath = "MyService.WorkerRole.PubConfig.xml"
    $primary_storagekey = (Get-AzureStorageKey `
         -StorageAccountName "$diagnostics_storagename").Primary
    $storage_context = New-AzureStorageContext `
        -StorageAccountName $diagnostics_storagename `
        -StorageAccountKey $primary_storagekey

    Set-AzureServiceDiagnosticsExtension `
        -StorageContext $storage_context `
        -DiagnosticsConfigurationPath $webrole_diagconfigpath `
        -ServiceName $service_name `
        -Slot Production `
        -Role "WebRole" 
    Set-AzureServiceDiagnosticsExtension `
        -StorageContext $storage_context `
        -DiagnosticsConfigurationPath $workerrole_diagconfigpath `
        -ServiceName $service_name `
        -Slot Production `
        -Role "WorkerRole"
```

## Získejte aktuální konfiguraci rozšíření diagnostiky
```ps

    Get-AzureServiceDiagnosticsExtension -ServiceName "MyService"
```


## Odeberte rozšíření diagnostiky
```ps

    Remove-AzureServiceDiagnosticsExtension -ServiceName "MyService"
```

Pokud jste povolili rozšíření diagnostiky pomocí `Set-AzureServiceDiagnosticsExtension` nebo `New-AzureServiceDiagnosticsExtensionConfig` bez parametru Role můžete rozšíření odebrat pomocí `Remove-AzureServiceDiagnosticsExtension` bez parametru Role. Pokud byl použit parametr role při povolování rozšíření, pak musí být rovněž použit při odebírání rozšíření.

Chcete-li odebrat rozšíření diagnostiky pro každou jednotlivou roli:

```ps

    Remove-AzureServiceDiagnosticsExtension -ServiceName "MyService" -Role "WebRole"
```


## Viz také
* [Monitorování aplikací Azure Cloud Services pomocí nástroje Application Insights](app-insights-cloudservices.md)
* [Odesílání Azure Diagnostics do Application Insights](app-insights-azure-diagnostics.md)
* [Automatizace konfigurace výstrah](app-insights-powershell-alerts.md)

<!--HONumber=Sep16_HO3-->


