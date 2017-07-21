---
title: "Vytvoření vývojového prostředí pro mikroslužby Azure | Dokumentace Microsoftu"
description: "Nainstalujte modul runtime, sadu SDK a nástroje a vytvořte místní vývojový cluster. Po dokončení této instalace a nastavení budete moci sestavovat aplikace."
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: 
ms.assetid: b94e2d2e-435c-474a-ae34-4adecd0e6f8f
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/20/2017
ms.author: ryanwi, mikhegn
ms.translationtype: Human Translation
ms.sourcegitcommit: f7479260c7c2e10f242b6d8e77170d4abe8634ac
ms.openlocfilehash: 926dfe3de0715f855e6d5b57f10c2366cda8583b
ms.contentlocale: cs-cz
ms.lasthandoff: 06/21/2017


---
# Příprava vývojového prostředí
<a id="prepare-your-development-environment" class="xliff"></a>
> [!div class="op_single_selector"]
> * [Windows](service-fabric-get-started.md) 
> * [Linux](service-fabric-get-started-linux.md)
> * [OSX](service-fabric-get-started-mac.md)
> 
> 

 Pokud chcete sestavovat a spouštět [aplikace Azure Service Fabric][1] na vývojovém počítači, musíte nainstalovat modul runtime, sadu SDK a nástroje. Musíte taky povolit spouštění skriptů prostředí Windows PowerShell, které jsou součástí sady SDK.

## Požadavky
<a id="prerequisites" class="xliff"></a>
### Podporované verze operačních systémů
<a id="supported-operating-system-versions" class="xliff"></a>
Pro vývoj jsou podporovány tyto verze operačních systémů:

* Windows 7
* Windows 8 / Windows 8.1
* Windows Server 2012 R2
* Windows Server 2016
* Windows 10

> [!NOTE]
> Windows 7 ve výchozím nastavení obsahuje jenom prostředí Windows PowerShell 2.0 Rutiny prostředí PowerShell pro Service Fabric vyžadují PowerShell 3.0 nebo novější. Můžete si [stáhnout prostředí Windows PowerShell 5.0][powershell5-download] z webu Microsoft Download Center.
> 
> 

## Instalace sady SDK a nástrojů
<a id="install-the-sdk-and-tools" class="xliff"></a>
### Použití sady Visual Studio 2017
<a id="to-use-visual-studio-2017" class="xliff"></a>
Nástroje Service Fabric jsou součástí úlohy Azure Development and Management v sadě Visual Studio 2017. Povolte tuto úlohu jako součást instalace sady Visual Studio.
Kromě toho budete muset sadu Microsoft Azure Service Fabric SDK nainstalovat pomocí instalačního programu webové platformy.

* [Instalace sady Microsoft Azure Service Fabric SDK][core-sdk]

### Použití sady Visual Studio 2015 (vyžaduje Visual Studio 2015 Update 2 nebo novější)
<a id="to-use-visual-studio-2015-requires-visual-studio-2015-update-2-or-later" class="xliff"></a>
Pro sadu Visual Studio 2015 jsou nainstalované nástroje Service Fabric společně se sadou SDK, pomocí Instalace webové platformy:

* [Instalace sady Microsoft Azure Service Fabric SDK a nástrojů][full-bundle-vs2015]

### Jenom instalace sady SDK
<a id="sdk-installation-only" class="xliff"></a>
Pokud potřebujete jenom sadu SDK, můžete nainstalovat tento balíček:
* [Instalace sady Microsoft Azure Service Fabric SDK][core-sdk]

Aktuální verze jsou:
* Sada Service Fabric SDK 2.6.220
* Modul runtime Service Fabric 5.6.220
* Nástroje sady Visual Studio 2015 1.6.50508.2
* Visual Studio 2017 s aktualizací Update 2

Aktuální verze Preview jsou:
* Sada Service Fabric SDK 255.255.2718.255
* Modul runtime Service Fabric 255.255.5718.255
* Nástroje sady Visual Studio 2015 1.6.50509.5
* Visual Studio 2017 s aktualizací Update 3 ve verzi Preview 1

Seznam podporovaných verzí najdete v tématu [Podpora pro Service Fabric](service-fabric-support.md)

## Povolení spouštění skriptů prostředí PowerShell
<a id="enable-powershell-script-execution" class="xliff"></a>
Platforma Service Fabric používá skripty prostředí Windows PowerShell k vytvoření místního vývojového clusteru a k nasazení aplikací ze sady Visual Studio. Systém Windows ve výchozím nastavení spouštění těchto skriptů blokuje. Pokud je chcete povolit, musíte upravit zásady spouštění prostředí PowerShell. Otevřete prostředí PowerShell jako správce a zadejte tento příkaz:

```powershell
Set-ExecutionPolicy -ExecutionPolicy Unrestricted -Force -Scope CurrentUser
```

## Další kroky
<a id="next-steps" class="xliff"></a>
Teď, když jste dokončili nastavení vývojového prostředí, můžete začít sestavovat a spouštět aplikace.

* [Vytvořte první aplikaci Service Fabric v sadě Visual Studio](service-fabric-create-your-first-application-in-visual-studio.md)
* [Naučte se nasadit a spravovat aplikace v místním clusteru](service-fabric-get-started-with-a-local-cluster.md)
* [Seznamte se s programovacími modely: Reliable Services a Reliable Actors](service-fabric-choose-framework.md)
* [Prohlédněte si ukázky kódu Service Fabric na GitHubu](https://aka.ms/servicefabricsamples)
* [Vizualizujte cluster pomocí Service Fabric Exploreru](service-fabric-visualizing-your-cluster.md)
* [Postupujte podle studijního plánu Service Fabric a získejte obecný úvod k platformě](https://azure.microsoft.com/documentation/learning-paths/service-fabric/)
* Informace o [možnostech podpory pro Service Fabric](service-fabric-support.md)

[1]: http://azure.microsoft.com/en-us/campaigns/service-fabric/ "Stránka kampaně Service Fabric"
[2]: http://go.microsoft.com/fwlink/?LinkId=517106 "VS RC"
[full-bundle-vs2015]:http://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-VS2015 "Odkaz na VS 2015 WebPI"
[full-bundle-dev15]:http://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-Dev15 "Odkaz na Dev15 WebPI"
[core-sdk]:http://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-CoreSDK "Odkaz na Core SDK WebPI"
[powershell5-download]:https://www.microsoft.com/en-us/download/details.aspx?id=50395

