<properties
    pageTitle="Začínáme s Mobile Services pro aplikace Xamarin Android | Microsoft Azure"
    description="V tomto kurzu začnete používat Azure Mobile Services pro vývoj na platformě Xamarin Android."
    services="mobile-services"
    documentationCenter="xamarin"
    authors="lindydonna"
    manager="dwrede"
    editor="mollybos"/>

<tags
    ms.service="mobile-services"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin-android"
    ms.devlang="dotnet"
    ms.topic="get-started-article"
    ms.date="07/21/2016"
    ms.author="donnam"/>

# <a name="getting-started"> </a>Začínáme používat mobilní služby

[AZURE.INCLUDE [mobile-services-selector-get-started](../../includes/mobile-services-selector-get-started.md)]
&nbsp;

[AZURE.INCLUDE [mobile-service-note-mobile-apps](../../includes/mobile-services-note-mobile-apps.md)]
> Ekvivalentní verzi tohoto tématu pro Mobile Apps naleznete v části [Vytvoření aplikace Xamarin.Android](../app-service-mobile/app-service-mobile-xamarin-android-get-started.md).

V tomto kurzu se dozvíte, jak přidat cloudovou back-end službu do mobilní aplikace Xamarin.Android pomocí Azure Mobile Services. V tomto návodu vytvoříte novou mobilní službu a jednoduchou aplikaci _Seznam úkolů_, která ukládá data aplikací v rámci nové mobilní služby. Mobilní služby, které vytvoříte, využívají podporované jazyky .NET pomocí sady Visual Studio pro serverovou obchodní logiku a ke správě mobilních služeb. Vytvoření mobilní služby, který umožňuje zapisovat serverovou obchodní logiku v jazyce JavaScript naleznete v části [Back-end verze jazyka JavaScript] v tomto tématu.

>[AZURE.NOTE]Toto téma ukazuje, jak vytvořit nový projekt mobilních služeb pomocí portálu Azure Classic. Pomocí Visual Studia 2013 Update 2 můžete také přidat nový projekt mobilních služeb do existujícího řešení sady Visual Studio. Další informace naleznete v tématu [Rychlé spuštění: Přidání mobilní služby (.NET back-end)](http://msdn.microsoft.com/library/windows/apps/dn629482.aspx)

Zde je snímek obrazovky dokončené aplikace:

![][0]

Ve všech dalších kurzech používání Mobile Services pro aplikace pro Xamarin.Android se předpokládá dokončení tohoto kurzu.

>[AZURE.NOTE]K dokončení tohoto kurzu potřebujete mít účet Azure. Pokud účet nemáte, můžete si zaregistrovat zkušební verzi Azure a získat až 10 bezplatných mobilních služeb, které můžete používat i po skončení zkušebního období. Podrobnosti najdete v tématu [Bezplatná zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-services-dotnet-backend-xamarin-android-get-started).
>Tento kurz vyžaduje [Visual Studio Professional 2013](https://go.microsoft.com/fwLink/p/?LinkID=257546). K dispozici je bezplatná zkušební verze.

## Vytvoření nové mobilní služby

[AZURE.INCLUDE [mobile-services-dotnet-backend-create-new-service](../../includes/mobile-services-dotnet-backend-create-new-service.md)]

## Vytvoření nové aplikace Xamarin Android

Jakmile vytvoříte mobilní službu, můžete snadno využít postup rychlého spuštění portálu Classic k vytvoření nové aplikace, nebo úpravě stávající aplikace pro připojení k mobilní službě.

V této části stáhněte novou aplikaci Xamarin Android a projekt služby pro mobilní službu.

1. Pokud jste tak ještě neučinili, nainstalujte Visual Studio s funkcí Xamarin. Pokyny naleznete v tématu [Instalační program a instalace Visual Studia a Xamarin](https://msdn.microsoft.com/library/mt613162.aspx). Xamarin Studio můžete také použít na počítači Mac OS X, viz téma [Instalační program, instalace a ověření pro uživatele Mac](https://msdn.microsoft.com/library/mt488770.aspx).  

2. Na [portálu classic] klikněte na tlačítko **Mobile Services** a pak klikněte na mobilní službu, kterou jste právě vytvořili.

3. Na kartě Rychlé spuštění klikněte na tlačítko **Xamarin** pod položkou **Vybrat platformu** a rozbalte možnost **Vytvořit novou aplikaci Xamarin**.

    ![][6]

    Zobrazí se tří jednoduché kroky k vytvoření aplikace pro Xamarin Android připojené k mobilní službě.

    ![][7]

4. Pod položkou **Stáhnout a publikovat službu do cloudu** a vyberte možnost **Android** a klikněte na tlačítko **Stáhnout**.

    Tyto soubory ke stažení a řešení obsahují projekty pro mobilní službu a pro vzorovou aplikaci _Seznam úkolů_, která je připojena k mobilní službě. Uložte komprimovaný soubor projektu do místního počítače a poznamenejte si, kam jste jej uložili.

5. Uložte publikovaný profil, uložte stažený soubor do místního počítače a poznamenejte si, kam jste jej uložili.

## Testování mobilní služby

[AZURE.INCLUDE [mobile-services-dotnet-backend-test-local-service](../../includes/mobile-services-dotnet-backend-test-local-service.md)]

## Publikování vaši mobilní služby

[AZURE.INCLUDE [mobile-services-dotnet-backend-publish-service](../../includes/mobile-services-dotnet-backend-publish-service.md)]

## Spuštění aplikace Xamarin Android

Závěrečná fáze tohoto kurzu je sestavení a spuštění nové aplikace.

1. Přejděte do projektu klienta v rámci řešení mobilních služeb, buď v sadě Visual Studio, nebo Xamarin Studio.

2. Stiskněte tlačítko **Spustit** a sestavte projekt a spusťte aplikaci. Budete požádáni o výběr emulátoru nebo připojeného zařízení USB.

    > [AZURE.NOTE] Abyste mohli spustit projekt v Android emulátoru, musíte definovat minimálně jedno virtuální zařízení Android (AVD). Použijte správce AVD k vytvoření a správě těchto zařízení.

3. V aplikaci zadejte smysluplný text, například _Dokončit kurz_ a pak klikněte na ikonu plus (**+**).

    ![][10]

    Tím se odešle požadavek POST do nové mobilní služby hostované v Azure. Data z požadavku se vloží do tabulky TodoItem. Položky uložené v tabulce se vrátí pomocí mobilní služby a v seznamu se zobrazí data.

    > [AZURE.NOTE]
    > Kód, který přistupuje k mobilní službě pro účely dotazování a vkládání dat, si můžete prohlédnout v souboru ToDoActivity.cs jazyka C#.

## Další kroky
Teď, když jste dokončili rychlé spuštění, se naučíte, jak provádět další důležité úkoly v Mobile Services:

* [Začínáme s offline synchronizací dat]
  <br/>Zjistěte, jak rychlý start používá synchronizaci offline dat k tomu, aby byla aplikace svižná a robustní.

* [Začínáme s ověřováním]
  <br/>Zjistěte, jak ověřovat uživatele vaší aplikace pomocí zprostředkovatele identity.

* [Začínáme s nabízenými oznámeními]
  <br/>Podívejte se, jak do aplikace odeslat velmi základní nabízené oznámení.

* [Poradce při potížích s .NET back-end pro Mobile Services]
  <br/> Další informace, jak diagnostikovat a opravit problémy, které mohou nastat u .NET back-end pro Mobile Services.

[AZURE.INCLUDE [app-service-disqus-feedback-slug](../../includes/app-service-disqus-feedback-slug.md)]

<!-- Anchors. -->
[Začínáme používat Mobile Services]:#getting-started
[Vytvoření nové mobilní služby]:#create-new-service
[Další kroky]:#next-steps



<!-- Images. -->
[0]: ./media/mobile-services-dotnet-backend-xamarin-android-get-started/mobile-quickstart-completed-android.png
[6]: ./media/mobile-services-dotnet-backend-xamarin-android-get-started/mobile-portal-quickstart-xamarin.png
[7]: ./media/mobile-services-dotnet-backend-xamarin-android-get-started/mobile-quickstart-steps-xamarin-android.png
[8]: ./media/mobile-services-dotnet-backend-xamarin-android-get-started/mobile-xamarin-project-android-vs.png
[9]: ./media/mobile-services-dotnet-backend-xamarin-android-get-started/mobile-xamarin-project-android-xs.png
[10]: ./media/mobile-services-dotnet-backend-xamarin-android-get-started/mobile-quickstart-startup-android.png

<!-- URLs. -->
[Začínáme s offline synchronizací dat]: mobile-services-xamarin-android-get-started-offline-data.md
[Začínáme s ověřováním]: mobile-services-dotnet-backend-xamarin-android-get-started-users.md
[Začínáme s nabízenými oznámeními]: mobile-services-dotnet-backend-xamarin-android-get-started-push.md
[Visual Studio Professional 2013]: https://go.microsoft.com/fwLink/p/?LinkID=257546
[Mobile Services SDK]: http://go.microsoft.com/fwlink/?LinkId=257545
[JavaScript a HTML]: mobile-services-win8-javascript/
[klasický portál Azure]: https://manage.windowsazure.com/
[portálu classic]: https://manage.windowsazure.com/
[Back-end verze jazyka JavaScript]: mobile-services-android-get-started.md
[Poradce při potížích s .NET back-end pro Mobile Services]: mobile-services-dotnet-backend-how-to-troubleshoot.md


<!--HONumber=Aug16_HO4-->


