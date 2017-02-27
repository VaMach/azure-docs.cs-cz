---
title: "Vytvoření vaší první aplikace mikroslužeb Azure | Dokumentace Microsoftu"
description: "Vytvoření, nasazení a ladění aplikace Service Fabric pomocí Visual Studia"
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: 
ms.assetid: c3655b7b-de78-4eac-99eb-012f8e042109
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: hero-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 12/14/2016
ms.author: ryanwi
translationtype: Human Translation
ms.sourcegitcommit: 45411dcfd2dc221de9566b382a1b1c62da914208
ms.openlocfilehash: 21f2e6e9b422d497882a2414fd24974d60ea685e


---
# <a name="create-your-first-azure-service-fabric-application"></a>Vytvoření první aplikace Azure Service Fabric
> [!div class="op_single_selector"]
> * [C# – Windows](service-fabric-create-your-first-application-in-visual-studio.md)
> * [Java – Linux](service-fabric-create-your-first-linux-application-with-java.md)
> * [C# – Linux](service-fabric-create-your-first-linux-application-with-csharp.md)
> 
> 

Sada Service Fabric SDK zahrnuje doplněk pro Visual Studio, který poskytuje šablony a nástroje pro vytváření, nasazení a ladění aplikací Service Fabric. Toto téma vás provede procesem vytvoření vaší první aplikace v sadě Visual Studio.

## <a name="prerequisites"></a>Požadavky
Než začnete, ujistěte se, že máte [nastavené vývojové prostředí](service-fabric-get-started.md).

## <a name="video-walkthrough"></a>Video s návodem
Následující video vás provede kroky v tomto kurzu:

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Creating-your-first-Service-Fabric-application-in-Visual-Studio/player]
> 
> 

## <a name="create-the-application"></a>Vytvoření aplikace
Aplikace Service Fabric může obsahovat jednu nebo víc služeb, z nichž každá má určitou roli při poskytování funkcí aplikace. První projekt aplikace a s ním i první projekt služby vytvoříte pomocí průvodce novým projektem. Pokud budete chtít, můžete později přidat i další služby.

1. Spusťte sadu Visual Studio jako správce.
2. Klikněte na **Soubor > Nový projekt > Cloud > Aplikace Service Fabric**.
3. Dejte aplikaci název a klikněte na **OK**.
   
    ![Dialogové okno Nový projekt ve Visual Studiu][1]
4. Na další stránce vyberte jako první typ služby, kterou chcete do aplikace zahrnout, možnost **Stavový**. Dejte službě název a klikněte na **OK**.
   
    ![Dialogové okno Nová služba ve Visual Studiu][2]
   
   > [!NOTE]
   > Další informace o možnostech najdete v tématu [Přehled programovacího modelu Service Fabric](service-fabric-choose-framework.md).
   > 
   > 
   
    Visual Studio vytvoří projekt aplikace a projekt stavové služby a zobrazí je v Průzkumníku řešení.
   
    ![Průzkumník řešení po vytvoření aplikace se stavovou službou][3]
   
    Projekt aplikace jako takový neobsahuje žádný kód. Odkazuje ale na sadu projektů služeb. Kromě toho obsahuje další tři typy obsahu:
   
   * **Publikační profily**: slouží ke správě předvoleb nástrojů pro různá prostředí.
   * **Skripty**: Zahrnuje skript prostředí PowerShell pro nasazení/upgrade vaší aplikace. Visual Studio používá skript na pozadí. Skript můžete také volat přímo na příkazovém řádku.
   * **Definice aplikace**: Obsahuje manifest aplikace pod ve složce *ApplicationPackageRoot*. Přidružené soubory parametrů aplikace, které se nachází ve složce *ApplicationParameters*, definují aplikaci a umožňují vám ji nakonfigurovat speciálně pro určité prostředí.
     
     Přehled obsahu projektu služby najdete v tématu [Začínáme se službami Reliable Services](service-fabric-reliable-services-quick-start.md).

## <a name="deploy-and-debug-the-application"></a>Nasazení a ladění aplikace
Teď máte aplikaci a můžete ji zkusit spustit.

1. Stisknutím klávesy F5 ve Visual Studiu aplikaci nasadíte pro ladění.
   
   > [!NOTE]
   > Nasazení bude napoprvé chvíli trvat, protože Visual Studio vytváří místní cluster pro vývoj. Místní cluster spouští na jednom počítači kód platformy, na kterém budete stavět v clusteru s více počítači. V okně výstupu sady Visual Studio se zobrazí stav vytváření clusteru.
   > 
   > 
   
    Až bude cluster připravený, aplikace pro správu na hlavním panelu systému místního clusteru, která je součástí sady SDK, zobrazí upozornění.
   
    ![Upozornění na hlavním panelu systému místního clusteru][4]
2. Až se aplikace spustí, Visual Studio automaticky zobrazí prohlížeč diagnostických událostí, ve kterém uvidíte výstup trasování ze služby.
   
    ![Prohlížeč diagnostických událostí][5]
   
    V případě použití šablony stavové služby zprávy uvádí jenom hodnotu čítače zvýšenou v metodě `RunAsync` v souboru MyStatefulService.cs.
3. Pokud některou událost rozbalíte, zobrazí se další podrobnosti, včetně uzlu, na kterém kód běží. V tomto případě je to uzel _Node_2, to se ale může na vašem počítači lišit.
   
    ![Podrobnosti v prohlížeči diagnostických událostí][6]
   
    Místní cluster obsahuje pět uzlů hostovaných na jednom počítači. Napodobuje tím cluster s pěti uzly, ve kterém se uzly nachází na různých počítačích. Abychom napodobili situaci, kdy počítač přestane pracovat, zatímco je na něm spuštěn ladící program sady Visual Studio, zkusme teď jeden z uzlů místního clusteru zastavit.
   
   > [!NOTE]
   > Diagnostické události aplikace vydávané šablonou projektu používají zahrnutou třídu `ServiceEventSource`. Další informace najdete v tématu [Místní monitorování a diagnostika služeb](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md).
   > 
   > 
4. Najděte ve svém projektu služby třídu odvozenou od StatefulService (třeba MyStatefulService) a na první řádek metody `RunAsync` umístěte zarážku.
   
    ![Zarážka v metodě RunAsync stavové služby][7]
5. Pokud chcete spustit Service Fabric Explorer, na hlavním panelu systému klikněte pravým tlačítkem na aplikaci Local Cluster Manager a vyberte **Manage Local Cluster** (Správa místního clusteru).
   
    ![Spuštění nástroje Service Fabric Explorer z aplikace Local Cluster Manager][systray-launch-sfx]
   
    Service Fabric Explorer nabízí vizuální znázornění clusteru, včetně sady aplikací, které jsou v něm nasazené, a sady fyzických uzlů, které ho tvoří. Další informace o nástroji Service Fabric Explorer najdete v tématu [Vizualizace vašeho clusteru](service-fabric-visualizing-your-cluster.md).
6. V levém podokně rozbalte položky **Cluster > Uzly** a najděte uzel, na kterém běží váš kód.
7. Kliknutím na **Akce > Deaktivovat (restartovat)** simulujte restartování počítače. Nebo deaktivujte uzel ze zobrazení seznamu uzlů v levém podokně.)
   
    ![Zastavení uzlu v Service Fabric Exploreru][sfx-stop-node]
   
    Na okamžik byste měli zahlédnout, jak Visual Studio dojde k vaší zarážce a výpočet probíhající na jednom uzlu plynule převezme jiný uzel.
8. Vraťte se do prohlížeče diagnostických událostí a podívejte se na zprávy. Hodnota čítače pořád roste, i když události ve skutečnosti přicházejí z jiného uzlu.
   
    ![Prohlížeč diagnostických událostí po převzetí služeb při selhání][diagnostic-events-viewer-detail-post-failover]

## <a name="switch-cluster-mode"></a>Přepnutí režimu clusteru
Místní vývojový cluster je ve výchozím nastavení nakonfigurovaný tak, aby běžel jako cluster s pěti uzly, což se hodí při ladění služeb nasazených na více uzlech. Nasazení aplikace do vývojového clusteru s pěti uzly může ovšem určitý čas trvat. Pokud chcete rychle iterovat změny kódu bez toho, aby vaše aplikace běžela na pěti uzlech, přepněte vývojový cluster do režimu jednoho uzlu. Jestliže chcete svůj kód spustit na clusteru s jedním uzlem, klikněte na hlavním panelu systému pravým tlačítkem na nástroj Local Cluster Manager a vyberte **Přepnout režim clusteru -> 1 uzel**.  

![Přepnutí režimu clusteru][switch-cluster-mode]

Po změně režimu clusteru se vývojový cluster restartuje a zároveň se z něj odeberou všechny na něm zřízené nebo spuštěné aplikace.

Můžete také změnit režim clusteru pomocí PowerShellu:

1. Jako správce spusťte nové okno prostředí PowerShell.
2. Spusťte instalační skript clusteru z této složky sady SDK:
   
    ```powershell
    & "$ENV:ProgramFiles\Microsoft SDKs\Service Fabric\ClusterSetup\DevClusterSetup.ps1" -CreateOneNodeCluster
    ```
   
    Instalace clusteru bude chvíli trvat. Po dokončení instalace byste měli vidět výstup podobný tomuhle:
   
    ![Výstup po instalaci clusteru][cluster-setup-success-1-node]

## <a name="cleaning-up"></a>Čištění
Před zabalením je dobré si uvědomit, že místní cluster je skutečný. Při zastavení ladicího programu se odebere instance aplikace a zruší se registrace typu aplikace. Cluster ale dál běží na pozadí. Při správě clusteru máte několik možností:

1. Pokud chcete cluster zastavit, ale ponechat si data aplikací a trasování, klikněte v aplikaci na hlavním panelu systému na **Stop Local Cluster** (Zastavit místní cluster).
2. Pokud chcete cluster úplně odstranit, klikněte v aplikaci na hlavním panelu systému na možnost **Remove Local Cluster** (Odebrat místní cluster). Pokud vyberete tuto možnost, další nasazení po příštím stisknutí klávesy F5 ve Visual Studiu bude zase pomalé. Cluster odstraňte jenom v případě, že se místní cluster nechystáte nějakou dobu používat nebo potřebujete uvolnit prostředky.

## <a name="next-steps"></a>Další kroky
* Naučte se vytvořit [cluster v Azure](service-fabric-cluster-creation-via-portal.md) nebo [samostatný clusteru ve Windows](service-fabric-cluster-creation-for-windows-server.md).
* Zkuste si vytvořit službu pomocí programovacího modelu [Reliable Services](service-fabric-reliable-services-quick-start.md) nebo [Reliable Actors](service-fabric-reliable-actors-get-started.md).
* Zkuste nasadit [kontejner Windows](service-fabric-deploy-container.md) nebo stávající aplikaci jako [hostující spustitelný soubor](service-fabric-deploy-existing-app.md).
* Zjistěte, jak vystavit svoje služby na internetu pomocí [rozhraní front-end webové služby](service-fabric-add-a-web-frontend.md).
* Projděte si [testovací prostředí](https://msdnshared.blob.core.windows.net/media/2016/07/SF-Lab-Part-I.docx) a vytvořte bezstavovou službu, nakonfigurujte sledování a hlášení o stavu a proveďte upgrade aplikace.
* Informace o [možnostech podpory pro Service Fabric](service-fabric-support.md)

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



<!--HONumber=Feb17_HO3-->


