<properties
    pageTitle="Začínáme s Mobile Services pro Xamarin.Android | Microsoft Azure"
    writer="craigd"
    description="Naučte se používat Azure Mobile Services s aplikací Xamarin.Android."
    documentationCenter="xamarin"
    authors="lindydonna"
    manager="dwrede"
    editor=""
    services="mobile-services"/>

<tags
    ms.service="mobile-services"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin-android"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="07/21/2016"
    ms.author="donnam"/>

# <a name="getting-started"></a>Začínáme používat Mobile Services

[AZURE.INCLUDE [mobile-services-selector-get-started](../../includes/mobile-services-selector-get-started.md)]
&nbsp;

[AZURE.INCLUDE [mobile-service-note-mobile-apps](../../includes/mobile-services-note-mobile-apps.md)]
> Ekvivalentní verzi tohoto tématu pro Mobile Apps naleznete v části [Vytvoření aplikace Xamarin.Android](../app-service-mobile/app-service-mobile-xamarin-android-get-started.md).

V tomto kurzu se dozvíte, jak přidat cloudovou back-end službu do mobilní aplikace Xamarin.Android pomocí Azure Mobile Services. V tomto návodu vytvoříte novou mobilní službu a jednoduchou aplikaci *Seznam úkolů*, která ukládá data aplikací v rámci nové mobilní služby.

Pokud upřednostňujete video, klip níže postupuje stejně jako tento kurz.

Video: „Začínáme s Xamarin a Azure Mobile Services“ od Craiga Dunna, věrného vývojáře pro Xamarin (délka: 10:05 min.)

> [AZURE.VIDEO getting-started-with-xamarin-and-mobile-services]

Zde je snímek obrazovky dokončené aplikace:

![][0]

Dokončení tohoto kurzu vyžaduje XCode a Xamarin Studio pro OS X nebo Visual Studio v systému Windows se síťovým počítačem Mac. Úplné instalační pokyny najdete v tématu [Nastavení a instalace pro Visual Studio a Xamarin](https://msdn.microsoft.com/library/mt613162.aspx). 

Stažený projekt rychlého startu obsahuje komponentu Azure Mobile Services pro Xamarin.Android. I když je tento projekt zaměřen na Android 4.2 nebo novější verze, Mobile Services SDK vyžaduje pouze Android 2.2 nebo vyšší verzi.

> [AZURE.IMPORTANT] K dokončení tohoto kurzu potřebujete mít účet Azure. Pokud účet nemáte, můžete si zaregistrovat zkušební verzi Azure a získat až 10 bezplatných mobilních služeb, které můžete používat i po skončení zkušebního období. Podrobnosti najdete v tématu [Bezplatná zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A9C9624B5).

## <a name="create-new-service"> </a>Vytvoření nové mobilní služby

[AZURE.INCLUDE [mobile-services-create-new-service](../../includes/mobile-services-create-new-service.md)]

## Vytvoření nové aplikace Xamarin.Android

Jakmile vytvoříte mobilní službu, můžete snadno využít postup rychlého spuštění portálu Azure Classic k vytvoření nové aplikace, nebo úpravě stávající aplikace pro připojení k mobilní službě.

V této části vytvoříte novou aplikaci pro Xamarin.Android, která je připojena k mobilní službě.

1.  Na [portál Azure Classic] klikněte na tlačítko **Mobile Services** a pak klikněte na mobilní službu, kterou jste právě vytvořili.

2. Na kartě Rychlé spuštění klikněte na tlačítko **Xamarin.Android** pod položkou **Vybrat platformu** a rozbalte možnost **Vytvořit novou aplikaci Android**.

    ![][6]

    Zobrazí se tří jednoduché kroky k vytvoření aplikace pro Xamarin.Android připojené k mobilní službě.

    ![][7]

3. Klikněte na tlačítko **Vytvořit tabulku TodoItem** a vytvořte tabulku pro ukládání dat aplikací.

4. V části **Stažení a spuštění aplikace** klikněte na tlačítko **Stáhnout**.

    Tato akce stáhne projekt pro vzorovou aplikaci _Seznam úkolů_, která je připojena k mobilní službě. Uložte komprimovaný soubor projektu do místního počítače a poznamenejte si, kam jste jej uložili.

## Spusťte svoji aplikaci pro Android

Závěrečná fáze tohoto kurzu je sestavení a spuštění nové aplikace.

1. Přejděte do umístění, kam jste uložili komprimované soubory projektu a rozbalte soubory do počítače.

2. V aplikaci Xamarin Studio nebo Visual Studio klikněte na tlačítko **Soubor**, **Otevřít**, přejděte na nekomprimované ukázkové soubory a výběrem souboru **XamarinTodoQuickStart.Android.sln** ho otevřete.

3. Stiskněte tlačítko **Spustit** a sestavte projekt a spusťte aplikaci. Budete požádáni o výběr emulátoru nebo připojeného zařízení USB.

    > [AZURE.NOTE] Abyste mohli spustit projekt v Android emulátoru, musíte definovat alespoň jedno virtuální zařízení Android (AVD). Použijte správce AVD k vytvoření a správě těchto zařízení.

4. V aplikaci zadejte smysluplný text, například _Dokončit kurz_ a klikněte na tlačítko **Přidat**.

    ![][10]

    Tím se odešle požadavek POST do nové mobilní služby hostované v Azure. Data z požadavku se vloží do tabulky TodoItem. Položky uložené v tabulce se vrátí pomocí mobilní služby a v seznamu se zobrazí data.

    > [AZURE.NOTE]
    > Kód, který přistupuje k mobilní službě pro účely dotazování a vkládání dat, si můžete prohlédnout v souboru ToDoActivity.cs jazyka C#.

6. Zpět v [portál Azure Classic] klikněte na kartu **Data** a pak klikněte na tabulku **TodoItems**.

    ![][11]

    Tato možnost vám umožňuje procházet data vložená aplikací do tabulky.

    ![][12]

## <a name="next-steps"> </a>Další kroky
Teď, když jste dokončili rychlé spuštění, se naučíte, jak provádět další důležité úkoly v Mobile Services:

* [Začínáme s offline synchronizací dat] Zjistěte, jak rychlý start využívá synchronizaci dat offline k vytvoření fungující a robustní aplikace.

* [Začínáme s ověřováním] Zjistěte, jak ověřovat uživatele vaší aplikace pomocí zprostředkovatele identity.

* [Začínáme s nabízenými oznámeními] Podívejte se, jak do aplikace odeslat velmi základní nabízené oznámení.



[AZURE.INCLUDE [app-service-disqus-feedback-slug](../../includes/app-service-disqus-feedback-slug.md)]

<!-- Anchors. -->
[Začínáme používat Mobile Services]:#getting-started
[Vytvoření nové mobilní služby]:#create-new-service
[Definování instance mobilních služeb]:#define-mobile-service-instance
[Další kroky]:#next-steps

<!-- Images. -->
[0]: ./media/partner-xamarin-mobile-services-android-get-started/mobile-quickstart-completed-android.png
[2]: ./media/partner-xamarin-mobile-services-android-get-started/mobile-create.png
[3]: ./media/partner-xamarin-mobile-services-android-get-started/mobile-create-page1.png
[4]: ./media/partner-xamarin-mobile-services-android-get-started/mobile-create-page2.png
[5]: ./media/partner-xamarin-mobile-services-android-get-started/obile-services-selection.png
[6]: ./media/partner-xamarin-mobile-services-android-get-started/mobile-portal-quickstart-xamarin-android.png
[7]: ./media/partner-xamarin-mobile-services-android-get-started/mobile-quickstart-steps-xamarin-android.png
[8]: ./media/partner-xamarin-mobile-services-android-get-started/mobile-xamarin-project-android-xs.png
[9]: ./media/partner-xamarin-mobile-services-android-get-started/mobile-xamarin-project-android-vs.png
[10]: ./media/partner-xamarin-mobile-services-android-get-started/mobile-quickstart-startup-android.png
[11]: ./media/partner-xamarin-mobile-services-android-get-started/mobile-data-tab.png
[12]: ./media/partner-xamarin-mobile-services-android-get-started/mobile-data-browse.png
[13]: ./media/partner-xamarin-mobile-services-android-get-started/mobile-services-diagram.png


<!-- URLs. -->
[Začínáme s daty]: /develop/mobile/tutorials/get-started-with-data-xamarin-android
[Začínáme s offline synchronizací dat]: mobile-services-xamarin-android-get-started-offline-data.md
[Začínáme s ověřováním]: /develop/mobile/tutorials/get-started-with-users-xamarin-android
[Začínáme s nabízenými oznámeními]: /develop/mobile/tutorials/get-started-with-push-xamarin-android
[Mobile Services Android SDK]: https://go.microsoft.com/fwLink/p/?LinkID=266533
[Azure]: http://azure.microsoft.com/
[portál Azure Classic]: https://manage.windowsazure.com/




<!---HONumber=Aug16_HO4-->


