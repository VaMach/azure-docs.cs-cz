<properties
    pageTitle="Začínáme s Azure App Service Mobile Apps pro aplikace na platformě Xamarin.iOS | Microsoft Azure"
    description="V tomto kurzu začnete používat Mobile Apps pro vývoj na platformě Xamarin.iOS."
    services="app-service\mobile"
    documentationCenter="xamarin"
    authors="adrianhall"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="na"
    ms.tgt_pltfrm="mobile-xamarin-ios"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>



#Vytvoření aplikace Xamarin.iOS

[AZURE.INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

##Přehled

V tomto kurzu se dozvíte, jak přidat cloudovou back-end službu do mobilní aplikace Xamarin.iOS pomocí back-endu mobilní aplikace Azure.  Vytvoříte jak nový back-end mobilní aplikace, tak jednoduchou aplikaci Xamarin.iOS, která bude představovat _seznam úkolů_ a bude ukládat data do Azure.

Dokončení tohoto kurzu se předpokládá ve všech dalších kurzech k používání funkce Mobile Apps v Azure App Service pro Xamarin.iOS.

##Požadavky

Pro absolvování tohoto kurzu potřebujete:

* Aktivní účet Azure. Pokud účet nemáte, můžete si zaregistrovat zkušební verzi Azure a získat až 10 bezplatných mobilních aplikací, které můžete používat i po skončení zkušebního období. Podrobnosti najdete v článku [Bezplatná zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/).

* Visual Studio s Xamarinem. Pokyny najdete v tématu o [nastavení a instalaci pro Visual Studio a Xamarin](https://msdn.microsoft.com/library/mt613162.aspx).

* Počítač Mac s nainstalovaným Xcode verze 7.0 nebo novějším a Xamarin Studio Community. Přečtěte si témata o [nastavení a instalaci nástrojů Visual Studio a Xamarin](https://msdn.microsoft.com/library/mt613162.aspx) a o [nastavení, instalaci a ověření pro uživatele počítačů Mac](https://msdn.microsoft.com/library/mt488770.aspx) (MSDN).

>[AZURE.NOTE] Pokud chcete začít se službou Azure App Service, ještě než si zaregistrujete účet Azure, přejděte k [možnosti vyzkoušet si tuto službu](https://tryappservice.azure.com/?appServiceName=mobile). Tam si můžete v App Service hned vytvořit krátkodobou úvodní mobilní aplikaci – nepožaduje se žádná platební karta ani to s sebou nenese žádné závazky.

## Vytvoření nového back-endu mobilní aplikace Azure

Podle těchto pokynů vytvořte nový back-end mobilní aplikace:

[AZURE.INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]

## Konfigurace serverového projektu

Nyní máte zřízen back-end mobilní aplikace Azure, který je možné použít v mobilních klientských aplikacích. Dále si stáhnete serverový projekt pro jednoduchý back-end seznamu úkolů a publikujete ho v Azure.

Podle pokynů níže nakonfigurujte serverový projekt tak, aby používal buď back-end Node.js, nebo .NET.

[AZURE.INCLUDE [app-service-mobile-configure-new-backend](../../includes/app-service-mobile-configure-new-backend.md)]

## Stáhnutí a spuštění aplikace Xamarin.iOS

1. Otevřete v okně prohlížeče [Azure Portal].

2. V okně nastavení mobilní aplikace klikněte na **Začínáme** > **Xamarin.iOS**. V kroku 3 klikněte na možnost **Vytvořit novou aplikaci**, pokud ještě nebyla vybrána.  Pak klikněte na tlačítko **Stáhnout**.

    Tím se stáhne projekt, který obsahuje klientskou aplikaci připojenou k mobilní aplikaci. Uložte komprimovaný soubor projektu do místního počítače a poznamenejte si, kam jste jej uložili.

3. Extrahujte projekt, který jste stáhli, a otevřete jej v nástroji Xamarin Studio (nebo v nástroji Visual Studio).

    ![][9]

    ![][8]

4. Stiskněte klávesu F5, aby se projekt sestavil a aplikace se spustila v emulátoru iPhonu.

5. Zadejte do aplikace smysluplný text, například _Naučit se Xamarin_, a klikněte na tlačítko **+**.

    ![][10]

    Tím se pošle požadavek POST na nový back-end mobilní aplikace hostovaný v Azure. Data z požadavku se vloží do tabulky TodoItem. Položky uložené v tabulce se vrátí back-endu mobilní aplikace a v seznamu se zobrazí data.

>[AZURE.NOTE]Na kód, který přistupuje k back-endu mobilní aplikace pro dotazování a vkládání dat, se můžete podívat v souboru C# QSTodoService.cs.

##Další kroky

* [Přidání ověřování do aplikace ](app-service-mobile-xamarin-ios-get-started-users.md)
  <br/>Zjistěte, jak ověřovat uživatele aplikace pomocí zprostředkovatele identity.

* [Přidání nabízených oznámení do aplikace](app-service-mobile-xamarin-ios-get-started-push.md)
  <br/>Podívejte se, jak do aplikace odeslat velmi základní nabízené oznámení.

<!-- Anchors. -->
[Začínáme s back-endy mobilních aplikací]:#getting-started
[Vytvoření nového back-endu mobilní aplikace]:#create-new-service
[Další kroky]:#next-steps



<!-- Images. -->
[6]: ./media/app-service-mobile-xamarin-ios-get-started/xamarin-ios-quickstart.png
[8]: ./media/app-service-mobile-xamarin-ios-get-started/mobile-xamarin-project-ios-vs.png
[9]: ./media/app-service-mobile-xamarin-ios-get-started/mobile-xamarin-project-ios-xs.png
[10]: ./media/app-service-mobile-xamarin-ios-get-started/mobile-quickstart-startup-ios.png

<!-- URLs. -->
[Azure Portal]: https://portal.azure.com/



<!--HONumber=Oct16_HO1-->


