<properties
    pageTitle="Začínáme používat Azure Mobile Services pro aplikace iOS | JavaScript Backend"
    description="Využijte tento kurz a začněte využívat Azure Mobile Services pro vývoj pro iOS."
    services="mobile-services"
    documentationCenter="ios"
    authors="krisragh"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="mobile-services"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-ios"
    ms.devlang="objective-c"
    ms.topic="hero-article"
    ms.date="07/21/2016"
    ms.author="krisragh"/>

# <a name="getting-started"> </a>Začínáme používat Mobile Services

[AZURE.INCLUDE [mobile-services-selector-get-started](../../includes/mobile-services-selector-get-started.md)]

&nbsp;

[AZURE.INCLUDE [mobile-service-note-mobile-apps](../../includes/mobile-services-note-mobile-apps.md)]
> Ekvivalentní verzi mobilní aplikace tohoto tématu naleznete v části [Vytvoření aplikace pro iOS v Azure Mobile Apps](../app-service-mobile/app-service-mobile-ios-get-started.md).

V tomto kurzu se dozvíte, jak přidat cloudovou back-end službu do mobilní aplikace iOS pomocí Azure Mobile Services.

V tomto návodu vytvoříte novou mobilní službu a jednoduchou aplikaci _Seznam úkolů_, která ukládá data aplikací v rámci nové mobilní služby. Vytvořená mobilní služba používá JavaScript pro serverovou obchodní logiku. Postup vytvoření mobilní služby se serverovou obchodní logikou v rozhraní .NET, naleznete v části [Verze rozhraní .NET back-end] tohoto tématu.

> [AZURE.NOTE] K dokončení tohoto kurzu potřebujete mít účet Azure. Pokud účet nemáte, můžete si zaregistrovat zkušební verzi Azure a získat [bezplatné mobilní služby, které můžete používat i po skončení zkušebního období](https://azure.microsoft.com/pricing/details/mobile-services/). Podrobnosti najdete v tématu [Bezplatná zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=AE564AB28&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdevelop%2Fmobile%2Ftutorials%2Fget-started-ios%2F%20).

## <a name="create-new-service"> </a>Vytvoření nové mobilní služby

[AZURE.INCLUDE [mobile-services-create-new-service](../../includes/mobile-services-create-new-service.md)]

## Vytvoření nové aplikace iOS

Pro vytvoření nové aplikace připojené k mobilní službě můžete využít jednoduchý Rychlý start na portálu Azure Classic:

1. Na [portál Azure Classic] klikněte na tlačítko **Mobile Services** a pak klikněte na mobilní službu, kterou jste právě vytvořili.

2. Na kartě Rychlý start klikněte na tlačítko **iOS** pod položkou **Vybrat platformu** a rozbalte možnost **Vytvořit novou aplikaci iOS**. Zobrazí se kroky k vytvoření aplikace iOS připojené k mobilní službě.

3. Klikněte na tlačítko **Vytvořit tabulku TodoItem** a vytvořte tabulku pro ukládání dat aplikací.

4. V části **Stažení a spuštění aplikace** klikněte na tlačítko **Stáhnout**. Tato akce stáhne projekt pro vzorovou aplikaci _Seznam úkolů_, která je připojena k mobilní službě společně s Mobile Services iOS SDK. Uložte komprimovaný soubor projektu do místního počítače a poznamenejte si, kam jste jej uložili.

## Spuštění nové aplikace iOS

[AZURE.INCLUDE [mobile-services-ios-run-app](../../includes/mobile-services-ios-run-app.md)]

<ol start="4">
<li><p>Zpět v [portál Azure Classic] klikněte na kartu **DATA** a pak klikněte na tabulku **TodoItem**. Tato možnost vám umožňuje procházet data vložená aplikací do tabulky.<p></li></ol></p>

## <a name="next-steps"> </a>Další kroky
Naučte se provádět další důležité úkoly v Mobile Services:

* [Začínáme s offline synchronizací dat]
    <br/>Zjistěte, jak můžete používat synchronizaci dat offline k vytvoření fungující a robustní aplikace.

* [Přidejte ověřování do existující aplikace]
    <br/>Zjistěte, jak ověřovat uživatele vaší aplikace pomocí zprostředkovatele identity.

* [Přidejte nabízené oznámení do existující aplikace]
    <br/>Podívejte se, jak do aplikace odeslat velmi základní nabízené oznámení.

[AZURE.INCLUDE [app-service-disqus-feedback-slug](../../includes/app-service-disqus-feedback-slug.md)]


<!-- Anchors. -->
[Začínáme používat Mobile Services]:#getting-started
[Vytvoření nové mobilní služby]:#create-new-service
[Definování instance mobilních služeb]:#define-mobile-service-instance
[Další kroky]:#next-steps

<!-- Images. -->
[6]: ./media/mobile-services-ios-get-started/mobile-portal-quickstart-ios.png
[7]: ./media/mobile-services-ios-get-started/mobile-quickstart-steps-ios.png
[8]: ./media/mobile-services-ios-get-started/mobile-xcode-project.png

[10]: ./media/mobile-services-ios-get-started/mobile-quickstart-startup-ios.png
[11]: ./media/mobile-services-ios-get-started/mobile-data-tab.png
[12]: ./media/mobile-services-ios-get-started/mobile-data-browse.png


<!-- URLs. -->
[Začínáme s offline synchronizací dat]: mobile-services-ios-get-started-offline-data.md
[Přidejte ověřování do existující aplikace]: mobile-services-dotnet-backend-ios-get-started-users.md
[Přidejte nabízené oznámení do existující aplikace]: mobile-services-dotnet-backend-ios-get-started-push.md


[Mobile Services iOS SDK]: https://go.microsoft.com/fwLink/p/?LinkID=266533
[portál Azure Classic]: https://manage.windowsazure.com/
[Xcode]: https://go.microsoft.com/fwLink/p/?LinkID=266532
[Verze rozhraní .NET back-end]: mobile-services-dotnet-backend-ios-get-started.md



<!---HONumber=Aug16_HO4-->


