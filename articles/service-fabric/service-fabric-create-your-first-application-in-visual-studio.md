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
   ms.date="09/28/2016"
   ms.author="ryanwi"/>



# Vytvoření první aplikace Azure Service Fabric

> [AZURE.SELECTOR]
- [C#](service-fabric-create-your-first-application-in-visual-studio.md)
- [Java](service-fabric-create-your-first-linux-application-with-java.md)

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

4. Na další stránce vyberte jako první typ služby, kterou chcete do aplikace zahrnout, možnost **Stavový**. Dejte službě název a klikněte na **OK**.

    ![Dialogové okno Nová služba ve Visual Studiu][2]

    >[AZURE.NOTE] Další informace o možnostech najdete v tématu [Přehled programovacího modelu Service Fabric](service-fabric-choose-framework.md).

    Visual Studio vytvoří projekt aplikace a projekt stavové služby a zobrazí je v Průzkumníku řešení.

    ![Průzkumník řešení po vytvoření aplikace se stavovou službou][3]

    Projekt aplikace jako takový neobsahuje žádný kód. Odkazuje ale na sadu projektů služeb. Kromě toho obsahuje další tři typy obsahu:

    - **Publikační profily**: slouží ke správě předvoleb nástrojů pro různá prostředí.

    - **Skripty**: Zahrnuje skript prostředí PowerShell pro nasazení/upgrade vaší aplikace. Visual Studio používá skript na pozadí. Skript můžete také volat přímo na příkazovém řádku.

    - **Definice aplikace**: Obsahuje manifest aplikace pod ve složce *ApplicationPackageRoot*. Přidružené soubory parametrů aplikace, které se nachází ve složce *ApplicationParameters*, definují aplikaci a umožňují vám ji nakonfigurovat speciálně pro určité prostředí.

    Přehled obsahu projektu služby najdete v tématu [Začínáme se službami Reliable Services](service-fabric-reliable-services-quick-start.md).

## Nasazení a ladění aplikace

Teď máte aplikaci a můžete ji zkusit spustit.

1. Stisknutím klávesy F5 ve Visual Studiu aplikaci nasadíte pro ladění.

    >[AZURE.NOTE] Nasazení bude napoprvé chvíli trvat, protože Visual Studio vytváří místní cluster pro vývoj. Místní cluster spouští na jednom počítači stejný kód platformy, na kterém později budete stavět v clusteru s více počítači. V okně výstupu sady Visual Studio se zobrazí stav vytváření clusteru.

    Až bude cluster připravený, aplikace pro správu na hlavním panelu systému místního clusteru, která je součástí sady SDK, zobrazí upozornění.

    ![Upozornění na hlavním panelu systému místního clusteru][4]

2. Až se aplikace spustí, Visual Studio automaticky zobrazí prohlížeč diagnostických událostí, ve kterém uvidíte výstup trasování ze služby.

    ![Prohlížeč diagnostických událostí][5]

    V případě použití šablony stavové služby zprávy uvádí jenom hodnotu čítače zvýšenou v metodě `RunAsync` v souboru MyStatefulService.cs.

3. Pokud některou událost rozbalíte, zobrazí se další podrobnosti, včetně uzlu, na kterém kód běží. V tomto případě je to uzel _Node_2, to se ale může na vašem počítači lišit.

    ![Podrobnosti v prohlížeči diagnostických událostí][6]

    Místní cluster obsahuje pět uzlů hostovaných na jednom počítači. Napodobuje tím cluster s pěti uzly, ve kterém se uzly nachází na různých počítačích. Zkusme teď jeden z uzlů v místním clusteru zastavit, abychom napodobili situaci, kdy daný počítač přestane pracovat, a současně spustíme program Visual Studio Debugger.

    >[AZURE.NOTE] Diagnostické události aplikace vydávané šablonou projektu používají zahrnutou třídu `ServiceEventSource`. Další informace najdete v tématu [Místní monitorování a diagnostika služeb](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md).

4. Najděte ve svém projektu služby třídu odvozenou od StatefulService (třeba MyStatefulService) a na první řádek metody `RunAsync` umístěte zarážku.

    ![Zarážka v metodě RunAsync stavové služby][7]

5. Na hlavním panelu systému klikněte pravým tlačítkem na aplikaci Local Cluster Manager a výběrem možnosti **Manage Local Cluster** (Správa místního clusteru) spusťte Service Fabric Explorer.

    ![Spuštění nástroje Service Fabric Explorer z aplikace Local Cluster Manager][systray-launch-sfx]

    Service Fabric Explorer nabízí vizuální znázornění clusteru, včetně sady aplikací, které jsou v něm nasazené, a sady fyzických uzlů, které ho tvoří. Další informace o nástroji Service Fabric Explorer najdete v tématu [Vizualizace vašeho clusteru](service-fabric-visualizing-your-cluster.md).

6. V levém podokně rozbalte položky **Cluster > Uzly** a najděte uzel, na kterém běží váš kód.

7. Kliknutím na **Akce > Deaktivovat (restartovat)** simulujte restartování počítače. (Upozorňujeme, že deaktivaci můžete provést taky z místní nabídky v zobrazení seznamu uzlů v levém podokně.)

    ![Zastavení uzlu v Service Fabric Exploreru][sfx-stop-node]

    Na okamžik byste měli zahlédnout, jak Visual Studio dojde k vaší zarážce a výpočet probíhající na jednom uzlu plynule převezme jiný uzel.

8. Vraťte se do prohlížeče diagnostických událostí a podívejte se na zprávy. Všimněte si, že hodnota čítače pořád roste, i když události ve skutečnosti přicházejí z jiného uzlu.

    ![Prohlížeč diagnostických událostí po převzetí služeb při selhání][diagnostic-events-viewer-detail-post-failover]

## Přepnutí režimu clusteru

Místní cluster pro vývoj je ve výchozím nastavení nakonfigurován tak, aby běžel jako cluster s pěti uzly, což se hodí při ladění služeb nasazených na více uzlech. Nasazení aplikace do clusteru pro vývoj s pěti uzly může ovšem určitý čas trvat. Pokud chcete rychle iterovat změny bez toho, aby vaše aplikace běžela na pěti uzlech, můžete cluster pro vývoj přepnout do režimu jednoho uzlu. Jestliže chcete svůj kód spustit na clusteru s jedním uzlem, klikněte na hlavním panelu systému pravým tlačítkem na nástroj Local Cluster Manager a vyberte **Přepnout režim clusteru -> 1 uzel**.  

![Přepnutí režimu clusteru][switch-cluster-mode]

Po změně režimu clusteru se cluster pro vývoj restartuje a zároveň se z něj odeberou všechny na něm zřízené nebo spuštěné aplikace.

## Čištění

  Před zabalení je dobré si uvědomit, že místní cluster je naprosto skutečný. Při zastavení ladicího programu se odebere instance aplikace a zruší se registrace typu aplikace. Cluster ale dál běží na pozadí. Při správě clusteru máte několik možností:

  1. Pokud chcete cluster zastavit, ale ponechat si data aplikací a trasování, klikněte v aplikaci na hlavním panelu systému na **Stop Local Cluster** (Zastavit místní cluster).

  2. Pokud chcete cluster úplně odstranit, klikněte v aplikaci na hlavním panelu systému na možnost **Remove Local Cluster** (Odebrat místní cluster). Upozorňujeme, že pokud vyberete tuto možnost, další nasazení po příštím stisknutí klávesy F5 ve Visual Studiu bude zase pomalé. Cluster odstraňte jenom v případě, že se místní cluster nechystáte nějakou dobu používat nebo potřebujete uvolnit prostředky.

## Další kroky

- Naučte se vytvořit [cluster v Azure](service-fabric-cluster-creation-via-portal.md) nebo [samostatný clusteru ve Windows](service-fabric-cluster-creation-for-windows-server.md).
- Zkuste si vytvořit službu pomocí programovacího modelu [Reliable Services](service-fabric-reliable-services-quick-start.md) nebo [Reliable Actors](service-fabric-reliable-actors-get-started.md).
- Zjistěte, jak vystavit svoje služby na internetu, prostřednictvím [rozhraní front-end webové služby](service-fabric-add-a-web-frontend.md).
- Projděte si [testovací prostředí](https://msdnshared.blob.core.windows.net/media/2016/07/SF-Lab-Part-I.docx) a vytvořte bezstavovou službu, nakonfigurujte sledování a hlášení o stavu a proveďte upgrade aplikace.

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



<!--HONumber=Sep16_HO5-->


