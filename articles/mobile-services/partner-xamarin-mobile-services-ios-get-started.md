---
title: Začínáme s Mobile Services pro aplikace Xamarin iOS | Microsoft Docs
description: V tomto kurzu začnete používat Azure Mobile Services pro vývoj na platformě Xamarin iOS.
services: mobile-services
documentationcenter: xamarin
author: conceptdev
manager: dwrede
editor: ''

ms.service: mobile-services
ms.workload: mobile
ms.tgt_pltfrm: mobile-xamarin-ios
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 07/21/2016
ms.author: craig.dunn@xamarin.com

---
# <a name="getting-started"> </a>Začínáme používat mobilní služby
[!INCLUDE [mobile-services-selector-get-started](../../includes/mobile-services-selector-get-started.md)]

&nbsp;

[!INCLUDE [mobile-service-note-mobile-apps](../../includes/mobile-services-note-mobile-apps.md)]

> Ekvivalentní verzi tohoto tématu pro Mobile Apps naleznete v části [Vytvoření aplikace Xamarin.iOS](../app-service-mobile/app-service-mobile-xamarin-ios-get-started.md).
> 
> 

V tomto kurzu se dozvíte, jak přidat cloudovou back-end službu do mobilní aplikace Xamarin iOS pomocí Azure Mobile Services. V tomto návodu vytvoříte novou mobilní službu a jednoduchou aplikaci *Seznam úkolů*, která ukládá data aplikací v rámci nové mobilní služby.

Pokud upřednostňujete video, klip níže postupuje stejně jako tento kurz.

Video: „Začínáme s Xamarin a Azure Mobile Services“ od Craiga Dunna, věrného vývojáře pro Xamarin (délka: 10:05 min.)

> [!VIDEO https://channel9.msdn.com/Series/Windows-Azure-Mobile-Services/Getting-Started-with-Xamarin-and-Windows-Azure-Mobile-Services/player]
> 
> 

Zde je snímek obrazovky dokončené aplikace:

![][0]

Dokončení tohoto kurzu vyžaduje XCode a Xamarin Studio pro OS X nebo Visual Studio v systému Windows se síťovým počítačem Mac. Úplné instalační pokyny najdete v tématu [Nastavení a instalace pro Visual Studio a Xamarin](https://msdn.microsoft.com/library/mt613162.aspx). 

> [!IMPORTANT]
> K dokončení tohoto kurzu potřebujete mít účet Azure. Pokud účet nemáte, můžete si zaregistrovat zkušební verzi Azure a získat až 10 bezplatných mobilních služeb, které můžete používat i po skončení zkušebního období. Podrobnosti najdete v článku [Bezplatná zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/).
> 
> 

## <a name="create-new-service"> </a>Vytvoření nové mobilní služby
[!INCLUDE [mobile-services-create-new-service](../../includes/mobile-services-create-new-service.md)]

## Vytvoření nové aplikace Xamarin.iOS
Jakmile vytvoříte mobilní službu, můžete snadno využít postup rychlého spuštění portálu Azure Classic k vytvoření nové aplikace, nebo úpravě stávající aplikace pro připojení k mobilní službě.

V této části vytvoříte novou aplikaci pro Xamarin.iOS, která je připojena k mobilní službě.

1. Na [portál Azure Classic] klikněte na tlačítko **Mobile Services** a pak klikněte na mobilní službu, kterou jste právě vytvořili.
2. Na kartě Rychlé spuštění klikněte na tlačítko **Xamarin.iOS** pod položkou **Vybrat platformu** a rozbalte možnost **Vytvořit novou aplikaci Xamarin.iOS**.
   
    ![][6]
   
    Zobrazí se tří jednoduché kroky k vytvoření aplikace pro Xamarin.iOS připojené k mobilní službě.
   
    ![][7]
3. Pokud jste tak ještě neučinili, stáhněte a nainstalujte Xcode (doporučujeme nejnovější verzi Xcode 6.0 nebo novější) a [Xamarin Studio].
4. Klikněte na tlačítko **Vytvořit tabulku TodoItem** a vytvořte tabulku pro ukládání dat aplikací.
5. V části **Stažení a spuštění aplikace** klikněte na tlačítko **Stáhnout**.
   
    Tato akce stáhne projekt pro vzorovou aplikaci *Seznam úkolů*, která je připojena k mobilní službě a odkazuje na komponentu Azure Mobile Services pro Xamarin.iOS. Uložte komprimovaný soubor projektu do místního počítače a poznamenejte si, kam jste jej uložili.

## Spuštění nové aplikace Xamarin.iOS
Závěrečná fáze tohoto kurzu je sestavení a spuštění nové aplikace.

1. Přejděte do umístění, kam jste uložili komprimované soubory projektu, rozbalte soubory do počítače a otevřete soubor řešení **XamarinTodoQuickStart.iOS.sln** pomocí aplikace Xamarin Studio nebo Visual Studio.
   
    ![][8]
   
    ![][9]
2. Stiskněte tlačítko **Spustit** pro sestavení projektu a spusťte aplikaci v emulátoru iPhonu, který je výchozí pro tento projekt.
3. V aplikaci zadejte smysluplný text, například *Dokončit kurz* a pak klikněte na ikonu plus (**+**).
   
    ![][10]
   
    Tím se odešle požadavek POST do nové mobilní služby hostované v Azure. Data z požadavku se vloží do tabulky TodoItem. Položky uložené v tabulce se vrátí pomocí mobilní služby a v seznamu se zobrazí data.
   
   > [!NOTE]
   > Na kód, který přistupuje k mobilní službě pro dotazování a vkládání dat, se můžete podívat v souboru C# ToDoService.cs.
   > 
   > 
4. Zpět v [portál Azure Classic] klikněte na kartu **Data** a pak klikněte na tabulku **TodoItems**.
   
    ![][11]
   
    Tato možnost vám umožňuje procházet data vložená aplikací do tabulky.
   
    ![][12]

## Další kroky
Teď, když jste dokončili rychlé spuštění, se naučíte, jak provádět další důležité úkoly v Mobile Services:

* [Začínáme s offline synchronizací dat] Zjistěte, jak rychlý start využívá synchronizaci dat offline k vytvoření fungující a robustní aplikace.
* [Začínáme s ověřováním] Zjistěte, jak ověřovat uživatele vaší aplikace pomocí zprostředkovatele identity.
* [Začínáme s nabízenými oznámeními] Podívejte se, jak do aplikace odeslat velmi základní nabízené oznámení.

[!INCLUDE [app-service-disqus-feedback-slug](../../includes/app-service-disqus-feedback-slug.md)]

<!-- Anchors. -->
[Začínáme používat Mobile Services]:#getting-started
[Vytvoření nové mobilní služby]:#create-new-service
[Definování instance mobilních služeb]:#define-mobile-service-instance
[Další kroky]:#next-steps

<!-- Images. -->
[0]: ./media/partner-xamarin-mobile-services-ios-get-started/mobile-quickstart-completed-ios.png
[6]: ./media/partner-xamarin-mobile-services-ios-get-started/mobile-portal-quickstart-xamarin-ios.png
[7]: ./media/partner-xamarin-mobile-services-ios-get-started/mobile-quickstart-steps-xamarin-ios.png
[8]: ./media/partner-xamarin-mobile-services-ios-get-started/mobile-xamarin-project-ios-xs.png
[9]: ./media/partner-xamarin-mobile-services-ios-get-started/mobile-xamarin-project-ios-vs.png
[10]: ./media/partner-xamarin-mobile-services-ios-get-started/mobile-quickstart-startup-ios.png
[11]: ./media/partner-xamarin-mobile-services-ios-get-started/mobile-data-tab.png
[12]: ./media/partner-xamarin-mobile-services-ios-get-started/mobile-data-browse.png


<!-- URLs. -->
[Začínáme s offline synchronizací dat]: mobile-services-xamarin-ios-get-started-offline-data.md
[Začínáme s ověřováním]: partner-xamarin-mobile-services-ios-get-started-users.md
[Začínáme s nabízenými oznámeními]: partner-xamarin-mobile-services-ios-get-started-push.md

[Xamarin Studio]: http://xamarin.com/download
[Mobile Services iOS SDK]: https://go.microsoft.com/fwLink/p/?LinkID=266533

[portál Azure Classic]: https://manage.windowsazure.com/



<!---HONumber=Aug16_HO4-->


