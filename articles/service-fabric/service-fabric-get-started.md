---
title: Nastavení vývojového prostředí | Microsoft Docs
description: Nainstalujte modul runtime, sadu SDK a nástroje a vytvořte místní vývojový cluster. Po dokončení této instalace a nastavení budete moci sestavovat aplikace.
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: ''

ms.service: service-fabric
ms.devlang: dotNet
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 09/13/2016
ms.author: ryanwi

---
# Příprava vývojového prostředí
> [!div class="op_single_selector"]
> -[ Windows](service-fabric-get-started.md)
> 
> * [Linux](service-fabric-get-started-linux.md)
> * [OSX](service-fabric-get-started-mac.md)
> 
> 

 Pokud chcete sestavovat a spouštět [aplikace Azure Service Fabric][1] na vývojovém počítači, musíte nainstalovat modul runtime, sadu SDK a nástroje. Musíte taky povolit spouštění skriptů prostředí Windows PowerShell, které jsou součástí sady SDK.

## Požadavky
### Podporované verze operačních systémů
Pro vývoj jsou podporovány tyto verze operačních systémů:

* Windows 7
* Windows 8 / Windows 8.1
* Windows Server 2012 R2
* Windows 10

> [!NOTE]
> Windows 7 ve výchozím nastavení obsahuje jenom prostředí Windows PowerShell 2.0 Rutiny prostředí PowerShell pro Service Fabric vyžadují PowerShell 3.0 nebo novější. Můžete [stáhnout prostředí Windows PowerShell 5.0][powershell5-stažení] z webu Microsoft Download Center.
> 
> 

## Instalace modulu runtime, sady SDK a nástrojů
Instalace webové platformy nabízí dvě konfigurace pro vývoj Service Fabric:

* [Instalace modulu runtime Service Fabric, sady SDK a nástrojů pro sadu Visual Studio 2015 (je vyžadována verze Visual Studio 2015 Update 2 nebo novější)][full-bundle-vs2015]
* [Instalace jenom modulu runtime Service Fabric a sady SDK (bez nástrojů pro Visual Studio)][core-sdk]

## Povolení spouštění skriptů prostředí PowerShell
Platforma Service Fabric používá skripty prostředí Windows PowerShell k vytvoření místního vývojového clusteru a k nasazení aplikací ze sady Visual Studio. Systém Windows ve výchozím nastavení spouštění těchto skriptů blokuje. Pokud je chcete povolit, musíte upravit zásady spouštění prostředí PowerShell. Otevřete prostředí PowerShell jako správce a zadejte tento příkaz:

```powershell
Set-ExecutionPolicy -ExecutionPolicy Unrestricted -Force -Scope CurrentUser
```

## Další kroky
Teď, když jste dokončili nastavení vývojového prostředí, můžete začít sestavovat a spouštět aplikace.

* [Vytvořte první aplikaci Service Fabric v sadě Visual Studio](service-fabric-create-your-first-application-in-visual-studio.md)
* [Naučte se nasadit a spravovat aplikace v místním clusteru](service-fabric-get-started-with-a-local-cluster.md)
* [Seznamte se s programovacími modely: Reliable Services a Reliable Actors](service-fabric-choose-framework.md)
* [Prohlédněte si ukázky kódu Service Fabric na GitHubu](https://aka.ms/servicefabricsamples)
* [Vizualizujte cluster pomocí Service Fabric Exploreru](service-fabric-visualizing-your-cluster.md)
* [Postupujte podle studijního plánu Service Fabric a získejte obecný úvod k platformě](https://azure.microsoft.com/documentation/learning-paths/service-fabric/)

[1]: http://azure.microsoft.com/en-us/campaigns/service-fabric/ "Stránka kampaně Service Fabric"
[2]: http://go.microsoft.com/fwlink/?LinkId=517106 "VS RC"
[full-bundle-vs2015]:http://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-VS2015 "odkaz na VS 2015 WebPI"
[full-bundle-dev15]:http://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-Dev15 "odkaz na Dev15 WebPI"
[core-sdk]:http://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-CoreSDK "odkaz na Core SDK WebPI"
[powershell5-stažení]:https://www.microsoft.com/en-us/download/details.aspx?id=50395



<!--HONumber=Sep16_HO4-->


