<properties
   pageTitle="Vytvoření první aplikace Service Fabric v sadě Visual Studio | Microsoft Azure"
   description="Vytvoření, nasazení a ladění aplikace Service Fabric pomocí Visual Studia"
   services="service-fabric"
   documentationCenter=".net"
   authors="rwike77"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotNet"
   ms.topic="hero-article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="03/27/2016"
   ms.author="ryanwi"/>

# Vytvoření první aplikace Azure Service Fabric v sadě Visual Studio

Sada Service Fabric SDK zahrnuje doplněk pro Visual Studio, který poskytuje šablony a nástroje pro vytváření, nasazení a ladění aplikací Service Fabric. Toto téma vás provede procesem vytvoření vaší první aplikace v sadě Visual Studio.

## Požadavky

Než začnete, ujistěte se, že máte [nastavené vývojové prostředí](service-fabric-get-started.md).

## Video s návodem

Následující video vás provede kroky v tomto kurzu:

>[AZURE.VIDEO creating-your-first-service-fabric-application-in-visual-studio]

## Vytvoření aplikace

Aplikace Service Fabric může obsahovat jednu nebo víc služeb, z nichž každá má určitou roli při poskytování funkcí aplikace. Pomocí průvodce novým projektem můžete vytvořit první projekt aplikace a s ním i první projekt služby. Další služby můžete přidat později.

1. Spusťte sadu Visual Studio jako správce.

2. Klikněte na **Soubor > Nový projekt > Cloud > Aplikace Service Fabric**.

3. Dejte aplikaci název a klikněte na **OK**.

    ![Dialogové okno Nový projekt ve Visual Studiu][1]

4. Na další stránce se zobrazí žádost o výběr typu první služby, kterou chcete do aplikace zahrnout. Pro účely tohoto kurzu vybereme možnost **Stavová**. Dejte službě název a klikněte na **OK**.

    ![Dialogové okno Nová služba ve Visual Studiu][2]

    >[AZURE.NOTE] Další informace o možnostech najdete v článku [Zvolte rozhraní](service-fabric-choose-framework.md).

    Visual Studio vytvoří projekt aplikace a projekt stavové služby a zobrazí je v Průzkumníku řešení.

    ![Průzkumník řešení po vytvoření aplikace se stavovou službou][3]

    Projekt aplikace jako takový neobsahuje žádný kód. Odkazuje ale na sadu projektů služeb. Kromě toho obsahuje další tři typy obsahu:

    - **Publikační profily**: slouží ke správě předvoleb nástrojů pro různá prostředí.

    - **Skripty**: Zahrnuje skript prostředí PowerShell pro nasazení/upgrade vaší aplikace. Tento skript používá Visual Studio na pozadí a můžete ho přímo vyvolat na příkazovém řádku.

    - **Definice aplikace**: Obsahuje soubor manifest aplikace (ve složce *ApplicationPackageRoot*) a přidružené soubory parametrů aplikace (ve složce *ApplicationParameters*), které definují aplikaci a umožňují vám nakonfigurovat ji speciálně pro dané prostředí.

    Přehled obsahu projektu služby najdete v tématu [Začínáme se službami Reliable Services](service-fabric-reliable-services-quick-start.md).

## Nasazení a ladění aplikace

Teď máte aplikaci a můžete ji zkusit spustit.

1. Stisknutím klávesy F5 ve Visual Studiu aplikaci nasadíte pro ladění.

    >[AZURE.NOTE] To bude napoprvé chvíli trvat, protože Visual Studio vytváří místní cluster pro vývoj. Místní cluster spouští na jednom počítači stejný kód platformy, na kterém později budete stavět v clusteru s více počítači. V okně výstupu sady Visual Studio se zobrazí stav vytváření clusteru.

    Až bude cluster připravený, aplikace pro správu na hlavním panelu systému místního clusteru, která je součástí sady SDK, zobrazí upozornění.

    ![Upozornění na hlavním panelu systému místního clusteru][4]

2. Až se aplikace spustí, Visual Studio automaticky zobrazí prohlížeč diagnostických událostí, ve kterém uvidíte výstup trasování ze služby.

    ![Prohlížeč diagnostických událostí][5]

    V případě použití šablony stavové služby zprávy uvádí jenom hodnotu čítače zvýšenou v metodě `RunAsync` v souboru MyStatefulService.cs.

3. Pokud některou událost rozbalíte, zobrazí se další podrobnosti, včetně uzlu, na kterém kód běží. V tomto případě je to uzel _Node_2, to se ale může na vašem počítači lišit.

    ![Podrobnosti v prohlížeči diagnostických událostí][6]

    Místní cluster obsahuje pět uzlů hostovaných na jednom počítači. Napodobuje tím cluster s pěti uzly, ve kterém se uzly nachází na různých počítačích. Zkusme teď jeden z uzlů v místním clusteru zastavit, jako by daný počítač přestal pracovat, a současně spustíme program Visual Studio Debugger.

    >[AZURE.NOTE] Diagnostické události aplikace vydávané šablonou projektu používají zahrnutou třídu `ServiceEventSource`. Další informace najdete v tématu [Místní monitorování a diagnostika služeb](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md).

4. Najděte ve svém projektu služby třídu odvozenou od StatefulService (třeba MyStatefulService) a na první řádek metody `RunAsync` umístěte zarážku.

    ![Zarážka v metodě RunAsync stavové služby][7]

5. Na hlavním panelu systému klikněte pravým tlačítkem na aplikaci Local Cluster Manager a výběrem možnosti **Manage Local Cluster** (Správa místního clusteru) spusťte Service Fabric Explorer.

    ![Spuštění nástroje Service Fabric Explorer z aplikace Local Cluster Manager][systray-launch-sfx]

    Service Fabric Explorer nabízí vizuální znázornění clusteru, včetně sady aplikací, které jsou v něm nasazené, a sady fyzických uzlů, které ho tvoří. Další informace o nástroji Service Fabric Explorer najdete v tématu [Vizualizace vašeho clusteru](service-fabric-visualizing-your-cluster.md).

6. V levém podokně rozbalte položky **Cluster > Uzly** a najděte uzel, na kterém běží váš kód.

7. Kliknutím na **Akce > Deaktivovat (restartovat)** simulujte restartování počítače. (To můžete provést také pomocí místní nabídky v zobrazení seznamu uzlu v levém podokně, když vyberete tři tečky.)

    ![Zastavení uzlu v Service Fabric Exploreru][sfx-stop-node]

    Na okamžik byste měli zahlédnout, jak Visual Studio dojde k vaší zarážce a výpočet probíhající na jednom uzlu plynule převezme jiný uzel.

8. Vraťte se do prohlížeče diagnostických událostí a podívejte se na zprávy. Všimněte si, že hodnota čítače pořád roste, i když události ve skutečnosti přicházejí z jiného uzlu.

    ![Prohlížeč diagnostických událostí po převzetí služeb při selhání][diagnostic-events-viewer-detail-post-failover]

### Čištění

  Před zabalení je dobré si uvědomit, že místní cluster je naprosto skutečný. I po zastavení ladicího programu a ukončení Visual Studia budou aplikace dál běžet v pozadí. V závislosti na povaze vašich aplikací může tato aktivita na pozadí spotřebovávat značné množství prostředků vašeho počítače. To můžete řešit pomocí několika možností:

  1. Pokud chcete jednotlivou aplikaci a všechna její data odebrat, použijte akci **Odstranit aplikaci** v Service Fabric Exploreru, a to buď v nabídce **AKCE**, nebo v místní nabídce v zobrazení seznamu aplikací v levém podokně.

    ![Odstranění aplikace v Service Fabric Exploreru][sfe-delete-application]

  2. Po odstranění aplikace z clusteru můžete zvolit možnost **Unprovision Type** (Zrušit zřízení typu), která umožňuje odebrat balíček aplikace, včetně kódu a konfigurace, z úložiště imagí clusteru.
  3. Pokud chcete cluster zastavit, ale ponechat si data aplikací a trasování, klikněte v aplikaci na hlavním panelu systému na **Stop Local Cluster** (Zastavit místní cluster).

  4. Pokud chcete cluster úplně odstranit, klikněte v aplikaci na hlavním panelu systému na možnost **Remove Local Cluster** (Odebrat místní cluster). Upozorňujeme, že pokud vyberete tuto možnost, další nasazení po příštím stisknutí klávesy F5 ve Visual Studiu bude zase pomalé. Použijte tuto možnost jenom v případě, že se místní cluster nechystáte nějakou dobu používat nebo potřebujete uvolnit prostředky.



## Další kroky

<!--
Temporarily removing this link because we have removed the ASP.NET template.

 - [See how you can expose your services to the Internet with a web service front end](service-fabric-add-a-web-frontend.md)
-->
- [Zjistěte, jak vytvořit cluster v Azure.](service-fabric-cluster-creation-via-portal.md)
- [Získejte další informace o službách Reliable Services.](service-fabric-reliable-services-quick-start.md)
- [Zkuste si vytvořit službu pomocí programovacího modelu Reliable Actors.](service-fabric-reliable-actors-get-started.md)

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



<!--HONumber=Jun16_HO2-->


