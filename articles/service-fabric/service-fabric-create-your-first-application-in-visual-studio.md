---
title: "Vytvoření spolehlivé služby Azure Service Fabric pomocí jazyka C#"
description: "Vytvoření, nasazení a ladění aplikace spolehlivé služby postavené na Azure Service Fabric pomocí sady Visual Studio."
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: vturecek
ms.assetid: c3655b7b-de78-4eac-99eb-012f8e042109
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: hero-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 01/19/2018
ms.author: ryanwi
ms.openlocfilehash: 2ecb8f8068043936d00f2c9752666490137414e3
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/01/2018
---
# <a name="create-your-first-c-service-fabric-stateful-reliable-services-application"></a>Vytvoření první aplikace spolehlivé stavové služby Service Fabric v jazyce C#

Zjistěte, jak nasadit první aplikaci Service Fabric pro .NET v systému Windows během několika minut. Jakmile budete hotovi, budete mít funkční místní cluster s aplikací spolehlivé služby.

## <a name="prerequisites"></a>Požadavky

Než začnete, ujistěte se, že máte [nastavené vývojové prostředí](service-fabric-get-started.md). To zahrnuje instalaci sady Service Fabric SDK a sady Visual Studio 2017 nebo 2015.

## <a name="create-the-application"></a>Vytvoření aplikace

Spusťte sadu Visual Studio jako **správce**.

Vytvořte projekt pomocí klávesové zkratky `CTRL`+`SHIFT`+`N`.

V dialogovém okně **Nový projekt** zvolte **Cloud > Aplikace Service Fabric**.

Pojmenujte aplikaci **MyApplication** a stiskněte **OK**.

   
![Dialogové okno Nový projekt ve Visual Studiu][1]

V dalším dialogovém okně můžete vytvořit jakýkoli typ aplikace Service Fabric. Pro účely tohoto Rychlého startu zvolte **Stavová služba**.

Pojmenujte službu **MyStatefulService** a stiskněte **OK**.

![Dialogové okno Nová služba ve Visual Studiu][2]


Visual Studio vytvoří projekt aplikace a projekt stavové služby a zobrazí je v Průzkumníku řešení.

![Průzkumník řešení po vytvoření aplikace se stavovou službou][3]

Projekt aplikace (**MyApplication**) jako takový neobsahuje žádný kód. Odkazuje ale na sadu projektů služeb. Kromě toho obsahuje další tři typy obsahu:

* **Profily publikování**  
Profily pro nasazování do různých prostředí.

* **Skripty**  
Skript PowerShellu pro nasazení/upgrade aplikace.

* **Definice aplikace**  
Zahrnuje soubor ApplicationManifest.xml ve složce *ApplicationPackageRoot*, který popisuje složení aplikace. Ve složce *ApplicationParameters* se nachází související soubory parametrů aplikace, které můžete použít k zadání parametrů specifických pro prostředí. Sada Visual Studio během nasazení do konkrétního prostředí vybere soubor parametrů aplikace, který je zadaný v přidruženém profilu publikování.
    
Přehled obsahu projektu služby najdete v tématu [Začínáme se službami Reliable Services](service-fabric-reliable-services-quick-start.md).

## <a name="deploy-and-debug-the-application"></a>Nasazení a ladění aplikace

Teď, když máte aplikaci, ji spusťte.

Stisknutím klávesy `F5` v sadě Visual Studio aplikaci nasaďte pro účely ladění.

>[!NOTE]
>Při prvním místním spuštění a nasazení aplikace sada Visual Studio vytvoří místní cluster pro účely ladění. To může nějakou dobu trvat. Stav vytváření clusteru se zobrazí v okně výstupu sady Visual Studio.

Až bude cluster připravený, aplikace pro správu na hlavním panelu systému místního clusteru, která je součástí sady SDK, zobrazí upozornění.
   
![Upozornění na hlavním panelu systému místního clusteru][4]

Až se aplikace spustí, Visual Studio automaticky zobrazí **Prohlížeč diagnostických událostí**, ve kterém uvidíte výstup trasování ze služeb.
   
![Prohlížeč diagnostických událostí][5]

>[!NOTE]
>Události by měly automatiky spustit sledování v prohlížeči diagnostických událostí, ale pokud ho budete muset nakonfigurovat ručně, nejdřív otevřete soubor `ServiceEventSource.cs` umístěný v projektu **MyStatefulService**. Zkopírujte hodnotu atributu `EventSource` v horní části třídy `ServiceEventSource`. V příkladu níže má zdroj událostí název `"MyCompany-MyApplication-MyStatefulService"`, který ve vaší situaci může být jiný.
>
>![Vyhledání názvu zdroje události služby][service-event-source-name]
>
>Potom kliknutím na ikonu ozubeného kola umístěnou na kartě Prohlížeč diagnostických událostí otevřete dialogové okno **Zprostředkovatelé Trasování událostí pro Windows**. Vložte právě zkopírovaný název zdroje událostí do vstupního pole **Zprostředkovatelé Trasování událostí pro Windows**. Potom klikněte na tlačítko **Použít**. Tím automaticky spustí trasování událostí.
>
>![Nastavení názvu zdroje diagnostických událostí][setting-event-source-name]
>
>Nyní by se události měly zobrazovat v okně Diagnostické události.

Použitá šablona stavové služby uvádí hodnotu čítače, která se zvyšuje v metodě `RunAsync` v souboru **MyStatefulService.cs**.

Pokud některou událost rozbalíte, zobrazí se další podrobnosti, včetně uzlu, na kterém kód běží. V tomto případě je to uzel \_Node\_0, to se ale může na vašem počítači lišit.
   
![Podrobnosti v prohlížeči diagnostických událostí][6]

Místní cluster obsahuje pět uzlů hostovaných na jednom počítači. V produkčním prostředí je každý uzel hostovaný na odlišném fyzickém nebo virtuálním počítači. Abychom napodobili situaci, kdy počítač přestane pracovat, zatímco je na něm spuštěn ladící program sady Visual Studio, zkusme teď jeden z uzlů místního clusteru zastavit.

V okně **Průzkumník řešení** otevřete soubor **MyStatefulService.cs**. 

Vyhledejte metodu `RunAsync` a na prvním řádku metody nastavte zarážku.

![Zarážka v metodě RunAsync stavové služby][7]

Spusťte nástroj **Service Fabric Explorer** kliknutím pravým tlačítkem na aplikaci **Local Cluster Manager** na hlavním panelu systému a zvolením **Manage Local Cluster** (Správa místního clusteru).

![Spuštění nástroje Service Fabric Explorer z aplikace Local Cluster Manager][systray-launch-sfx]

[**Service Fabric Explorer**](service-fabric-visualizing-your-cluster.md) nabízí vizuální znázornění clusteru. To zahrnujte sadu aplikací, které jsou v něm nasazené, a sadu fyzických uzlů, které ho tvoří.

V levém podokně rozbalte položky **Cluster > Uzly** a najděte uzel, na kterém běží váš kód.

Kliknutím na **Akce > Deaktivovat (restartovat)** simulujte restartování počítače.

![Zastavení uzlu v Service Fabric Exploreru][sfx-stop-node]

Na okamžik byste měli zahlédnout, jak Visual Studio dojde k vaší zarážce a výpočet probíhající na jednom uzlu plynule převezme jiný uzel.


Dále se vraťte do prohlížeče diagnostických událostí a podívejte se na zprávy. Hodnota čítače pořád roste, i když události ve skutečnosti přicházejí z jiného uzlu.

![Prohlížeč diagnostických událostí po převzetí služeb při selhání][diagnostic-events-viewer-detail-post-failover]

## <a name="cleaning-up-the-local-cluster-optional"></a>Vyčištění místního clusteru (volitelné)

Pamatujte, že tento místní cluster je skutečný. Při zastavení ladicího programu se odebere instance aplikace a zruší se registrace typu aplikace. Cluster ale dál běží na pozadí. Až budete připraveni zastavit místní cluster, máte několik možností.

### <a name="keep-application-and-trace-data"></a>Zachování dat aplikace a trasování

Vypněte cluster kliknutím pravým tlačítkem na aplikaci **Local Cluster Manager** na hlavním panelu systému a zvolením **Stop Local Cluster** (Zastavit místní cluster).

### <a name="delete-the-cluster-and-all-data"></a>Odstranění clusteru a veškerých dat

Odeberte cluster kliknutím pravým tlačítkem na aplikaci **Local Cluster Manager** na hlavním panelu systému a zvolením **Remove Local Cluster** (Odebrat místní cluster). 

Pokud zvolíte tuto možnost, sada Visual Studio cluster znovu nasadí při příštím spuštění aplikace. Zvolte tuto možnost v případě, že se místní cluster nechystáte nějakou dobu používat nebo potřebujete uvolnit prostředky.

## <a name="next-steps"></a>Další kroky
Další informace o [spolehlivých službách](service-fabric-reliable-services-introduction.md).
<!-- Image References -->

[1]: ./media/service-fabric-create-your-first-application-in-visual-studio/new-project-dialog.png
[2]: ./media/service-fabric-create-your-first-application-in-visual-studio/new-project-dialog-2.png
[3]: ./media/service-fabric-create-your-first-application-in-visual-studio/solution-explorer-stateful-service-template.png
[4]: ./media/service-fabric-create-your-first-application-in-visual-studio/local-cluster-manager-notification.png
[5]: ./media/service-fabric-create-your-first-application-in-visual-studio/diagnostic-events-viewer.png
[6]: ./media/service-fabric-create-your-first-application-in-visual-studio/diagnostic-events-viewer-detail.png
[7]: ./media/service-fabric-create-your-first-application-in-visual-studio/runasync-breakpoint.png
[sfx-stop-node]: ./media/service-fabric-create-your-first-application-in-visual-studio/sfe-deactivate-node.png
[systray-launch-sfx]: ./media/service-fabric-create-your-first-application-in-visual-studio/launch-sfx.png
[diagnostic-events-viewer-detail-post-failover]: ./media/service-fabric-create-your-first-application-in-visual-studio/diagnostic-events-viewer-detail-post-failover.png
[sfe-delete-application]: ./media/service-fabric-create-your-first-application-in-visual-studio/sfe-delete-application.png
[switch-cluster-mode]: ./media/service-fabric-create-your-first-application-in-visual-studio/switch-cluster-mode.png
[cluster-setup-success-1-node]: ./media/service-fabric-get-started-with-a-local-cluster/cluster-setup-success-1-node.png
[service-event-source-name]: ./media/service-fabric-create-your-first-application-in-visual-studio/event-source-attribute-value.png
[setting-event-source-name]: ./media/service-fabric-create-your-first-application-in-visual-studio/setting-event-source-name.png
