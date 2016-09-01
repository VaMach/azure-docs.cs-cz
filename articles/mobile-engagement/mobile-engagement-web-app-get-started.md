<properties
    pageTitle="Začínáme s Azure Mobile Engagementem pro Web Apps | Microsoft Azure"
    description="Naučte se používat Azure Mobile Engagement s analytickými funkcemi a nabízenými oznámeními pro Web Apps."
    services="mobile-engagement"
    documentationCenter="Mobile"
    authors="piyushjo"
    manager=""
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="na"
    ms.devlang="js"
    ms.topic="hero-article"
    ms.date="06/01/2016"
    ms.author="piyushjo" />

# Začínáme s Azure Mobile Engagementem pro Web Apps

[AZURE.INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

V tomto tématu se dozvíte, jak můžete pomocí Azure Mobile Engagementu porozumět využití své webové aplikace.

V tomto kurzu budete potřebovat následující:

+ Visual Studio 2015 nebo jiný editor podle vaší volby
+ [Sada Web SDK](http://aka.ms/P7b453) 

Sada Web SDK je ve verzi Preview, takže zatím podporuje jenom analytické funkce, ale ne odesílání nabízených oznámení v prohlížeči nebo v aplikaci. 

> [AZURE.NOTE] K dokončení tohoto kurzu potřebujete mít aktivní účet Azure. Pokud účet nemáte, můžete si během několika minut vytvořit bezplatný zkušební účet. Podrobnosti najdete v článku [Bezplatná zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-web-app-get-started).

##Nastavení Mobile Engagementu pro vaši webovou aplikaci

[AZURE.INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

##<a id="connecting-app"></a>Připojení aplikace k back-endu Mobile Engagementu

V tomto kurzu si představíme „základní integraci“, tedy minimální sadu, která je zapotřebí pro shromažďování dat.

Pomocí sady Visual Studio vytvoříme základní webovou aplikaci, na které si předvedeme integraci, i když uvedený postup můžete provést také s libovolnou webovou aplikací vytvořenou mimo sadu Visual Studio. 

###Vytvoření nové webové aplikace

Následující postup předpokládá použití sady Visual Studio 2015, ačkoliv kroky se podobají předchozím verzím sady Visual Studio. 

1. Spusťte Visual Studio a na obrazovce **Domů** vyberte **Nový projekt**.

2. V místním okně vyberte **Web** -> **Webová aplikace ASP.Net**. Vyplňte **Název** aplikace, **Umístění** **Název řešení** a potom klikněte na **OK**.

3. V místním okně **Vyberte šablonu** zvolte v části **Šablony ASP.Net 4.5** možnost **Prázdná** a klikněte na **OK**. 

Vytvořili jste nový prázdný projekt webové aplikace, do kterého budeme integrovat sadu Azure Mobile Engagement Web SDK.

###Připojení aplikace k back-endu Mobile Engagementu

1. Vytvořte ve svém řešení novou složku s názvem **javascript** a přidejte do ní soubor JS sady Web SDK **azure-engagement.js**. 

2. Do této složky javascript přidejte nový soubor **main.js** s následujícím kódem. Nezapomeňte aktualizovat připojovací řetězec. Tento objekt `azureEngagement` se bude používat k přístupu k metodám sady Web SDK. 

        var azureEngagement = {
            debug: true,
            connectionString: 'xxxxx'
        };

    ![Visual Studio se soubory JS][1]

##Povolení sledování v reálném čase

Pokud chcete začít odesílat data a zajistit, aby byli uživatelé aktivní, musíte odeslat alespoň jednu aktivitu do rozhraní back-end Mobile Engagementu. Aktivita v kontextu webové aplikace znamená webovou stránku. 

1. Vytvořte ve svém řešení novou stránku s názvem **home.html** a nastavte ji jako úvodní stránku své webové aplikace. 
2. Do této stránky zahrňte dva kódy JavaScript, které jsme přidali předtím, a to tak, že do značky body přidáte následující kód. 

        <script type="text/javascript" src="javascript/main.js"></script>
        <script type="text/javascript" src="javascript/azure-engagement.js"></script>

3. Aktualizace značky body, aby volala metodu `startActivity` programu EngagementAgent
        
        <body onload="engagement.agent.startActivity('Home')">

4. Stránka **home.html** bude vypadat takhle:
        
        <html>
        <head>
            ...
        </head>
        <body onload="engagement.agent.startActivity('Home')">
            <script type="text/javascript" src="javascript/main.js"></script>
            <script type="text/javascript" src="javascript/azure-engagement.js"></script>
        </body>
        </html>

##Připojení aplikace se sledováním v reálném čase

[AZURE.INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

![][2]

##Rozšířená analýza

Tady jsou všechny metody, které jsou v současné době v sadě Web SDK dostupné a které můžete použít k analýze:

1. Aktivity / webové stránky:

        engagement.agent.startActivity(name);
        engagement.agent.endActivity();

2. Události
        
        engagement.agent.sendEvent(name, extras);

3. Chyby

        engagement.agent.sendError(name, extras);

4. Úlohy

        engagement.agent.startJob(name);
        engagement.agent.endJob(name);

<!-- Images. -->
[1]: ./media/mobile-engagement-web-app-get-started/visual-studio-solution-js.png
[2]: ./media/mobile-engagement-web-app-get-started/session.png




<!---HONumber=Aug16_HO4-->


