
---
title: Začínáme s Azure Mobile Services pro aplikace Android
description: Využijte tento kurz a začněte využívat Azure Mobile Services pro vývoj pro Android.
services: mobile-services
documentationcenter: android
author: RickSaling
manager: erikre
editor: ''

ms.service: mobile-services
ms.workload: mobile
ms.tgt_pltfrm: mobile-android
ms.devlang: java
ms.topic: get-started-article
ms.date: 07/21/2016
ms.author: ricksal

---
# <a name="getting-started"> </a>Začínáme používat mobilní služby
[!INCLUDE [mobile-services-selector-get-started](../../includes/mobile-services-selector-get-started.md)]

&nbsp;

[!INCLUDE [mobile-service-note-mobile-apps](../../includes/mobile-services-note-mobile-apps.md)]

> Ekvivalentní verzi mobilní aplikace tohoto tématu naleznete v části [Vytvoření aplikace pro Android v Azure Mobile Apps](../app-service-mobile/app-service-mobile-android-get-started.md).
> 
> 

V tomto kurzu se dozvíte, jak přidat cloudovou back-end službu do mobilní aplikace Android pomocí Azure Mobile Services. V tomto návodu vytvoříte novou mobilní službu a jednoduchou aplikaci *Seznam úkolů*, která ukládá data aplikací v rámci nové mobilní služby. Mobilní služby, které vytvoříte, využívají podporované jazyky .NET pomocí sady Visual Studio pro serverovou obchodní logiku a ke správě mobilních služeb. Vytvoření mobilní služby, který umožňuje zapisovat serverovou obchodní logiku v jazyce JavaScript naleznete v části [Back-end verze jazyka JavaScript](mobile-services-android-get-started.md) v tomto tématu.

Zde je snímek obrazovky dokončené aplikace:

![](./media/mobile-services-dotnet-backend-android-get-started/mobile-quickstart-completed-android.png)

K dokončení tohoto kurzu budete potřebovat [vývojářské nástroje pro Android][Android Studio], které zahrnují integrované vývojové prostředí Android Studio a nejnovější platformu Android. Je požadována verze Android 4.2 nebo novější.

Stažený projekt rychlého spuštění obsahuje Mobile Services SDK pro Android.

> [!IMPORTANT]
> K dokončení tohoto kurzu potřebujete mít účet Azure. Pokud účet nemáte, můžete si zaregistrovat zkušební verzi Azure a získat až 10 bezplatných mobilních služeb, které můžete používat i po skončení zkušebního období. Podrobnosti najdete v tématu [Bezplatná zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=AE564AB28).
> 
> 

## <a name="create-new-service"> </a>Vytvoření nové mobilní služby
[!INCLUDE [mobile-services-dotnet-backend-create-new-service](../../includes/mobile-services-dotnet-backend-create-new-service.md)]

## Stáhnout mobilní službu do místního počítače
Teď, když jste vytvořili mobilní službu, si stáhněte projekt individuální mobilní služby, který můžete spustit v místním počítači nebo virtuálním počítači.

1. Klikněte na mobilní službu, kterou jste právě vytvořili a pak na kartě Rychlé spuštění klikněte na tlačítko **Android** pod položkou **Vybrat platformu** a rozbalte možnost **Vytvořit novou aplikaci Android**.
   
    ![][1]
2. Pokud jste tak ještě neučinili, stáhněte a nainstalujte [Visual Studio Professional 2013](https://go.microsoft.com/fwLink/p/?LinkID=391934) nebo novější verzi.
3. V kroku 2 klikněte na tlačítko **Stáhnout** pod položkou **Stáhnout a publikovat službu do cloudu**.
   
    Stáhněte tak projekt Visual Studio, který implementuje mobilní služby. Uložte komprimovaný soubor projektu do místního počítače a poznamenejte si, kam jste jej uložili.

## Testování mobilní služby
[!INCLUDE [mobile-services-dotnet-backend-test-local-service](../../includes/mobile-services-dotnet-backend-test-local-service.md)]

## Publikování vaši mobilní služby
[!INCLUDE [mobile-services-dotnet-backend-publish-service](../../includes/mobile-services-dotnet-backend-publish-service.md)]

## Vytvoření nové aplikace Android
V této části vytvoříte novou aplikaci pro Android, která je připojena k mobilní službě.

1. Na [portál Azure Classic] klikněte na tlačítko **Mobile Services** a pak klikněte na mobilní službu, kterou jste právě vytvořili.
2. Na kartě Rychlé spuštění klikněte na tlačítko **Android** pod položkou **Vybrat platformu** a rozbalte možnost **Vytvořit novou aplikaci Android**.
   
    ![][2]
3. Pokud jste tak ještě neučinili, stáhněte a nainstalujte nástroje [Android Developer Tools][Android SDK] do svého místního počítače nebo virtuálního počítače.
4. V části **Stažení a spuštění aplikace** klikněte na tlačítko **Stáhnout**.
   
    Tato akce stáhne projekt pro vzorovou aplikaci *Seznam úkolů*, která je připojena k mobilní službě. Uložte komprimovaný soubor projektu do místního počítače a poznamenejte si, kam jste jej uložili.

## Spusťte svoji aplikaci pro Android
[!INCLUDE [mobile-services-run-your-app](../../includes/mobile-services-android-get-started.md)]

## <a name="next-steps"> </a>Další kroky
Teď, když jste dokončili rychlé spuštění, se naučíte, jak provádět další důležité úkoly v Mobile Services:

* [Přidání nabízených oznámení do aplikace]
  <br/>Podívejte se, jak do aplikace odeslat velmi základní nabízené oznámení.
* [Přidání ověřování do aplikace]
  <br/>Zjistěte, jak omezit přístup ke svým back-end datům pro konkrétní registrované uživatele vaší aplikace.
* [Poradce při potížích s .NET back-end pro Mobile Services]
  <br/> Další informace, jak diagnostikovat a opravit problémy, které mohou nastat u .NET back-end pro Mobile Services.

<!-- Anchors. -->
[Začínáme používat Mobile Services]:#getting-started
[Vytvoření nové mobilní služby]:#create-new-service
[Definování instance mobilních služeb]:#define-mobile-service-instance
[Další kroky]:#next-steps

<!-- Images. -->
[0]: ./media/mobile-services-dotnet-backend-android-get-started/mobile-quickstart-completed-android.png
[1]: ./media/mobile-services-dotnet-backend-android-get-started/mobile-quickstart-steps-vs-AS.png
[2]: ./media/mobile-services-dotnet-backend-android-get-started/mobile-quickstart-steps-android-AS.png


[6]: ./media/mobile-services-dotnet-backend-android-get-started/mobile-portal-quickstart-android.png
[7]: ./media/mobile-services-dotnet-backend-android-get-started/mobile-quickstart-steps-android.png
[8]: ./media/mobile-services-dotnet-backend-android-get-started/mobile-eclipse-quickstart.png

[10]: ./media/mobile-services-dotnet-backend-android-get-started/mobile-quickstart-startup-android.png
[11]: ./media/mobile-services-dotnet-backend-android-get-started/mobile-data-tab.png
[12]: ./media/mobile-services-dotnet-backend-android-get-started/mobile-data-browse.png

[14]: ./media/mobile-services-dotnet-backend-android-get-started/mobile-services-import-android-workspace.png
[15]: ./media/mobile-services-dotnet-backend-android-get-started/mobile-services-import-android-project.png

<!-- URLs. -->
[Začínáme (Eclipse)]: mobile-services-dotnet-backend-android-get-started-ec.md
[Přidání nabízených oznámení do aplikace]: mobile-services-dotnet-backend-android-get-started-push.md
[Přidání ověřování do aplikace]: mobile-services-dotnet-backend-android-get-started-auth.md
[Android SDK]: https://go.microsoft.com/fwLink/p/?LinkID=280125
[Android Studio]: https://developer.android.com/sdk/index.html
[Mobile Services Android SDK]: https://go.microsoft.com/fwLink/p/?LinkID=266533
[Poradce při potížích s .NET back-end pro Mobile Services]: mobile-services-dotnet-backend-how-to-troubleshoot.md

[portál Azure Classic]: https://manage.windowsazure.com/



<!---HONumber=Aug16_HO4-->


