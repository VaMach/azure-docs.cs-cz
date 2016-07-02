<properties
    pageTitle="Začínáme používat Mobile Services pro univerzální aplikace Windows | Microsoft Azure"
    description="Využijte tento kurz a začněte využívat Azure Mobile Services pro vývoj univerzálních aplikací pro Windows v C#."
    services="mobile-services"
    documentationCenter="windows"
    authors="ggailey777"
    manager="erikre"
    editor=""/>

<tags
    ms.service="mobile-services"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="05/11/2016"
    ms.author="glenga"/>


# <a name="getting-started"> </a>Začínáme používat Mobile Services

[AZURE.INCLUDE [mobile-services-selector-get-started](../../includes/mobile-services-selector-get-started.md)]

&nbsp;

[AZURE.INCLUDE [mobile-service-note-mobile-apps](../../includes/mobile-services-note-mobile-apps.md)]
> Ekvivalentní verzi tohoto tématu pro Mobile Apps naleznete v části [Vytvoření aplikace pro Windows pomocí mobilních aplikací](../app-service-mobile/app-service-mobile-windows-store-dotnet-get-started.md).

V tomto kurzu se dozvíte, jak přidat cloudovou back-end službu do univerzální aplikace pro Windows pomocí Azure Mobile Services. Univerzální řešení aplikací pro Windows zahrnuje projekty pro aplikace Windows Store 8.1 a Windows Phone Store 8.1 a běžně sdílený projekt. Další informace naleznete v tématu [Sestavení univerzálních aplikací pro Windows, které se zaměřují na Windows a Windows Phone](http://msdn.microsoft.com/library/windows/apps/xaml/dn609832.aspx).

V tomto návodu vytvoříte novou mobilní službu a jednoduchou aplikaci *Seznam úkolů*, která ukládá data aplikací v rámci nové mobilní služby. Mobilní služby, které vytvoříte, využívají podporované jazyky .NET pomocí sady Visual Studio pro serverovou obchodní logiku a ke správě mobilních služeb. Vytvoření mobilní služby, který umožňuje zapisovat serverovou obchodní logiku v jazyce JavaScript naleznete v části Back-end verze jazyka JavaScript v tomto tématu.

>[AZURE.NOTE]Toto téma ukazuje, jak vytvořit nový projekt mobilních služeb a univerzální aplikaci pro Windows pomocí portál Azure Classic. Pomocí Visual Studia 2013 Update 3 můžete také přidat nový projekt mobilních služeb do existujícího řešení sady Visual Studio. Další informace naleznete v tématu [Přidání Mobile Services do stávající aplikace](mobile-services-dotnet-backend-windows-universal-dotnet-get-started-data.md).

>Pokud chcete přidat mobilní službu do projektu aplikace Windows Phone 8.0 nebo Windows Phone Silverlight 8.1, projděte si téma [Přidání Mobile Services do stávající aplikace pro Windows Phone](mobile-services-windows-phone-get-started-data.md).

[AZURE.INCLUDE [mobile-services-windows-universal-get-started](../../includes/mobile-services-windows-universal-get-started.md)]

Pro absolvování tohoto kurzu potřebujete:

* Aktivní účet Azure. Pokud účet nemáte, můžete si zaregistrovat zkušební verzi Azure a získat až 10 bezplatných mobilních služeb, které můžete používat i po skončení zkušebního období. Podrobnosti najdete v tématu [Bezplatná zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-services-dotnet-backend-windows-store-dotnet-get-started%2F).
* [Visual Studio 2013].

## Vytvoření nové mobilní služby

[AZURE.INCLUDE [mobile-services-dotnet-backend-create-new-service](../../includes/mobile-services-dotnet-backend-create-new-service.md)]

## Vytvoření nové univerzální aplikace pro Windows

Jakmile vytvoříte mobilní službu, můžete snadno využít postup rychlého spuštění portálu Azure Classic k vytvoření nové aplikace, nebo úpravě stávající aplikace pro připojení k mobilní službě.

V této části vytvoříte novou univerzální aplikaci pro Windows, která je připojena k mobilní službě.

1. Na [portál Azure Classic] klikněte na tlačítko **Mobile Services** a pak klikněte na mobilní službu, kterou jste právě vytvořili.

2. Na kartě Rychlé spuštění klikněte na tlačítko **Windows** pod položkou **Vybrat platformu** a rozbalte možnost **Vytvořit novou aplikaci Windows Store**.

    Zobrazí se tří jednoduché kroky k vytvoření aplikace pro Windows Store připojené k mobilní službě.

    ![Postup rychlého spuštění Mobile Services](./media/mobile-services-dotnet-backend-windows-store-dotnet-get-started/mobile-quickstart-steps.png)

3. Pokud jste tak ještě neučinili, stáhněte a nainstalujte nástroje [Visual Studio 2013] do svého místního počítače nebo virtuálního počítače.

4. V části **Stažení a spuštění aplikace a služby v místě** vyberte jazyk pro aplikaci Windows Store a pak klikněte na tlačítko **Stáhnout**.

    Tato soubory ke stažení a řešení obsahují projekty pro mobilní službu a pro vzorovou aplikaci _Seznam úkolů_, která je připojena k mobilní službě. Uložte komprimovaný soubor projektu do místního počítače a poznamenejte si, kam jste jej uložili.

## Testování aplikací proti místní mobilní službě

[AZURE.INCLUDE [mobile-services-dotnet-backend-test-local-service-dotnet](../../includes/mobile-services-dotnet-backend-test-local-service-dotnet.md)]

>[AZURE.NOTE]Kód, který přistupuje k mobilní službě pro dotazování a vkládání dat a který se nachází v souboru MainPage.xaml.cs, můžete zkontrolovat.


## Publikování vaši mobilní služby

[AZURE.INCLUDE [mobile-services-dotnet-backend-publish-service](../../includes/mobile-services-dotnet-backend-publish-service.md)]


<ol start="4">
<li><p>V projektu sdíleného kódu otevřete soubor App.xaml.cs, vyhledejte kód, který vytvoří instanci <a href="http://msdn.microsoft.com/library/Windowsazure/microsoft.windowsazure.mobileservices.mobileserviceclient.aspx" target="_blank">MobileServiceClient</a>, okomentujte kód, který vytvoří tohoto klienta pomocí <em>localhost</em> a zrušte komentář kódu, který vytvoří klienta pomocí adresy URL vzdálené mobilní služby, který vypadá podobně jako následující:</p>

        <pre><code>public static MobileServiceClient MobileService = new MobileServiceClient(
            "https://todolist.azure-mobile.net/",
            "XXXX-APPLICATION-KEY-XXXXX");</code></pre>

    <p>The client will now access the mobile service published to Azure.</p></li>
</ol>

## Otestujte aplikaci pro mobilní služby hostované v Azure

Teď po publikování mobilní služby a připojení klienta ke vzdálené mobilní službě hostované v Azure můžeme spustit aplikaci pomocí Azure pro uložení položky.

[AZURE.INCLUDE [mobile-services-windows-universal-test-app](../../includes/mobile-services-windows-universal-test-app.md)]


## Další kroky
Teď, když jste dokončili rychlé spuštění, se naučíte, jak provádět další důležité úkoly v Mobile Services:

* [Přidání Mobile Services do stávající aplikace][Začínáme s daty]
  <br/>Další informace o ukládání a dotazování dat pomocí Mobile Services.

* [Začínáme s offline synchronizací dat]
  <br/>Zjistěte, jak můžete používat synchronizaci dat offline k vytvoření fungující a robustní aplikace.

* [Přidat ověřování do aplikace pro Mobile Services ][Začínáme s ověřováním]
  <br/>Zjistěte, jak ověřovat uživatele vaší aplikace pomocí zprostředkovatele identity.

* [Přidat nabízená oznámení do aplikace][Začínáme s nabízenými oznámeními]
  <br/>Podívejte se, jak do aplikace odeslat velmi základní nabízené oznámení.

* [Poradce při potížích s .NET back-end pro Mobile Services]
  <br/> Další informace, jak diagnostikovat a opravit problémy, které mohou nastat u .NET back-end pro Mobile Services.

Další informace o univerzálních aplikací pro Windows naleznete v části [Podpora více platforem zařízení z jedné mobilní služby](mobile-services-how-to-use-multiple-clients-single-service.md#shared-vs).

[AZURE.INCLUDE [app-service-disqus-feedback-slug](../../includes/app-service-disqus-feedback-slug.md)]

<!-- Anchors. -->

<!-- Images. -->



<!-- URLs. -->
[Visual Studio 2013]: https://go.microsoft.com/fwLink/p/?LinkID=257546
[Začínáme s daty]: mobile-services-dotnet-backend-windows-universal-dotnet-get-started-data.md
[Začínáme s offline synchronizací dat]: mobile-services-windows-store-dotnet-get-started-offline-data.md
[Začínáme s ověřováním]: mobile-services-dotnet-backend-windows-universal-dotnet-get-started-users.md
[Začínáme s nabízenými oznámeními]: mobile-services-dotnet-backend-windows-universal-dotnet-get-started-push.md
[Visual Studio Professional 2013]: https://go.microsoft.com/fwLink/p/?LinkID=257546
[Mobile Services SDK]: http://go.microsoft.com/fwlink/?LinkId=257545
[JavaScript a HTML]: mobile-services-win8-javascript/
[portál Azure Classic]: https://manage.windowsazure.com/
[Poradce při potížích s .NET back-end pro Mobile Services]: mobile-services-dotnet-backend-how-to-troubleshoot.md



<!--HONumber=Jun16_HO2-->


