<properties
    pageTitle="Začínáme s Azure Mobile Engagementem pro nasazení Unity Android"
    description="Naučte se používat Azure Mobile Engagement s analytickými funkcemi a nabízenými oznámeními pro aplikace Unity nasazované v zařízeních iOS."
    services="mobile-engagement"
    documentationCenter="unity"
    authors="piyushjo"
    manager=""
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-unity-android"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="03/25/2016"
    ms.author="piyushjo" />

# Začínáme s Azure Mobile Engagementem pro nasazení Unity Android

[AZURE.INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

V tomto tématu si ukážeme, jak používat Azure Mobile Engagement, jak porozumět používání aplikací a odesílat nabízená oznámení segmentovaným uživatelům aplikace Unity při nasazení v zařízení Android.
Tento kurz používá jako výchozí bod klasický kurz pro Unity, hru Roll a Ball. Před pokračováním v integraci Mobile Engagementu, kterou představujeme v kurzu níže, postupujte podle kroků v tomto [kurzu](mobile-engagement-unity-roll-a-ball.md). 

V tomto kurzu budete potřebovat následující:

+ [Unity Editor](http://unity3d.com/get-unity)
+ [Mobile Engagement Unity SDK](https://aka.ms/azmeunitysdk)
+ Google Android SDK

> [AZURE.NOTE] K dokončení tohoto kurzu potřebujete mít aktivní účet Azure. Pokud účet nemáte, můžete si během několika minut vytvořit bezplatný zkušební účet. Podrobnosti najdete v článku [Bezplatná zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-unity-android-get-started).

##<a id="setup-azme"></a>Nastavení Mobile Engagementu pro vaši aplikaci pro Android

[AZURE.INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal.md)]

##<a id="connecting-app"></a>Připojení aplikace k back-endu Mobile Engagementu

###Import balíčku Unity

1. Stáhněte si [balíček Mobile Engagement Unity](https://aka.ms/azmeunitysdk) a uložte jej do místního počítače. 

2. Přejděte na stránku **Assets (Prostředky) -> Import Package (Importovat balíček) -> Custom Package (Vlastní balíček)** a vyberte balíček, který jste stáhli v předchozím kroku. 

    ![][70] 

3. Zkontrolujte, zda jsou vybrány všechny soubory, a klikněte na tlačítko **Import**. 

    ![][71] 

4. Po úspěšném importu se importované soubory SDK zobrazí v projektu.  

    ![][72] 

###Aktualizace EngagementConfiguration

1. Ve složce SDK otevřete soubor skriptu **EngagementConfiguration** a aktualizujte **ANDROID\_CONNECTION\_STRING** připojovacím řetězcem, který jste dříve získali z portálu Azure.  

    ![][73]

2. Soubor uložte 

3. Spusťte **File (Soubor) -> Engagement -> Generate Android Manifest (Generovat manifest Android)**. Jedná se o plugin přidaný sadou Mobile Engagement SDK, který po kliknutí automaticky aktualizuje nastavení projektu. 

    ![][74]

> [AZURE.IMPORTANT] Tento příkaz spusťte při každé aktualizaci souboru **EngagementConfiguration**, jinak se změny v aplikaci neprojeví. 

###Konfigurace aplikace pro základní sledování

1. Otevřete skript **PlayerController** připojený k objektu Player pro úpravy. 

2. Přidejte následující příkaz using:

        using Microsoft.Azure.Engagement.Unity;

3. Do metody `Start()` přidejte následující
    
        EngagementAgent.Initialize();
        EngagementAgent.StartActivity("Home");

###Nasazení a spuštění aplikace
Před nasazením aplikace Unity do zařízení zkontrolujte, zda je v počítači nainstalován balíček Android SDK. 

1. Připojte zařízení Android k vašemu počítači. 

2. Klikněte na položky **File (Soubor) -> Build Settings (Nastavení sestavení)** 

    ![][40]

3. Vyberte **Android** a potom klikněte na **Switch Platform** (Přepnout platformu).

    ![][51]

    ![][52]

4. Klikněte na **Player settings** (Nastavení hráče) a zadejte platný identifikátor svazku. 

    ![][53]

5. Nakonec klikněte na **Build And Run** (Sestavit a spustit).

    ![][54]

6. Můžete být vyzváni k zadání názvu složky, do které se uloží balíček Android. 

7. Pokud všechno půjde dobře, balíček se nasadí v připojeném zařízení a v telefonu by se měla zobrazit vaše hra Unity. 

##<a id="monitor"></a>Připojení aplikace se sledováním v reálném čase

[AZURE.INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

##<a id="integrate-push"></a>Povolení nabízených oznámení a zasílání zpráv v aplikaci

[AZURE.INCLUDE [Enable Google Cloud Messaging](../../includes/mobile-engagement-enable-google-cloud-messaging.md)]

###Aktualizace EngagementConfiguration

1. Ve složce SDK otevřete soubor skriptu **EngagementConfiguration** a aktualizujte **ANDROID\_GOOGLE\_NUMBER** **číslem projektu Google**, které jste dříve získali z portálu Google Cloud Developer. Jedná se o řetězcovou hodnotu, takže ji nezapomeňte zavřít do uvozovek. 

    ![][75]

2. Uložte soubor. 

3. Spusťte **File (Soubor) -> Engagement -> Generate Android Manifest (Generovat manifest Android)**. Jedná se o plugin přidaný sadou Mobile Engagement SDK, který po kliknutí automaticky aktualizuje nastavení projektu. 

    ![][74]

###Konfigurace aplikace k přijímání oznámení

1. Otevřete skript **PlayerController** připojený k objektu Player pro úpravy. 

2. Do metody `Start()` přidejte následující

        EngagementReachAgent.Initialize();

3. Když je nyní aplikace aktualizovaná, nasaďte ji a spusťte v zařízení podle níže uvedených pokynů. 

[AZURE.INCLUDE [Send notification from portal](../../includes/mobile-engagement-android-send-push-from-portal.md)]

<!-- Images -->
[40]: ./media/mobile-engagement-unity-android-get-started/40.png
[70]: ./media/mobile-engagement-unity-android-get-started/70.png
[71]: ./media/mobile-engagement-unity-android-get-started/71.png
[72]: ./media/mobile-engagement-unity-android-get-started/72.png
[73]: ./media/mobile-engagement-unity-android-get-started/73.png
[74]: ./media/mobile-engagement-unity-android-get-started/74.png
[75]: ./media/mobile-engagement-unity-android-get-started/75.png
[51]: ./media/mobile-engagement-unity-android-get-started/51.png
[52]: ./media/mobile-engagement-unity-android-get-started/52.png
[53]: ./media/mobile-engagement-unity-android-get-started/53.png
[54]: ./media/mobile-engagement-unity-android-get-started/54.png


<!--HONumber=Jun16_HO2-->


